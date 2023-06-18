---
Tags: 
Created: 2023-04-04 00:23:35
---
(Links:: [[Networks and Graphs]])
# Formalities
## Graphs and vertex degrees
- A graph $G$ is a mathematical structure that contains a set of *vertices* and a set of *edges*. The set of vertices of $G$ is denoted by $V(G)$ and the set of its edges is denoted be $E(G)$. Each of $G$ is an **unordered pair** of vertices

> [!example]-
> $V(G)=\{v_1,v_2,...,v_6\}$, and $E(G)=\{\langle v_1,v_2\rangle,\langle v_1,v_4\rangle,...,\langle v_3,v_6\rangle\}$
> ```mermaid
> flowchart LR
> v1((v1)) 
> v2((v2))
> v3((v3)) 
> v4((v4))
> v5((v5)) 
> v6((v6))
> v1 --- v2
> v1 --- v4
> v2 --- v4
> v2 --- v5
> v4 --- v5
> v3 --- v5
> v3 --- v6
> ```

> [!definition] Neighbor Set
> For any vertex $v \in V(G)$, the neighbor set of $v$, by $N(v)$, is the set of all vertices, except for $v$ itself, that are adjacent to $v$ $$N(v)=\{u\in V(G)-\{v\}|\langle v,u\rangle \in E(G)\}$$

> [!definition] Simple Graphs
> A graph is called a simple graph if it does not contain multiple edges between the same pair of vertices nor loops (i.e., edges from and to the same vertex)

^4cc75e

> [!definition] Vertex Degree 
> In a graph $G$, the **degree of a vertex $v$** is the number of edges of $G$ incident to $v$, and it is denoted by $\delta(v)$.
> Loops are counted twice

> [!info] THEOREM 2.1 
> In any graph $G$, the sum of the vertex degrees is twice the number of edges: $${\sum_{v\in V(G)}\delta(v) = 2 \cdot|E(G)|}$$
> > [!info] Proof
> > When we count the edges of a graph $G$ by enumerating for each vertex $v$ of $G$ the edges incident with that vertex $v$, we are counting each edge exactly twice

- For any graph, the number of vertices with odd degree is even 

## Degree sequence
> [!definition] Degree Sequences
> The **degree sequence** of a graph $G$ is a list of the degrees of the vertices of $G$. The sequence is ordered if the numbers are in descending order.
> A sequence of numbers is called **graphic** if it is the **degree sequence of a simple graph**

- if every vertex has the same degree, the graph is called **regular**
- **k-regular** graphs have degree *k* for each vertex

> [!question]- How many different ordered graphic sequences of length 3 can be constructed?
> $$4$$

> [!info] THEOREM 2.2 (Havel-Hakimi)
> Let $[s,t_1,t_2,...,t_s,d_1,d_2,...,d_n]$ be an **ordered** sequence. This sequence is graphic <==> the sequence $[t_1-1,t_2-1,...,t_s-1,d_1,d_2,...,d_n]$ is graphic

## Subgraphs
> [!definition] Subgraphs
> Given a graph $G$, a graph $H$ is a subgraph of $G$ if $V(H) \subseteq V(G)$ and $E(H) \subseteq E(G)$
> 
> Given a graph $G$ and a subset of its vertices, $V^* \subseteq V(G)$ the **subgraph induced by $V^*$** has a vertex set $V^*$ and edge set $\{\langle u,v\rangle \in E(G)|u,v \in V^*\}$. 
> 
> Given a graph $G$ and a subset of its edges, $E^* \subseteq E(G)$ the **subgraph induced by $E^*$** has an edge set $E^*$ and vertex set $\{u,v\in V(G)|\langle u,v\rangle \in E^*\}$.
> 
> The subgraph induced by $V^*$ or $E^*$ is written as $G[V^*]$ or $G[E^*]$, respectively.

# Graph representations

## Data structures
> [!definition] Adjacency list
> Given a graph $G$, the adjacency-list of $G$ consists of an array *Adj* of $|V|$ pointers. Each points to a list of vertices. Each list corresponds to one vertex in $V$. For each $u \in V(G)$, the list *Adj[u]* contains all vertices $v$ such that there is an edge $\langle u,v\rangle$ in $E(G)$. These are the neighbors of $u$.
> If each node needs one word of memory, then $n+2m$ words of memory can store $G$

^3419b1

> [!definition] [[Graphs and Trees#Adjacency matrix for unweighted graphs|Adjacency matrix]]
> Given a graph $G$, the adjacency matrix of $G$ is a $|V|\times |V|$ matrix $A=(A_{ij})$ such that $a_{ij}$ is the number of edges between $v_i$ and $v_j$
> $n^2$ words to store $G$ of $n$ vertices
> **Properties of the adjacency matrix of a graph $G$**
> - symmetric: $\forall u,v:A_G[u,v] = A_G[v,u]$
> - $G$ is simple $\Longleftrightarrow \forall u,v : A_G[u,v]=1$ and $A[u,u]= 0$
> - $\forall u : \sum A_G[u,v] = \delta(u)$

^5eab95

> [!definition] Incidence matrix
> Given a graph $G$, the incidence matrix of $G$ is a $|V|\times |E|$ matrix $M$ such that $M[i,j]$ counts the number of times that edge $e_j$ is incident with vertex $v_i$
> 
> An edge $e_j$ can only *not* be incident with vertex $v_i$, have $v_i$ as one of its end points, or form a loop joining vertex $v_i$ with itself (only values 0,1 or 2)
> **Properties of the incidence matrix of a graph $G$**
> - A graph $G$ has no loops if and only if for all $i, j,M[i,j]\leq 1$
> - The sum of all values in row $i$ is equal to the degree of vertex $v_i$ -> $\forall i:\delta(v_i)=\sum_{j=1}^mM[i,j]$
> - $\displaystyle \forall j,\sum_{i=1}^nM[i,j]=2$

> [!question]- Let $G$ be a simple and connected graph with $n$ vertices and $m$ edges. How many $0$s does the adjacency-matrix of $G$ have?
> $$n^2-2m$$ 
## Graph isomorphism

> [!definition] Isomorphism
> Graphs $G_1$ and $G_2$ are called **isomorphic** if there is a **one-to-one mapping** $\phi : V(G_1)\to V(G_2)$ such that:
> For each edge $\langle u,v \rangle \in E(G_1)$, there's an edge $\langle \phi(u),\phi(v)\rangle \in E(G_2)$ and vice versa. (We call $\phi(u)$ and $\phi(v)$ the corresponding vertices of $u$ and $v$ in $G_2$)

> [!info] THEOREM 2.3
> If two graphs $G$ and $G^*$ are isomorphic, then their respective ordered degree sequences should be the same

- Two graphs $G_1$ and $G_2$ are not isomorphic if
	- $|V(G_1)|\neq|V(G_2)|$
	- $|E(G_1)|\neq|E(G_2)|$
	- the ordered degree sequence of $G_1$ is not the same as that of $G_2$

> [!question]- Let $G$ and $H$ be two simple graphs. If $G$ and $H$ are not isomorphic, then $G$ and $H$ have different number of vertices.
> False

# Connectivity
> [!definition] Walk, trails, paths and cycles
> Consider a graph $G$
> - A $(v_0,v_k)$**-walk** in $G$ is an alternating sequence $[v_0,e_1,v_1,e_2...v_{k-1},e_k,v_k]$ of vertices and edges from $G$ with $e_i=\langle v_{i-1},v_i\rangle$
> - In a **closed walk**, $v_0=v_k$
> - A **trail** is a walk in which all edges are distinct, a **path** is a trail in which also all vertices are distinct
> - A **cycle** is a closed trail in which all vertices except $v_0$ and $v_k$ are distinct.

^94b341

> [!definition] Connected
> Two distinct vertices u and v in graph $G$ are **connected** if there exists a $(u,v)$ - path in $G$. $G$ is **connected** if all pairs of distinct vertices are connected
> Connectivity of vertices is an [[Equivalence relations|equivalence relation]]
> > [!definition] Component
> > A subgraph $H$ of $G$ is called a **component** of $G$ if $H$ is connected and not contained in a connected subgraph of $G$ with more vertices or edges. The number of components of $G$ is denoted as $\omega(G)$

^d3fe85

> [!question]- Let $G$ be a graph with $n$ vertices and $m$ edges. What are, respectively, the min and the max number of components that $G$ can have?
> $$1 \text{ and } n$$ 
> Each vertex on its own can be considered a separate component if unconnected.

> [!definition] Cuts
> For a graph $G$ let $V^*\subset V(G)$ and $E^*\subset E(G)$
> - $V^*$ is called a **vertex cut** if $\omega(G-V^*)>\omega(G)$
> - If $V^*$ consists of a single vertex $v$, then $v$ is called a **cut vertex**
> - If $\omega(G-E^*)>\omega(G)$ then $E^*$ is called an **edge cut**
> - If $E^*$ consists of only a single edge $e$, then $e$ is known as a **cut edge**

^7639ab

- $\kappa(G)$: size of a minimal vertex cut for graph $G$
- $\lambda(G)$: size of a minimal edge cut for graph $G$

> [!question]- What can be the maximum size of a vertex cut in a graph with $n$ vertices?
> $$n-2$$

> [!info] THEOREM 2.4
> $\kappa(G)\geq \lambda(G)\geq min\{\delta(v)|v\in V(G)\}$

- A graph $G$ for which $\kappa(G)\geq k$ for some $k$ is said to be **k-connected**, and **k-edge-connected** if $\lambda(G)\geq k$

> [!question]- What is the minimum degree of vertices in a $k$-connected graph with $n$ vertices?
> $$\geq k$$

> [!definition] Complete Graph
> A simple graph $G$ with $n>2$ denoted by $K_n$ is complete if the degree of each vertex in $G$ is exactly $n-1$
> > [!warning]
> > Complete graphs are the only graphs that do not have a vertex cut

> [!definition] Vertex independent
> Consider a graph $G$ and a collection $P$ of (u,v)-paths in $G$, with $u,v\in V(G)$
> - $P$ is **vertex independent** if for all (u,v)-paths $P_1,P_2\in P$ we have that $V(P_1)\cap V(P_2)=\{u,v\}$
> - The collection is **edge independent** if for all its (u,v)-paths $P_1$ and $P_2$, we have that $E(P_1)\cap E(P_2)=\emptyset$

- Two (u, v)-paths $P_1$ and $P_2$ are vertex independent if they share only the vertices $u$ and $v$, and are edge independent if they have no edge in common. 

> [!info] THEOREM 2.5 (Menger)
> Let $G$ be a connected graph and $u$ and $v$ two nonadjacent vertices in $G$
> - The minimum number of vertices in a [[#^7639ab|vertex cut]] that disconnects $u$ and $v$ is equal to the maximum number of pairwise vertex-independent paths between $u$ to $v$
> - The minimum number of edges in an [[#^7639ab|edge cut]] that disconnects $u$ and $v$, is equal to the maximum number of pairwise edge-independent paths between $u$ and $v$

^e20f09

- A graph $G$ is $k$-connected iff any two distinct vertices are connected by at least $k$ pairwise vertex-independent paths
- $G$ is $k$-edge connected iff any two distinct vertices are connected by at least $k$ pairwise edge-independent paths
- each edge of a 2-edge-connected graph lies on a cycle

> [!question]- What is the min size of an edge cut disconnecting two vertices $u,v$ in $K_n$?
> $$n-1$$

> [!question]- What is the maximum size of a set of edge-independent paths in $K_n$ between two vertices $u,v$?
> $$n-1$$

> [!question]- Why can Menger's theorem not be used to compute vertex cuts of minimum size between pairs of vertices i $K_n$?
> Because $K_n$ graphs do not have any pair of nonadjacent vertices

## Harary graph

> [!definition] Harary graph
> $H_{k,n}$ is a $k$-connected simple graph with $n$ vertices and with a minimal number of edges

- Three cases for combinations $k$ and $n$:
	- **k is even**: construct $H_{k,n}$ join vertex $i$ to $k/2$ closest left-handed and right-handed neighbors
	- **k is odd, n is even**: construct $H_{k-1,n}$ and add $n/2$ edges by joining $i$ to left-hand neighbor at distance $\frac n2$
	- **k is odd, n is odd**: construct $H_{k-1,n}$ and add $(n+1)/2$ edges $\langle 0,\frac{n-1}{2}\rangle,\langle1,1+\frac{n-1}2\rangle,...,\langle\frac{n-1}2,n-1\rangle$
- ![[Harary graphs.png|500]]
	- (a) $H_{4,8}$
	- (b) $H_{5,8}$
	- (c) $H_{4,9}$
	- (d) $H_{5,9}$

> [!info] THEOREM 2.6
> The Harary graph $H_{k,n}$ is $k$-connected

# Drawing graphs
## Graph embeddings
> [!definition] Bipartite graph
> A graph $G$ is **bipartite** if $V(G)$ can be partitioned into two disjoint subsets $V_1$ and $V_2$ such each edge $e\in E(G)$ has one point in $V_1$ and the other in $V_2$:$E(G)\subseteq \{e=\langle v,u\rangle|v\in V_1,u\in V_2\}$

- set $E^* \subset E(G)$ is called **matching** in $G$ iff no pairs of edges in $E^*$ share a vertex
- if the size of $E^*$ is maximum amoung all matchings in $G$ then it is called a maximum-size matching

- **Ranked embedding**: 
	- Pick an arbitrary vertex $v \in V(G)$
	- compute the sets $N_r(v)$ of **vertices at distance $r$ from $v$**, for all $r\geq 0$ 
		- $N_0(v)=\{v\}$
		- $N_{r+1}(v)=\{x\in n(w)|w\in N_r(v)\}-\bigcup_{j=0}^{r-1}H_j(v)$ (At each layer, we place the immediate neighbors of vertices from previous level that have not been appeared in ay lower levels)
- If vertices at the same distance have no edges between them then $G$ is bipartite

> [!question]- What can be the length (i.e. number of edges) of a shortest cycle in a simple bipartite graph?
> $$4$$

## Planar graphs

> [!definition] Plane graph
> A plane graph is a specific embedding of a graph $G$ such that no two edges intersect. If such an embedding exists, $G$ is said to be **planar**

- a planar graph partitions the plane into a (possibly empty) set of bounded regions (surrounded by a set of edges) and only one unbounded region

> [!info] THEOREM 2.7 (Euler's formula)
> For a plane graph $G$ with $n$ vertices, $m$ edges, ad $r$ regions, we have that $n-m+r=2$
> > [!info]- Proof
> > For a connected acyclic simple graph $G$ with $n$ vertices, $|E(G)|=n-1$
> > 
> > **Base case**: If $r=1$, then the graph is acyclic. -> $n-m+r=n-(n-1)+r=r+1=2$
> > 
> > **Induction hypothesis**: We assume that the Euler's theorem holds for any planar graph with at most $r$ regions, where $r>1$
> > 
> > **Inductive step**: We need to show that the Euler's theorem holds for any planar graph with $r+1$ regions
> > - $G$ has $r+1$ regions
> > - $G_2$ has $n$ vertices, $m$ edges and $r$ regions
> > - a region $r^*$ has $k$ vertices excluding the joint vertex with $G_2$ -> $r^*$ has $k+1$ edges
> > - For $G$ we have $$(n+k)-(m+k+1)+(r+1)=n-m+r=2$$
> > - everytime we add a vertex to $G$ we also increase $m$ by one -> cancels out
> > - to close a new region we have to add an edge -> cancels out

> [!info] THEOREM 2.9
> For any connected simple planar graph $G$ with $n\geq 3$ vertices and $m$ edges, we have that $m\leq 3n-6$
> > [!info]- Proof
> > $B(r)$ is the number of edges of $G$ visible from region $r$
> > - $G$ is acyclic -> $m=n-1\leq 3n-6$
> > - If $G$ is cyclic -> $B(r)\geq3$ for each region -> $\sum_rB(r)\geq 3r$
> > - Each edge is counted at most twice -> $\sum_rB(r)\leq 2m$
> > $$3r\leq \sum_rB(r)\leq 2m\Rightarrow r\leq \frac23m$$
> > - Euler's theorem: $$\begin{align}m&=n+r-2\\ m&\leq n +\frac23 m-2\\ m&\leq 3n-6\end{align}$$

> [!info] THEOREM 2.10
> Every planar graph has at least one vertex $v$ with $\delta(v)\leq 5$
> > [!info]- Proof
> > Assume $n\geq3$
> > $G$ is planar -> $m\leq 3n-6$
> > $$\sum_{v\in V(G)}\delta(v)=2m\leq 6n-12 < 6n\Rightarrow \delta(v)\leq 5$$

> [!question]- Argue that $K_5$ is not planar
> Every planar graph must satisfy $m\leq 3n-6$
> $$\sum_{v\in V(K_5)}\delta(v)=5\times 4=20=2m\to m=10$$
> $$10\not\leq 3n-6=3\times 5-6=9$$

> [!question]- Every complete graph with 4 or more vertices is non-planar
> False