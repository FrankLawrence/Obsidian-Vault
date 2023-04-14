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

## Degree sequence
> [!definition] Vertex Degree 
> In a graph $G$, the **degree of a vertex $v$** is the number of edges of $G$ incident to $v$, and it is denoted by $\delta(v)$.
> Loops are counted twice

> [!info] THEOREM 2.1 
> In any graph $G$, the sum of the vertex degrees is twice the number of edges: $${\sum_{v\in V(G)}\delta(v) = 2 \cdot|E(G)|}$$

- For any graph, the number of vertices with odd degree is even 

> [!definition] Degree Sequences
> The **degree sequence** of a graph $G$ is a list of the degrees of the vertices of $G$. The sequence is ordered if the numbers are in non-decreasing order.
> A sequence of numbers is called **graphic** if it is the **degree sequence of a simple graph**

- if every vertex has the same degree, the graph is called **regular**
- **k-regular** graphs have degree *k* for each vertex

> [!info] THEOREM 2.2 (Havel-Hakimi)
> Let $[s,t_1,t_2,...,t_s,d_1,d_2,...,d_n]$ be an **ordered** sequence. This sequence is graphic <==> the sequence $[t_1-1,t_2-1,...,t_s-1,d_1,d_2,...,d_n]$ is graphic

## Subgraphs
> [!definition] Subgraphs
> Given a graph $G$, a graph $H$ is a subgraph of $G$ if $V(H) \subseteq V(G)$ and $E(H) \subseteq E(G)$
> 
> Given a graph $G$ and a subset of its vertices, $V^* \subseteq V(G)$ the **subgraph induced by $V^*$** has a vertex set $V^*$ and edge set $\{\langle u,v\rangle \in E(G)|u,v \in V^*\}$. 
> 
> Given a graph $G$ and a subset of its edges, $E^* \subseteq E(G)$ the **subgraph induced by $E^*$** has a vertex set $E^*$ and edge set $\{u,v\in V(G)|\langle u,v\rangle \in E^*\}$.
> 
> The subgraph induced by $V^*$ or $E^*$ is written as $G[V^*]$ or $G[E^*]$, respectively.

> [!definition] Graph Isomorphism
> Graphs $G_1$ and $G_2$ are called **isomorphic** if there is a **one-to-one mapping** $\phi : V(G_1)\to V(G_2)$ such that:
> For each edge $\langle u,v \rangle \in E(G_1)$, there's an edge $\langle \phi(u),\phi(v)\rangle \in E(G_2)$ and vice versa. (We call $\phi(u)$ and $\phi(v)$ the corresponding vertices of $u$ and $v$ in $G_2$)
# Graph representations

## Data structures
> [!definition] Data Structures for Graphs
> Given a graph $G$, the adjacency-list of $G$ consists of an array *Adj* of $|V|$ pointers. Each points to a list of vertices. Each list corresponds to one vertex in $V$. For each $u \in V(G)$, the list *Adj[u]* contains all vertices $v$ such that there is an edge $\langle u,v\rangle$ in $E(G)$. These are the neighbors of $u$

> [!definition] Adjacency matrix
> Given a graph $G$, the adjacency matrix of $G$ is a $|V|\times |V|$ matrix $A=(A_{ij})$ such that $a_{ij}$ is the number of edges between $v_i$ and $v_j$
> $n^2$ words to store $G$ of $n$ vertices
> **Properties of the adjacency matrix of a graph $G$**
> - symmetric: $\forall u,v:A_G[u,v] = A_G[v,u]$
> - $G$ is simple $\Longleftrightarrow \forall u,v : A_G[u,v]=1$ and $A[u,u]= 0$
> - $\forall u : \sum A_G[u,v] = \delta(u)$

> [!definition] Incidence matrix
> Given a graph $G$, the incidence matrix of $G$ is a $|V|\times |E|$ matrix $M$ such that $M[i,j]$ counts the number of times that edge $e_j$ is incident with vertex $v_i$
> 
> An edge $e_j$ can only *not* be incident with vertex $v_i$, have $v_i$ as one of its end points, or form a loop joining vertex $v_i$ with itself (only values 0,1 or 2)
> **Properties of the incidence matrix of a graph $G$**
> - A graph $G$ has no loops if and only if for all $i, j,M[i,j]\leq 1$
> - The sum of all values in row $i$ is equal to the degree of vertex $v_i$ -> $\forall i:\delta(v_i)=\sum_{j=1}^mM[i,j]$
> - $\displaystyle \forall j,\sum_{i=1}^nM[i,j]=2$
## Graph isomorphism
> [!definition] Isomophism
> Consider two graphs $G=(V,E)$ and $G^*=(V^*,E^*)$. $G$ and $G^*$ are **isomorphic** if there exists a one-to-one mapping $\phi:V\to V^*$ such that for every edge $e\in E$ with $e=\langle u,v\rangle$, there is a unique edge $e^* \in E^*$ with $e^*=\langle \phi(u),\phi(v)\rangle$

> [!info] THEOREM 2.3
> If two graphs $G$ and $G^*$ are isomorphic, then their respective ordered degree sequences should be the same

# Connectivity
> [!definition] Walk, trails, paths and cycles
> Consider a graph $G$
> - A $(v_0,v_k)$**-walk** in $G$ is an alternating sequence $[v_0,e_1,v_1,e_2...v_{k-1},e_k,v_k]$ of vertices and edges from $G$ with $e_i=\langle v_{i-1},v_i\rangle$
> - In a **closed walk**, $v_0=v_k$
> - A **trail** is a walk in which all edges are distinct, a **path** is a trail in which also all vertices are distinct
> - A **cycle** is a closed trail in which all vertices except $v_0$ and $v_k$ are distinct.

> [!definition] Connected
> Two distinct vertices u and v in graph $G$ are **connected** if there exists a $(u,v)$ - path in $G$. $G$ is **connected** if all pairs of distinct vertices are connected
> > [!definition] Component
> > A subgraph $H$ of $G$ is called a **component** of $G$ if $H$ is connected and not contained in a connected subgraph of $G$ with more vertices or edges. The number of components of $G$ is denoted as $\omega(G)$

> [!definition] Cuts
> For a graph $G$ let $V^*\subset V(G)$ and $E^*\subset E(G)$
> - $V^*$ is called a **vertex cut** if $\omega(G-V^*)>\omega(G)$
> - If $V^*$ consists of a single vertex $v$, then $v$ is called a **cut vertex**
> - If $\omega(G-E^*)>\omega(G)$ then $E^*$ is called an **edge cut**
> - If $E^*$ consists of only a single edge $e$, then $e$ is known as a **cut edge**

- $\kappa(G)$: size of a minimal vertex cut for graph $G$
- $\lambda(G)$: size of a minimal edge cut for graph $G$

> [!info] THEOREM 2.4
> $\kappa(G)\geq \lambda(G)\geq min\{\delta(v)|v\in V(G)\}$

- A graph $G$ for which $\kappa(G)\geq k$ for some $k$ is said to be **k-connected**, and **k-edge-connected** if $\lambda(G)\geq k$

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
> - The minimum number of vertices in a vertex cut that disconnects $u$ and $v$ is equal to the maximum number of pairwise vertex-independent paths between $u$ to $v$
> - The minimum number of edges in an edge cut that disconnects $u$ and $v$, is equal to the maximum number of pairwise edge-independent paths between $u$ and $v$

- A graph $G$ is $k$-connected iff any two distinct vertices are connected by at least $k$ pairwise vertex-independent paths
- $G$ is $k$-edge connected iff any two distinct vertices are connected by at least $k$ pairwise edge-independent paths
- each edge of a 2-edge-connected graph lies on a cycle

> [!definition] Harary graph
> $H_{k,n}$ is a $k$-connected simple graph with $n$ vertices and with a minimal number of edges

---
References: