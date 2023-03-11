---
Tags: lecture
Created: 2022-12-16 16:53:05
---
(Links:: [[Data Structures and Algorithms for CS]])
# AVL -trees
> [!definition]+ Ordering invariant
> At any node with key k in a binary search tree, all keys of the elements in the left subtree are strictly less than k, while all keys of the elements in the right subtree are strictly greater than k.

> [!definition]+ Height invariant
> At any node in the tree, the heights of the left and right subtrees differs by at most 1.

- height of a node: maximal length of a path to a leaf
- height of a tree is height of the root is maximal lenght of a path to a leaf
- height of AVL-tree with $n$ nodes in $O(\log n)$
- operations min, max, successor, predecessor, search, adding, deleting in $O(h) \;(\text{so in } O(\log n))$
- single and double rotations in $O(1)$
## Insert
- insert using algorithm for BST ($O(\log n)$)
- identify lowest unbalanced node on path from new node to root ($O(\log n)$)
- rebalance with single or double rotation ($O(1)$)
- at most 1 rebalance step is needed
=> Insertion in $O(\log n)$
## Delete
- remove node using algorithm for BST ($O(\log n)$)
- identify lowest unbalanced node on path from new node to root ($O(\log n)$)
- rebalance by considering 'heaviest child' of the unbalanced node ($O(1)$)
  if necessary, consider the next unbalanced node and iterate ($O(\log n)$)
=> Removal in $O(\log n)$
# Fibonacci numbers
## Naive recursive algorithm
- exponential-time algorithm
- recurrence equation
  $$T(n) =
\begin{cases}
1  & \text{if $n=1$ or $n=2$} \\
T(n-1) + T(n-2) + 2 & \text{if $n\geq 3$}
\end{cases}$$
$T(n)\in O(2^n)$
```cpp
Algorithm fib(n):
	if n = 1 or n = 2 then
		return 1
	else
		x := fib(n-1)
		y := fib(n-2)
	return x + y
```
## Bottom-up approach
- values computed are stored in an array
- indicies in array $r[1...n]$ initialized with 0
- indicies 1 and 2 initialized with 1
- values at indicies 3...n computed from previous two entries
- in $O(n)$
```cpp
Algorithm fib(n):
	new array r[1...n]
	r[1] := 1
	r[2] := 1
	for i := 3 to n do
		r[i] := r[i-1] + r[i-2]
	return r[n]
```
# Rod cutting
- rod of length n cut into pieces of length $i=1,...,n$ with price $p_i$
```cpp
Algorithm rodCuttingRec(p,n)
	if n = 0 then
		return 0
	q := -∞
	for i := 1 to n do
		q := max(q,p[i] + rodCuttingRec(p,n - i))
	return q
```
$$T(0)=1$$
$$T(n)=1+\sum_{j=0}^{n-1}T(j)$$
$$\Rightarrow T(n)=2^n$$
- [[Data Structures and Algorithms for CS Lecture 12#Rod cutting dynamic programming algorithm| Rod cutting dynamic programming algorithm]]

---
References: