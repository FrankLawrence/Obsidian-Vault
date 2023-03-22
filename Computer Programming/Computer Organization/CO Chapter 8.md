---
Tags: 
Created: 2023-03-12 20:22:17
---
(Links:: [[Computer Organization]])
# 8.3 E yes
Describe a structure similar to that in Figure 8.10 for a 16M × 32 memory using 1M × 4 memory chips.

The structure would consists of 8 columns since to get a 32 bit number from 4 bit numbers, you would need 8. Since each memory chip holds 1M of data, we would need 16 rows of them. Since there are 4 times as many rows, and twice the amount of columns, the address would have to have 19-bits for the internal chip address, and another 8 bits to distinguish between the 128 different memory chips (168).
# 8.6 M no
| Tag | Data |
| --- | ---- |
| 054 | A03C |
|     |      |
|     |      |

| Tag | Data |
| --- | ---- |
| 054 | A03C |
| 055 | 05D9 |
|     |      |

| Tag | Data |
| --- | ---- |
| 054 | A03C |
| 055 | 05D9 |
| 057 | 10D7 |

# 8.9 M yes
Consider the cache in Example 8.3. Assume that whenever a block is to be brought from the main memory and the corresponding set in the cache is full, the new block replaces the most recently used block of this set. Derive the solution for part (b) in this case.
In this case the same blocks 0, 16, 32, 48 and 64 are placed in the cache set 0. Here however, 64 overwrites the most recently used block which is block 3 containing 48. After this pass, the set contains 0, 16, 32, 64. 
In the second pass the most recently used block will be block 2 contianing 32. This will be replaced by 48 since it had been overwriten in the previous pass, and so on. This means that on every pass one number is replaced and would have to be fetched again from memory.
On two instances however there are 2 replacements: pass 5 and 9, when 64 is replaced with 0. Since 64 is inserted in the same pass, 48 is replaced with 64.
The time without cache stays the same and we get the following imporvement factor:
$$
\begin{align}
\text{Improvement factor}&= \frac{10\times 68\times 10\uptau}{1\times 68\times 11\uptau+8\times11\uptau+2(8\times 11\uptau)}\\
&= \frac{6800}{1012}\\
&= 6,17
\end{align}$$
# 8.12 M no
Repeat Problem 8.11, assuming that each cache block consists of two 32-bit words. For part (c), use a two-way set-associative cache that uses the LRU replacement algorithm
# 8.15 E yes
Consider the two-level cache described in Example 8.4. The average access time is given in the solution to part (b) of the example as 2.24τ . What value for h1 would be needed to reduce tavg to 1.5τ , if all other parameters are the same as in the example? Can the same result be achieved by improving the hit rate of L2?
$$
\begin{align}
1.5 \uptau&=t_{avg} \\
&=h_1\times 1\uptau+(1-h_1)(0.80\times15\uptau+0.20\times100\uptau) \\
&=h_1\times 1\uptau+(1-h_1) \times 32 \uptau \\
1.5 \uptau &=h_1\times 1\uptau+32\uptau-(h_1\times 32 \uptau) \\
-30.5\uptau &= -h_1\times 32 \uptau \\
30.5\uptau &= h_1\times 32 \uptau \\
h_1 &= 0,953125
\end{align}
$$
$$
\begin{align}
1.5\uptau &= 0.96\uptau + 0.04(h_2\times 15\uptau + (1-h_2)100\uptau) \\
1.5 \uptau &= 0,96\uptau + h_2\times 0,6\uptau+(1-h_2)4\uptau \\
1,5\uptau - 0,96 \uptau &= h_2 \times 0,6\uptau + 4 \uptau - h_2 4\uptau \\
0,54\uptau - 4 \uptau &= -h_2\times 0,34 \uptau \\
-0,346 \uptau &= -h_2\times 0,34 \uptau \\
1,01764 &= h_2
\end{align}
$$
# 8.18 M yes
Give a critique of the assumption made in Example 8.1, in Section 8.7.1, that the miss penalty is the same for both read and write accesses. Consider both the write-through and write-back cases, as described in Section 8.6, in formulating your answer.
# 8.21 E yes
When a program generates a reference to a page that does not reside in the physical main memory, execution of the program is suspended until the requested page is loaded into the main memory from a disk. What difficulties might arise when an instruction in one page has an operand in a different page? What capabilities must the processor have to handle this situation?

This is a so called *page fault*. The processor must be able to decide which off the existing pages that are loaded in the main memory should be replaced with the new page. Therefore it should be able to decide which pages are currently not needed for the current program (such as LRU).

---
References: