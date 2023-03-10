---
Tags: lecture
Created: 2022-12-17 17:12:33
---
(Links:: [[Data Structures and Algorithms for CS]])
# Rod cutting dynamic programming algorithm
- [[Data Structures and Algorithms for CS Lecture 11#Rod cutting| Rod cutting, recursive algorithm]]
- each subproblem is solved *once*, saving it's solution
	- If we refer to this subproblem's solution later, we look it up, rather than recompute
	  -> **use additional memory to save computation time** (*time-memory trade-off*)
```cpp
Algorithm rodCuttingDP(p,n):
	new array b[0...n]
	b[0] := 0
	for j := 1 to n do
		q := -∞
		for i := 1 to j do
			q := max(q,p[i] + b[j - i])
		b[j] := q
	return b[n]
```
- worst-case time complexity in $O(n^2)$
- gives not only revenue but also choice for where to cut
- order the pieces in increasing length
# Max-subarray
- give the maximum of the sum of the elements of a sub-array
## Algorithm in $O(n^2)$
```cpp
Algorithm maxSubArray(A,n):
	max := 0
	for left := 1 to n do
		sum := 0
		for right := left to n do
			sum := sum + A[right]
			if sum > max then
				max := sum
	return max
```
## Algorithm in $O(n)$
- **idea for B[r]**: maximal sum of subarray ending at index r 
- *Start*: $B[1] = max\{A[1],0\}$
```cpp
Algorithm maxSubArray(A,n):
	new array B
	B[1] := max(A[1],0)
	m := B[1]
	for r = 2 to n do 
		B[r] := max(0,B[r-1] + A[r])
		m := max(m, B[r])
	return m
```
# Knapsack
##  Problem
- *given:* 
	- a set $S$ with $n$ items
	- every item $i$ has weight $w_i$ and benefit $b_i$
	- maximum total wieght $W$
- *goal:*
	- take subset of items $T \subseteq S$ such that **$\sum_{i\in T}b_i$ maximal** under constraint **$\sum_{i\in T}w_i\leq W$**
## Idea algorithm
- $S_k$ contains elements $1,...,k$ (for $k \in\{1,...,n\}$)
- $B[k,w]$ value of best selection from $S_k$ with total weight $\leq w$
- if $w_k > w$: we cannot take item $k$
  -> $B[k,w] = B[k-1,w]$
- if $w_k \leq w$: we may take item $k$, but it is not necessarily the best option $B[k,w]=max\{B[k-1,w],B[k-1,w-w_k]+b_k\}$
___
- S consists of $n$ items with $b_i$ and $w_i$; $W$ is the max total weight
```cpp
Algorithm 01Knapsack(S,W):
	new B[0...n,0...W]
	for w := 0 to W do
		B[0,w] := 0
	for k := 1 to n do
		B[k,0] := 0
		for w := 1 to W do
			if w_k ≤ w then
				B[k,w] := max(B[k-1,w],B[k-1,w-w_k]+b_k)
			else
				B[k,w] := B[k-1,w]
```
## Time complexity
- worst-case time complexity in $O(nW)$ (size of the table)
- **pseudo-polynomial algorithm**
# Longset common subsequence
## Brute-force
- consider every subsequence of $X = \langle x_1,..., x_m\rangle$
- check whether it is also a subsequence of $Y=\langle y_1,...,y_m\rangle$
- keep track of the longest success found
- $2^m$ subsequences
## Dynamic programming algorithm
- $\Theta (m\cdot n)$ with $m$ and $n$ length of the sequences
- remember when $C[i,j] := C[i-1,j-1] + 1$ is called to trace back the sequence of letters
```cpp
Algorithm LCS(X,Y):
	new array C[0...m,0...n]
	for i := 1 to m do
		C[i,0] := 0
	for j := 0 to n do
		C[0,j] := 0
	for i := 1 to m do
		for j := 1 to n do
			if x_i = y_i then
				C[i,j] := C[i-1,j-1] + 1
			else
				C[i,j] := max{C[i,j-1],C[i-1,j]}
	return C
```


---
References: