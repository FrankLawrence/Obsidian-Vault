---
Tags: 
Created: 2022-11-11 16:08:27
---
(Links:: [[Data Structures and Algorithms for CS]])
# Divide-and-conquer
- *recursive*
	- break the problem into several subproblems that are similar to the original problem but smaller in size, solve the subproblems recursively, and then combine these solutions to create a solution to the original problem
	- *Divide* the problem into a number of subproblems that are smaller instances of the same problem
	- *Conquer* the subproblems by solving them recursively. If the subproblems sizes are small enough, however, just solve the subproblems in a striaghtforward manner.
	- *Combine* the solutions to the subproblems into the solution for the original problem
	
## Merge sort
Our MERGE procedure takes time $\Theta(n)$, where $n = r - p + 1$ is the total number of elements being merged
Merging takes $\Theta(n)$ time
# Merge sort algorithm
## Pseudocode
```
MERGE(A,p,q,r)
	n_1 = q - p + 1
	n_2 = r - q
	let L[1..n_1 + 1] and R[1..n_2 + 1] be new arrays
	for i = 1 to n_1
		L[i] = A[p + i - 1]
	for j = 1 to n_2
		R[j] = A[q + j]
	L[n_1 + 1] = infinity
	R[n_2 + 1] = infinity
	i = 1
	j = 1
	for k = p to r
		if L[i] <= R[j]
			A[k] = L[i]
			i = i + 1
		else A[k] = R[j]
			j = j + 1
```
**Loop invariant:**
At the start of each iteration of the **for** loop of lines 12-17, the subarray $A[p..k-1]$ contains the $k-p$ smallest elements of $L[1..n_1+1]$ and $R[1..n_2+1]$ in sorted order. Moreover, $L[i]$ and $R[j]$ are the smallest elements of their arrays that have not been copied back into $A$.
# Analyzing divide-and-conquer algorithms
- $T(n)$ is the running time on a problem of size $n$
- If the problem size is small enough e.g. $n \leq c$ for some constant $c$, we write $\Theta(1)$
- A division yields $a$ subproblems of size $1/b$
- It takes $T(n/b)$ time to solve one subproblems of size $n/b$
  -> It takes $aT(n/b)$ time to solve $a$ of them
- $D(n)$: time to divide the problem into subproblems
- $C(n)$: time to combine solutions of subproblems
$$\Rightarrow T(n) =
\begin{cases}\Theta(1)  & \text{if } n\leq c,\\
aT(n/b)+D(n)+C(n)       & \text{otherwise.x }
\end{cases}$$

---
References: