# Introduction & Motivation {#sec:intro}

Creating a program to decode QR-codes is deemed easy, given libraries and the computation power of modern computers; however, it can prove difficult given the restrictions of the FPGA board without hardware optimizations. The pipeline can be structured into multiple stages: 
1. Image capture and binarization using a computed threshold
2. Component identification and grouping
3. Geometric perspective mapping and correction
4. Reed-Solomon error correction

All of the mentioned steps make extensive use of algebraic computations (especially divisions). 

## Optimizations
Initial proposed optimizations included implementation of the Reed-solomon decoding and use of an fpu library. All subsequent division in the program would be replaced with custom instructions for a given set of operations that are performed many times over (single operations take on average 6 cycles, according to the library). We did not go with either of the two methods for the following reasons: 
1. Reed-solomon encoding proved to large of change, and would provide little optimization
2. We did not need to use the precision of 64-bit floating points, but could instead use 16.16 fixed point operations instead; a much simpler implementation, as the bus only supports 32bit values, and would need multiple stages to compute 64bit values.

The goal of this project is to port a complete QR-code decoder to the OR1420-based virtual prototype (VP) and to make it run fast enough to process camera frames in real time. The changes can be summarized into:
- Removing malloc and other library calls not available on the FPGA board.
- Replacing double/float types for division and sqrt computations with a fixed-point computation on integers instead
- Implementation of a hardware Goldschmidt divider integrated as a Custom Instruction in perspective calculation workflows
- Implementation of a hardware otsu thresholding module as a streaming component (computed in parallel with pixel acquisition).

## "Software-Only" restrictions

The library (`quirc`) we went with will not work out of the box on the provided board, but instead needed significant changes to get working. Firstly, the identification stage (`identify.c`) relies heavily on *projective geometry*: perspective setup and mapping, line intersection, and Otsu thresholding all use `double` variables (which are 64-bit wide!) and `sqrt()` operations, which requires `libm`. On the OR1420 chip, every precision operation must be *emulated* in software by the `soft-fp` runtime [^1], which compiled for the board, would take many hundreds of instructions. When loaded onto the FPGA and executed, the program crashes immediately when attempting to call floating-point computations in `perspective_setup`.

Additionally, `quirc` allocates its internal data structure with a single `malloc` call sized according to the image resolution. For a $640 \times 480$ image the allocation requires many hundred kilobytes.

Because the original code cannot execute on the FPGA board directly, there is no reference to compare it to. Instead, we use the most basic floating point division implementation as the base case.

## Hardware details

The board uses a 32-bit OpenRISC processor, and it has no hardware floating-point unit and no hardware integer divider instruction available for the compiler. Therefore, all arithmetic in the QR pipeline must be implemented in software, or via custom instructions and a hardware level implementation. Additionally, the pixel clock (`pclk`) is independent of the system clock, and components using the camera module must synchronize with both for data transfers to work.

# Software Implementations

## Fixed-Point Port of `quirc`

All `double` and `float` types in `quirc` were replaced with a 32-bit Q16.16 fixed-point type (`quirc_fixed_t`). This format stores 16 integer bits and 16 fractional bits, with a range between $[-32768, 32767.99998]$ and a resolution of $\frac{1}{2^{16}} \approx 1.5 \times 10^{-5}$.

Key changes across the library:

A naive implementation would use `(int64_t)a * b >> 16`, but this generates a call to GCC's `__muldi3`. Instead, we replaced it with a custom `mul_32x32_64` function that decomposes the operands into 16-bit halves and performs four 32-bit products:

``` 
static inline int64_t mul_32x32_64(int32_t a, int32_t b) {
    int32_t  ah = a >> 16,     bh = b >> 16;
    uint32_t al = a & 0xFFFF,  bl = b & 0xFFFF;
    return ((int64_t)(ah*bh) << 32)
         + ((int64_t)(ah*(int32_t)bl) << 16)
         + ((int64_t)((int32_t)al*bh) << 16)
         + (uint32_t)(al*bl);
}
static inline quirc_fixed_t quirc_fix_mul(quirc_fixed_t a, int32_t b) {
    return (quirc_fixed_t)(mul_32x32_64(a, b) >> 16);
}
```

Furthemore, the `quirc_fix_sdiv` and `quirc_fix_udiv` calls make use of the Goldschmidt hardware CI (link later section). Some operations, such as line-intersection checks, require a higher level of precision, and are handled by `safe_div_s64`. The `sqrt()` call in the original distance calculation was replaced by a digit-by-digit Newton-style integer square root (`quirc_isqrt`) that operates on 32-bit unsigned integers.
## Static Memory Pool

The `quirc` state structure, which holds region maps, capstone lists, grid data, and data related to region coloring, was originally allocated with a single `malloc`. We added a bare-metal initialisation (`quirc_init`) that takes in the four static arrays declared in `qr-code.c`:

```
static uint8_t bm_q_buf    [16384];              // quirc control struct
static uint8_t bm_image_buf[640 * 480];          // grayscale frame
static uint8_t bm_pixels_buf[640 * 480 * 2];     // thresholded pixel map
static uint8_t bm_flood_buf[480 * 128];          // flood-fill stack
```

# Decoding Pipeline

1. Frame capture: `takeSingleImageBlocking()` triggers the OV7670 to expose one frame. The camera module converts each RGB565 pixel to 8-bit grayscale and writes it directly to the SRAM buffer pointed to by `grayscale`.

2. Otsu: While each pixel is being written to SRAM, the Otsu module inside the camera controller increments the corresponding histogram bin. When `vsync` falls (end of frame), the Otsu FSM begins its 256-iteration threshold search. 

3. CPU poll: The CPU calls `otsu_get_threshold_blocking()`, spinning on sub-command 8 of the camera CI until the hardware signals `done`, then reading the threshold via sub-command 9. In practice the wait is very short because steps 2--3 complete in $\ll 1\,\mu\text{s}$ of CPU time after the frame ends.

4. BInarization: `pixels_setup()` iterates over the $640 \times 480$ grayscale buffer and writes `PIXEL_BLACK` (1) or `PIXEL_WHITE` (0) into the separate pixel map, using the hardware Otsu threshold.

5. Capstone processing: 
	1. Identifying`finder_scan()` is called for each of the 480 rows. It slides a window across the row looking for the five-run ratio 1:1:3:1:1 that characterises a QR finder pattern. When a candidate is found, `test_capstone()` seeds a flood-fill from the surrounding region to record the capstone extent and centre.
	2. Grouping: `test_grouping()` pairs and triples capstones by projecting each candidate's centre through the perspective transform of its neighbours, searching for the right-angle relationship that identifies a valid QR corner triple.

6. Extraction & Decoding: `quirc_extract()` applies the eight-parameter projective transform (computed in `perspective_setup` and refined by `jiggle_perspective`) to map every module of the QR grid back to pixel coordinates, reading the black/white value of each cell. `quirc_decode()` then applies Reed--Solomon error correction to recover the encoded data.

7. Output: Successfully decoded payloads are printed over UART using `printf`.

# Profiling and Performance

## Profiling Infrastructure

We implemented a lightweight macro-based profiler (`profiler_macros.h`) that uses the platform's hardware performance counters. The macros `PROFILER_PUSH(name)` and `PROFILER_POP()` bracket any named region and print the elapsed CPU cycles, stall cycles, and bus-idle cycles when the region exits. The profiler supports up to 64-level nesting and is compiled in only when `PROFILE=1` is passed to `make` (which sets `-D__WITH_PROFILE`).

## Measured Results

Table 1 summarises per-stage cycle counts for a single frame containing one version-2 QR code.

|**Stage**                       |   **CPU cycles**|  **Stall cycles**|  **Bus-idle cycles**|
|--------------------------------|-----------------|------------------|---------------------|
|`otsu_get_threshold` (HW poll)  |  \[PLACEHOLDER\]|   \[PLACEHOLDER\]|      \[PLACEHOLDER\]|
|`pixels_setup`                  |  \[PLACEHOLDER\]|   \[PLACEHOLDER\]|      \[PLACEHOLDER\]|
|`finder_scan` (all rows)        |  \[PLACEHOLDER\]|   \[PLACEHOLDER\]|      \[PLACEHOLDER\]|
|`test_grouping`                 |  \[PLACEHOLDER\]|   \[PLACEHOLDER\]|      \[PLACEHOLDER\]|
|`perspective_setup`             |  \[PLACEHOLDER\]|   \[PLACEHOLDER\]|      \[PLACEHOLDER\]|
|`jiggle_perspective`            |  \[PLACEHOLDER\]|   \[PLACEHOLDER\]|      \[PLACEHOLDER\]|
|**Total `quirc_end`**           |  \[PLACEHOLDER\]|   \[PLACEHOLDER\]|      \[PLACEHOLDER\]|

Profiling results for one frame (version-2 QR code, $640\times480$, hardware divider and Otsu enabled). Values are approximate and subject to change as optimisation continues.

## Real-Time Assessment

The OV7670 in VGA mode produces frames at approximately 30 fps, giving a frame budget of $\approx 33\,$ms per frame. At a system clock of $X$ MHz (to be confirmed), this corresponds to approximately $X \times 33\,000$ available cycles. \[PLACEHOLDER: insert measured cycle totals and compare to budget.\]

Testing in simulation confirmed reliable decoding of version-2 QR codes (containing a single error-correction block). Testing with live camera output is ongoing.

N. Otsu, "A Threshold Selection Method from Gray-Level Histograms," *IEEE Transactions on Systems, Man, and Cybernetics*, vol. 9, no. 1, pp. 62--66, Jan. 1979.

R. E. Goldschmidt, "Applications of division by convergence," M.S. thesis, Dept. of Elect. Eng., MIT, Cambridge, MA, 1964.

D. Beer, "quirc: QR-code recognition library," 2010--2012. Available: <https://github.com/dlbeer/quirc>

Z. Luo, H. Jiang, F. Qiao, and Q. Wei, "FPGA Design and Implementation of Fixed-Point Fast Divider Using Goldschmidt Division Algorithm and Mitchell Multiplication Algorithm," *IEEE Access*, vol. 9, pp. 118162--118174, 2021.

[^1]: https://gcc.gnu.org/onlinedocs/gccint/Soft-float-library-routines.html
