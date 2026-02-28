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