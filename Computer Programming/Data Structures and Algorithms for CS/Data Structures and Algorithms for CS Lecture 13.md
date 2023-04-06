---
Tags: lecture
Created: 2022-12-19 16:59:01
---
(Links:: [[Data Structures and Algorithms for CS]])
# Greedy algorithms
- **optimal substructure:** an optimal solution to the problem contains within it optimal solutions to subproblems
## Making change
- make a certain amount using a minimal number of coins
- we have infinitely many types of coins all with a certain value
- we have a every type of unlimited number of coins available
- we have $k$ types of coins $c_1 < c_2 < ... < c_k$
- $c_1 =1$
- we aim to find $x_1, ...,x_k\in \Bbb N$ such that
  $\sum_{i=1}^kx_ic_i=W$ we make exactly $W$
  $\sum_{i=1}^kx_i$ minimal with the least possible number of coins
### Greedy approach
- for some coins, for example $1<2<5$ or the euro coins, we can simplify the dynamic programming algorithm
- how to make 6 with a minimum number of coins?

| amount | best |
| ------ | ---- |
| 0      | 0    |
| 1      | 1    |
| 2      | 1    |
| 3      | 2    |
| 4      | 2    |
| 5      | 1    |
| 6      | 2    |

- note our calculations:
  best(0) = 0
  best(1) = 1 + min{a(0)} = 1
  best(2) = 1 + min{a(2 − 2), a(2 − 1)} = 1
  best(3) = 1 + min{a(3 − 2), a(3 − 1)} = 2
  best(4) = 1 + min{a(4 − 2), a(4 − 1)} = 2
  best(5) = 1 + min{a(5 − 5), a(5 − 2), a(5 − 1)} = 1
  best(6) = 1 + min{a(6 − 5), a(6 − 2), a(6 − 1)} = 2
	- so every time the choice is for the largest possible value
- simplify dynamic programming algorithm by choosing the largset possible coin value -> greedy algorithm

- we take in each step a **locally optimal choice**
- if the problem has the greedy choice property then locally optimal choices lead to a **globally optimal solution**
# Activity selection
- given:
	- set $S$ of activities $a_i$ each with start time $s_i$ and finish time $f_i$ 
	  $s_i < f_i$
- definition compatible:
	- two activities $a_i$ and $a_k$ are compatible if $f_i \leq s_k$ or $f_k \leq s_i$
- question:
	- give a maximum-size set of mutually compatible activities
___
- don't consider all $a_k$ only $a_1$: activity with **smallest finish time $f_1$** (greedy approach), then solve $S_{1j}$
```cpp
Algorithm activitySelector(s,f):
	n := s.length
	A := [a_1]
	k := 1
	for m = 2 to n do
		if s[m]≥f[k] then
			A := A u {a_m}
			k := m
```
# Fractional knapsack
- We take as much as we can from the **best benefit per weight**
- take quantity $x_i$ of all items $i$ such that $$\sum_{i\in S}b_i\cdot \frac {x_i}{w_i}$$
- take in each step as much as possible from the item $i$ with $\frac{b_i}{w_i}$ maximal -> in each step we take a greedy choice
```cpp
Algorithm fractionalKnapsack(S,W):
	for each item i in S do
		x_i := 0
		v_i := b_i/w_i
	w := 0
	while w < W do
		remove from S an item i with highest value index
		a := min{w_i, W - w}
		x_i := a
		w := w + a
		return sum_{i in S} b_i • x_i/w_i
```
# Single-source shortest path / Dijkstra's algorithm
- directed graph $G$ with positive weights $w$ and start vertex $s$ in $G$
- set $S$ contains verticies for which the weight of a shortest path has been found
```cpp
Algorithm Dijkstra(G,w,s):
	initialize(G,s)
	S := empty
	Q := G.V
	while Q != empty do
		u := extractMin(Q)
		S := S union {u}
		for each v in G.Adj[u] do
			relax(u,v,w)
```
- initialize in $\Theta(|V|)$
- init $S$ in constant time
- init $Q$: **build** priority queue using insert
- in while-loop: $|V|$ times **extract-min**
- in while-loop: $|V|$ times update of $S$
- for-loop is executed in total $|E|$ times with inside **update key** of $v$
___
- priority queue implemented as heap
	- insert, extract-min and update key in $O(\log |V|)$
	- algorithm in $O(|E|\cdot \log |V|)$
- priority queue implemented as array with $v$ at index $v$
	- extract-min teakes in the worst-case $|V|$ steps
	- algorithm in $O(|V|^2)$

# Huffman codes
- character that occurs more frequently has a shorter encoding

> [!definition]+ Prefix codes
> No codeword is the prefix of another codeword

- binary tree where left is 0, right is 1, leaves are codes
```cpp
Algorithm HuffmanCode(C):
	n := |C|
	Q := C
	for i = 1 to n - 1 do
		new node z
		z.left := x := removeMin(Q)
		z.right := y := removeMin(Q)
		z.freq := x.freq + y.freq
		insert(Q,z)
	return removeMin(Q)
```
**Complexity**
- init in $O(n)$
- for-loop takes $(n-1)\cdot \log n$
- using min-priority queue implemented using a binary min-heap in $O(n \log n)$

---
References: