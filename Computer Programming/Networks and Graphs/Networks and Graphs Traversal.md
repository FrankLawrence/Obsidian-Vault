---
Tags: 
Created: 2023-04-25 00:55:30
---
(Links:: [[Networks and Graphs]])
# Euler tours
## Constructing an Euler tour
- A $[u,v]$-walk in a graph $G$ is **closed** if $u=v$
- A **tour** is a [[Networks and Graphs Foundations#^94b341|closed walk]] that traverses **all edges**. A **trail** is a **walk** that traverses **each edge at most once**
- An **Euler tour** is a tour that traverses **each edge exactly once**. An **Euler trail** is a trail that traverses **each edge exactly once** (doesn't have to end at the beginning vertex)

> [!question]- Every Euler tour is also an Euler trail
> True

> [!info] THEOREM
> A connected graph $G$ has an Euler tour iff each of its vertices has an even degree
> > [!info] Proof
> > Let $T$ be an Euler tour in $G$ starting and ending in some vertex $u\in G$
> > - Every time $T$ leaves $u$ through some edge, it returns to $u$ through another edge
> > - Every time $T$ visits a vertex $v\neq u$ through some edge, it leaves $v$ through another edge
> > - Since each edge is traversed exactly once, these two facts yield a "pairing of edges" at each vertex $w$. So, $\delta(w)$ is even
> >   
> > **Base case**: $K_3$
> > **Induction hypothesis**: Assume the theorem holds for all graphs with less than $m$ edges
> > **Inductive step**: Show that the theorem holds for all graphs with $m$ edges
> > Take an arbitrary graph $G$ with $m$ edges. Graph $G$ has a cycle, since a graph where the degree of each vertex is at least 2 contains a cycle.
> > - $C$ is a cycle of $G$ and graph $G-C$ consists of a set of components connected by $C$
> > - Each vertex of $G-C$ has still an even degree 
> >   => Each component $\sigma$ of $G-C$ has an Euler tour, by induction hypothesis as each of them has less than $m$ edges
> > 
> > The Euler tour in $G$ can be made as follows. Start from an arbitrary vertex $u\in C$, walk on $C$ until hit the first component of $G-C$. Then, "do an Euler tour" in the component, continue walking on $C$ until hit the second component of $G-C$

> [!info] THEOREM
> A connected graph $G$ with at least 2 vertices has an (non-closed) Euler trail iff it has exactly two vertices of odd degree. Moreover, the trail starts and ends in the vertices of odd degree
> > [!info] Proof
> > Let $P$ be an Euler trail starting at $u$ and ending at $v$. 
> > - All vertices except $u$ and $v$ must be of even degree
> > - Both $u$ and $v$ must be of odd degree
> > 
> > If $G$ has exactly two vertices $u$ and $v$ of odd degree, then we can make a new graph $G^*$ by adding the edge $(u,v)$ to $G$
> > - The new graph $G^*$ has an Euler tour
> > - By removinng $(u,v)$ we are left with an Euler trail in $G$


## Fleury's Algorithm
- all vertices in graph $G$ have an even degree
- arbitrarily select a vertex $u \in G$ and set $T=\{u\}$
- While $E(T)\neq E(G)$, do the following:
	- Let $v$ be the last vertex added to $T$. Choose an edge $\langle v,w\rangle$ in $G-E(T)$ and add it to $T$
	- If $\langle v,w\rangle$ is a cut edge in $G-E(T)$, then it will be selected if there is no other option
## Heirholzer's Algorithm
- all vertices in graph $G$ have an even degree
1. Start from an arbitrary vertex $u$ in $G$, and build a trail $P$ until no new edge can be traversed
2. If $G-E(P)\neq \emptyset$, then remove $P$ from $G$ and repeat Step 1
3. If $G-E(P)=\emptyset$, then "merge" the trails found in Step 1 to get a tour of $G$
- To obtain an Euler tour, start from $P_1$ and follow in until hit $P_2$. Switch to $P_2$ and finish it and then continue $P_1$. Repeat for other trails
## Summary
- Fleury’s algorithm is elegant but inefficient (i.e., slow) because determining whether an edge is a cut edge or not is expensive (i.e., time consuming)
- Hierholzer’s algorithm is much more efficient than Fleury’s algorithm and it first computes a set of trails in G and then merges them to obtain an Euler tour in G
## The Chinese postman problem
- find a closed walk in the city such that:
	- each street is passed **at least once**
	- the total traveled distance is minimum
- if each vertex has an even degree find an Euler tour in $G$, using Fleury's or Hierholzer's algorithm
- graph $G$ has vertices of odd degrees
	- add some new edges into $G$ in order
	- make a new graph $G_2$ in which all vertices have even degrees
	- find an Euler tour in $G_2$
- "**pair**" each odd-degree vertex $u$ in $G$ with exactly one other odd-degree vertex $v$ in $G$
	- duplicate a shortest $[u,v]$-path in $G$

> [!info] Algorithm Chinese Postman Problem
> $v_1,v_2,...,v_{2r}$ are the **odd-degree vertices** in $G$ for $r \geq 0$
> 1. For each $v_i$ and $v_j$, find a **$[v_i,v_j]$-path $P_{ij}$ of minimum weight $w_{ij}$**. Let $e_{ij}$ be a **new edge between $v_i$ and $v_j$ with weight $w_{ij}$**
> 2. Find a **set $E^*$ of $r$ edges** in $E=\{e_{ij}|1\leq i \leq j\leq 2r\}$ such that:
> 	- no two different edges in $E^*$ share a vertex, and
> 	- the total weight of these $r$ edges is minimum
> 3. For each **edge $e_{ij}$ in $E^*$, duplicate the edges on the path $P_{ij}$ in $G$**
> 4. Compute an Euler tour in the resulting graph
# Hamilton cycles
- A Hamilton cycle in $G$ contains **every vertex** of $G$ **exactly once**
- There is no efficient algorithm to determine whether a given graph is Hamiltonian or not

> [!info] THEOREM
> If a graph $G$ is a Hamiltonian graph, then for any nonempty subset $V^*\subseteq V(G)$, we have that $\omega(G-V^*)\leq|V^*|$

> [!info] THEOREM (Dirac)
> If $G$ is a connected graph with $n\geq 3$ vertices in which $\forall v\in G$ we have that $\delta(v)\geq n/2$, then $G$ is Hamiltonian

## Finding a Hamilton cycle
> [!info] Posa's Algorithm
> 1. Pick a random vertex $u\in G$, and set the path $P=[u]$
> 2. Let $w$ be the last vertex added to $P$. While $P$ is not a Hamilton cycle do:
> 	1. If possible, select a vertex $y\in N(w)-V(P)$ and add it to the end of $P$
> 	2. If not, pick a $v\in N(w)$ randomly. Since $v$ lies on $P$, $P$ has an edge $\langle v,x\rangle$. Apply **rotational transformation** on $P$
> 
> ![[Posa's algorithm.png|500]]
> - (a) --> (b) rotational transformation after selecting vertex 4 ($w=6, v=4$)
## Traveling Salesman Problem
- Given a list of cities and the distances between each pair of cities, what is the shortest possible route that vists each city exactly once and returns to the origin city?
- Find a shortest ("best") Hamilton cycle in a **complete weighted graph** 

---
References: