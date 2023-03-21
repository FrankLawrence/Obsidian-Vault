---
Tags: 
Created: 2023-03-12 20:22:17
---
(Links:: [[Computer Organization]])
# 8.3 E yes
Describe a structure similar to that in Figure 8.10 for a 16M × 32 memory using 1M × 4 memory chips.
# 8.6 M no

# 8.9 M yes
Consider the cache in Example 8.3. Assume that whenever a block is to be brought from the main memory and the corresponding set in the cache is full, the new block replaces the most recently used block of this set. Derive the solution for part (b) in this case.
# 8.12 M no
Repeat Problem 8.11, assuming that each cache block consists of two 32-bit words. For part (c), use a two-way set-associative cache that uses the LRU replacement algorithm
# 8.15 E yes
Consider the two-level cache described in Example 8.4. The average access time is given in the solution to part (b) of the example as 2.24τ . What value for h1 would be needed to reduce tavg to 1.5τ , if all other parameters are the same as in the example? Can the same result be achieved by improving the hit rate of L2?
# 8.18 M yes
Give a critique of the assumption made in Example 8.1, in Section 8.7.1, that the miss penalty is the same for both read and write accesses. Consider both the write-through and write-back cases, as described in Section 8.6, in formulating your answer.
# 8.21 E yes
When a program generates a reference to a page that does not reside in the physical main memory, execution of the program is suspended until the requested page is loaded into the main memory from a disk. What difficulties might arise when an instruction in one page has an operand in a different page? What capabilities must the processor have to handle this situation?

---
References: