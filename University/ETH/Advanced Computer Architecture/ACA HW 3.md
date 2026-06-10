---
Tags: 
Created: 2026-05-12 21:25:16
---
(Links:: [[Advanced Computer Architecture]])

# Environment
Assume the following fixed latencies

| Cycles per operation | Memory Load/Store | Addition | Multiplication |     |
| -------------------- | ----------------- | -------- | -------------- | --- |
| Integer              | 1                 | 1        | 4              |     |
| Float                | 1                 | 3        | 5              |     |

Similar to the lab, assume every array maps to a single BRAM, meaning each array has a single memory port.
Address calculations (e.g., $2\times i$ and $2\times i+1$), as well as any operations occurring before & after the loop (such
as variable initialization), have a negligible cost and can be ignored in the questions.
You will need to explore the [Vitis HLS documentation (2025.2)](https://docs.amd.com/r/en-US/ug1399-vitis-hls) for further optimizations. Since this home-
work doesn’t use Vitis HLS, you need to specify every parameter of every pragma, including optional param-
eters.
# Designs
## Static HLS
### 1
```c
void kernel1 (int array[ARRAY_SIZE]) {
	for(int i = 0; i < ARRAY_SIZE; i++)
		array[i] = array[i] * 5;
}
```
```c
void kernel1 (int array[ARRAY_SIZE]) {
	#pragma HLS array_partition variable=array type=block factor=ARRAY_SIZE/5 dim=1
	for(int i = 0; i < ARRAY_SIZE; i++)
		#pragma HLS PIPELINE II=1 style=stp
		array[i] = array[i] * 5;
}
```
- We can unroll any amount of cycles, but for each we'd need just as many multiplication units, and we'd need to split up the memory array into that many sections as well.
### 2
```c
void kernel2 (int array[ARRAY_SIZE]) {
	for(int i = 3; i < ARRAY_SIZE; i++)
		array[i] = array[i - 1] + array[i - 2] * array[i - 3];
}
```
```c
void kernel2 (int array[ARRAY_SIZE]) {
	for(int i = 3; i < ARRAY_SIZE; i++)
		#pragma HLS PIPELINE II=1 style=stp
		array[i] = array[i - 1] + array[i - 2] * array[i - 3];
}
```
### 3
```c
void kernel3 (float hist[ARRAY_SIZE], float weight[ARRAY_SIZE], int index[ARRAY_SIZE]) {
	for(int i = 0; i < ARRAY_SIZE; ++i)
		hist[index[i]] = hist[index[i]] + weight[i];
}
```
```c
void kernel3 (float hist[ARRAY_SIZE], float weight[ARRAY_SIZE], int index[ARRAY_SIZE]) {
	for(int i = 0; i < ARRAY_SIZE; ++i)
		#pragma HLS PIPELINE II=5 style=stp
		hist[index[i]] = hist[index[i]] + weight[i];
}
```
### 4
```c
void kernel4 (int array[ARRAY_SIZE], int index[ARRAY_SIZE], int offset) {
	for(int i = offset + 1; i < ARRAY_SIZE - 1; ++i)
		array[offset] = array[offset] - index[i] * array[i] + index[i] * array[i + 1];
}
```
```c
void kernel4 (int array[ARRAY_SIZE], int index[ARRAY_SIZE], int offset) {
	#pragma HLS array_partition variable=array type=cyclic factor=2 dim=1
	int offset_val_neg = array[offset];
	int offset_val_pos = offset_val_neg;
	for(int i = offset + 1; i < ARRAY_SIZE - 1; ++i)
		#pragma HLS PIPELINE II=2 style=stp
		offset_val_neg -= index[i] * array[i];
		offset_val_pos += index[i] * array[i + 1];
	array[offset] = offset_val_neg + offset_val_pos
}
```
### 5
```c
void kernel5 (float bound, float a[ARRAY_SIZE], float b[ARRAY_SIZE]) {
	int i = 0;
	float sum = 0;

	while(sum < bound && i < ARRAY_SIZE) {
		sum = a[i] + b[i];
		i++;
	}
	
	return sum;
}
```
```c
void kernel5 (float bound, float a[ARRAY_SIZE], float b[ARRAY_SIZE]) {
	int i = 0;
	float sum = 0;

	while(sum < bound && i < ARRAY_SIZE) {
		sum = a[i] + b[i];
		i++;
	}
	
	return sum;
}
```
## Dynamic HLS
Kernel 5 contains an exit condition that depends on the runtime data. For this kernel, explain how a Dynamic HLS implementation could improve the average performance. Describe an example of data distribution where this implementation would perform significantly better than the Static HLS one, and why.

# Submission

For each design of Section 2.1 you optimize, you are required to submit the following:
- The modified code, written clearly in your submission.
- The scheduling dataflow graph of the first three iterations of the loop, in a similar format as in the lab. The x-axis must correspond to the cycle count, while the y-axis corresponds to the iteration count. Both axis must start at 0.
- The initialization interval (II), and a brief explanation why the interval is not one, if applicable.
- The total number of cycles required to finish one iteration and also the entire task.
- The total number of resources required by your design. List the number of adders, multipliers and ports required by each array.
- A short explanation of each optimization you applied, and why. If they come with downsides, also discuss them.

The homework should be solved on paper (a typed PDF document is also accepted). You do not need to
build or submit a Vitis HLS project. For designs that you think no optimization can be applied, you don’t
have to submit the modified code, but instead a reason why no further optimization is possible

---
References: