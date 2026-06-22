---
Tags:
  - Side-Channel-Attack
  - Hardware-Security
Created: 2026-06-14 22:51:14
---
# Idea
[[DRAM]] have a fundamental flaw in their physical design: Due to the need to make increasingly dense cells, *frequent activation of word lines* neighbouring particular cells between refreshes may *flip the cell states* due to various forms of capacitive coupling. This is exploited in the Rowhammer attack [^1].
Example code:
```cpp
code1a:
	mov (X), %eax // read from address X
	mov (Y), %ebx // read from address Y
	clflush (X)   // flush cache for address X
	clflush (Y)   // flush cache for address Y
	mfence
	jmp code1a
```

- `mov` instructions activate neighbouring rows
- `clflush` unprivileged x86 instructions flush the cash from the values of `X` and `Y` (so that future accesses are misses) and `mfence` roughly waits for the flush
- Repeat as quickly as possible
# Mitigations
- Error Correction Codes ([[ECC]]) may fail to detect multiple flips
- Shortening the refresh intervals mitigates but does not solve problems; implemented in firmware by some vendors
- Hard or impossible to avoid altogether without *changes in the DRAMs*
- Increase electrical noise margins (costly!)
- Count inside the DRAM the number of row activations within a time window, *identify potential victims, and refresh*
	- Introduced in DDR4 products but not in the JEDEC standard

---
References:

[^1]: M. Seaborn and T. Dullien, Exploiting the DRAM RowHammer Bug to Gain Kernel Privileges, Black Hat USA, August 2015
