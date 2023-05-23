---
Tags: 
Created: 2023-05-18 13:42:24
---
(Links:: [[Networks and Graphs]])
# Fundamentals
- A tree is a simple, connected and acyclic graph

> [!info] THEOREM
> In any simple and connected graph $G$ with $n$ vertices and $m$ edges, we have that $m\geq n-1$

> [!info] THEOREM
> Any connected graph $G$ with $n$ vertices and $n-1$ edges is a tree
> > [!info] Proof
> > $G$ has $n$ vertices and $n-1$ edges and contains a cycle with an edge $e\in E(C)$ where $G^*=G-e$
> > $G^*$ is connected and has $n$ vertices and $n-2$ edges
> > From Theorem 1: $$\begin{align}|E(G^*)|&\geq|V(G^*)|-1 \\ n-2&\geq n-1 \\ 0&\geq 1\end{align}$$
> > Thus $G$ has no cycles

> [!info] THEOREM
> A simple graph $G$ is a tree iff there exists only one $[u,v]$-path in $G$, for any pair $u,v\in V(G)$
> > [!info] Proof "if part"
> > - Let $u,v\in V(G)$ and $P$ and $Q$ be two $[u,v]$-paths in $G$
> > - $P$ and $Q$ will at one point diverge and meet again -> this forms a cycle in $G$ which is a contradiction
> 
> > [!info] Proof "only if part"
> > - Suppose $G$ contains a cycle
> > - We can assume that $G$ has at least 3 vertices
> > - $G$ has two $[v_1,v_2]$-paths:
> > 	- $[v_1,v_2]$
> > 	- $[v_1,v_{n-1},...,v_2]$
> > 
> > -> Contradiction

> [!info] THEOREM
> A simple and connected graph $G$ is a tree iff every edge of $G$ is a cut-edge
> > [!info] Proof
> > - Assume that $e=\langle u,v \rangle \in E(G)$ is not a cut-edge
> > - there exists a $[u,v]$-path in $G$ without $\langle u,v \rangle$ -> $G$ has a cycle -> contradiction
> > 
> > - $G$ is simple and connected 
> > - If $G$ has a cycle, then no edge of $C$ is a cut-edge -> contradiction
# Spanning trees
- A tree $T$ is a spanning tree of $G$ if:
	- $T$ is a subgraph of $G$
	- $V(T)=V(G)$
- $T$ is a **minimum spanning tree** if the sum of its edge weights is minumum among all spanning trees of $G$
- Let $G$ be a connected, undirected and weighted graph and $A$ be a subset of some MST of $G$
	- An edge $\langle u,v \rangle \in E(G)$ is a **safe edge for $A$** if $A\cup \{\langle u,v \rangle\}$ is also a subset of some MST of $G$
## Prim-Jarník Algorithm
- Let $G$ be a connected, undirected and weighted graph and $S$ be any subset of vertices of $G$
	- The sets $S,V-S$ is called a **cut** in $G$, and it is denoted by $(S,V-S)$
	- An edge $\langle u,v\rangle$ **crosses** the cut $(S,V-S)$ if one of its endpoints is in $S$ and the other one is in $V-S$
	- An edge is called a **light edge** crossing a cut if its weight is the minimum of any edge crossing the cut
- runs in **$O(m\log n)$** time

> [!info] THEOREM
> Let $G$ be a connected, undirected and weighted graph. Let
> - $A$ be a subset of some MST of $G$
> - $(S,V-S)$ be any cut of $G$ that does not intersect any edge of $A$
> - $\langle u,v\rangle$ be a light edge crossing $(S,V-S)$
> -> Then, the edge $\langle u,v\rangle$ is safe for $A$

```python
for each vertex u in V(G):
	u.key = ∞
r.key = 0
H = V(G) # a binary min-heap that stores V(G)
while H != Ø:
	u = Extract-Min(H)
	for each vertex v in G.Adj[u]: # for each neighbor v of u
		if v in H and w(u,v) < v.key:
			v.key = w(u,v)
```
## Kruskal's Algorithm
- A collection of trees is called a **forest**
- algorithm runs in $O(m\log n)$ time
```python
T = Ø
for each vertex u in V(G):
	Make-Tree(u)
Sort the edges of G increasingly, ordered by their weight.
for each edge ⟨u,v⟩ taken from the order:
	if Tree(u) != Tree(v):
		T = T ⋃ {⟨u,v⟩}
		UNION(Tree(u),Tree(v))
return T
```
# Clustering
- items within each set (or cluster) are "similar" to each other
- items in different sets (or clusters) are "dissimilar" to each other

> [!question] Given two documents $d_i,d_j$, can you think of a way to determine how similar or dissimilar $d_i$ and $d_j$ are?
> Distance functions describe the similarity:
> 1. $f(d_i,d_j)=500$ - the number of common words in $d_i$ and $d_j$
> 1. $g(d_i,d_j)=\frac{1}{\text{the number of common words in } d_i \text{ and } d_j}$

> [!example] Clusters of documents
> - set $S$ has 10 documents each with 500 words
> 	- 3 of them have 200-250 words in common (a) -> $250 \leq f(a,a)\leq 300$
> 	- 5 of them have 400-500 words in common (b) -> $0 \leq f(b,b)\leq 100$
> 	- 2 of them have 100-150 words in common (c) -> $350 \leq f(c,c)\leq 400$
> - every two other documents have either no words in common, or only up to 10 words in common
> 	- $f(a,b)\geq 490$
> 	- $f(b,c)\geq 490$
> 	- $f(a,c)\geq 490$
> - Let $G=(V,E)$ be a weighted complete graph such that:
> 	- $V =$ the set of documents, and
> 	- for any two vertices $u,v$ let $w(u,v)=f(u,v)$
> - Next, run Kruskal's algorithm on $G$ and force the algorithm to stop as soon as it produces $k$ components
> - Since the weight between two dissimilar documents is large ($\geq 490$), they won't be in the same cluster

# Single-Source Shortest Paths in Weighted Graphs
- The *weight* of a path $p=\langle v_0,v_1,...,v_k\rangle$ is the sum of the weights of its edges: $$w(p)=\sum_{i=0}^{k}(v_{i-1},v_i)$$
## Dijkstra's Algorithm
- $\vec G$ is a weighted directed graph with $n$ vertices and $m$ edges, and **all edge weights are nonnegative**
- runs in $O(n^2)$ time
- $O(m\log n)$ runtime when using a binary min-heap for the priority-queue
- $O(m+n\log n)$ runtime when using a Fibonacci heap for the priority-queue

1. For each vertex $v\in V$, Dijstra's algorithm maintains a value $v.d$, which is called a **shortest-path estimate**
2. Initially, the $d$ values of all vertices is set to $\infty$, except for vertex $s$. The algorithm then gradually decreases the values until htey reach to their final value, which equals the weight of the shortest path from $s$ to that vertex
3. For Step 2, the algorithm uses the technique of **relaxation**: the process of **relaxing an edge** consists of testing whether we can improve the shortest path to $v$ found so far by going through $u$

```python
for each vertex v in V(G):
	v.d = ∞ # the d values are the keys of vertices
s.d = 0
S = Ø
Q = V(G) # Q is a min-priority queue that stores all of V(G), keyed by their d values
while Q != Ø:
	u = Extract-Min(Q)
	S = S ⋃ {u}
	for each vertex v in G.Adj[u] # for each neighbor v of u:
		if u.d + w(u,v) < v.d:
			v.d = u.d + w(u,v)
```
- Array $A$ stores keys of vertices ($v_i.d$) at $A[i]$
- Construction of Q done in $O(n)$ time
- `Extract-Min` operation takes $O(n)$ time
- `Decrease-Key` operation (update value of vertex) takes $O(1)$ time
- The *while loop* executes $n$ times in total. The *for loop* executes $m$ times in total -> $=(m+n^2)=O(n^2)$ runtime
## Bellman-Ford Algorithm
```python
for each vertex v in V(G):
	v.d = ∞ # the d values are the keys of vertices
s.d = 0
for i = 1 to |V(G)| - 1:         # computes shortest path
	for each edge (u,v) in E(G):
		if u.d + w(u,v) < v.d:
			v.d = u.d + w(u,v)
for each edge (u,v) in E(G):     # detects negative-weight cycles
	if u.d + w(u,v) < v.d then
		return False
return True
```
- running time in $O(mn)$
- returns `True` iff $G$ does not contain any negative-weight cycle

> [!question]- If we run the Bellman-Ford algorithm on a graph that contains a negative-weight cycle reachable from a source vertex $s$, then which vertices will get an incorrect key in the end?
> - Those on the negative-weight cycle
> - Those reachable from any vertex on the negative-weight cycle

---
References: