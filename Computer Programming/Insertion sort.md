---
Tags: 
Created: 2022-11-03 13:43:31
---
(Links:: [[Data Structures and Algorithms for CS]])
# Insertion sort: pseudocode
```
the input is an array A of length n; arrays start at index 1

Algorithm insertionSort(A,n):
	for j:= 2 to n do
		key := A[j]
		// Insert A[j] into the sorted sequence A[1..j-1].
		i:= j-1
		while i > 0 and A[i] > key do
			A[i+1] := A[i]
			i := i-1
		A[i+1] := key
```
- The algorithm sorts the input numbers **in place**: it rearranges the numbers within the array $A$, with at most a constant number of them stored outside the array at any time. -> The input array $A$ contains the sorted sequence after the algorithm
# Time complexity
It takes roughly equal to $c_1n^2$ to sort $n$ items, where $c_1$ is a constant that does not depend on $n$.
# **Loop invariant**
```ad-important
title: definition
collapse: none
We state the properties of $A[1..j-1]$ formally as a **loop invariant**:
> At the start of each iteration of the **for** loop of lines 1-8, the subarray $A[1..j-1]$ consists of the elements originally in $A[1..j-1]$, but in sorted order.
```
- We use loop invariants to help us understand why an algorithm is correct. We must use three things about loop invariants:
	- **Initialization**: It is true prior to the first iteration of the loop
	- **Maintenance**: If it is true before an iteration of the loop, it remains true before the next iteration.
	- **Termination**: When the loop terminates, the invariant gives us a useful property that helps show that the algorithm is correct. 
	
## Loop invariant for insertion sort
- Initialization: When $j = 2$ (before the first iteration) the subarray $A[1..j-1]$ has one element which is the original element in $A[1]$ and it is sorted
  -> loop invariant holds prior to the first iteration
- Maintenance: the for loop moves all the values up until it finds the proper position for $A[j]$ 
  It inserts the value of $A[j]$
  -> the subarray $A[1..j]$ consists of the original elements, but in sorted order
- Termination: it executes until $j = n + 1$ -> the subarray $A[1..n]$ consists of the original elements in sorted order -> $A[1..n]$ is the entire array -> the array is sorted
# Analysis of insertion sort
- **running time**: number of primitive operations or "steps" executed
- $c_i$ denotes the cost for each statement
- $t_j$ denotes the number of times the *while* loop  test in line 5 is executed for that value of j
```
                                                              cost    times
for j:= 2 to n do                                             c_1     n
	key := A[j]                                               c_2     n-1
	// Insert A[j] into the sorted sequence A[1..j-1].        0       n-1
	i:= j-1                                                   c_4     n-1
	while i > 0 and A[i] > key do                             c_5     sum j=2 - n for t 
		A[i+1] := A[i]                                        c_6     sum j=2 - n for t-1
		i := i-1                                              c_7     sum j=2 - n for t-1
	A[i+1] := key                                             c_8     n-1
```
We sum the products of the *cost* and *times* columns to get the running time $T(n)$ of *INSERTION-SORT*
$$T(n) = c_1n+c_2(n-1)+c_4(n-1)+c_5\sum_{j=2}^nt_j+c_6\sum_{j=2}^n(t_j-1)+c_7\sum_{j=2}^n(t_j-1)+c_8(n-1)$$

---
References: