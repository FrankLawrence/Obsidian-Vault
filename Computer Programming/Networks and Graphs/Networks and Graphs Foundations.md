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

> [!definition] Handshaking Lemma 
> In any graph $G$, we have $\displaystyle{\sum_{v\in V(G)}\delta(v) = 2 |E(G)|}$

> [!definition] Degree Sequences
> The **degree sequence** of a graph $G$ is a list of the degrees of the vertices of $G$. The sequence is ordered if the numbers are in non-decreasing order.
> A sequence of numbers is called **graphic** if it is the **degree sequence of a simple graph**

- if every vertex has the same degree, the graph is called **regular**

> [!definition] Data Structures for Graphs
> Given a graph $G$, the adjacency-list of $G$ consists of an array *Adj* of $|V|$ pointers. Each pointes to a list of vertices. Each list corresponds to one vertex in $V$. For each $u \in V(G)$, the list *Adj[u]* contains all vertices $v$ such that there is an edge $\langle u,v\rangle$ in $E(G)$. These are the neighbors of $u$

> [!definition] Adjacency-matrix
> Given a graph $G$, the adjacency-matrix of $G$ is a $|V|\times |V|$ matrix $A=(A_{ij})$ such that $a_{ij}=$ the number of edges between $v_i$ and $v_j$
> $n^2$ words to store $G$ of $n$ vertices
> **Properties of the adjacency-matrix of a graph $G$**
> - symmetric: $\forall u,v:A_G[u,v] = A_G[v,u]$
> - $G$ is simple $\Longleftrightarrow \forall u,v : A_G[u,v]=1$ and $A[u,u]= 0$
> - $\forall u : \sum A_G[u,v] = \delta(u)$
## Subgraphs
> [!definition] Subgraphs
> Given a graph $G$, a graph $H$ is a subgraph of $G$ if $V(H) \subseteq V(G)$ and $E(H) \subseteq E(G)$
> 
> Given a graph $G$ and a subset of its vertices, $V^* \subseteq V(G)$ the **subgraph induced by $V^*$** has a vertex set $V^*$ and edge set $\{\langle u,v\rangle \in E(G)|u,v \in V^*\}$. The subgraph induced by $V^*$ is denoted by $G[V^*]$
> 
> Given a graph $G$ and a subset of its edges, $E^* \subseteq E(G)$ the **subgraph induced by $E^*$** has a vertex set $E^*$ and edge set $\{u,v\in V(G)|\langle u,v\rangle \in E^*\}$. The subgraph induced by $V^*$ is denoted by $G[E^*]$

> [!definition] Graph Isomorphism
> Graphs $G_1$ and $G_2$ are called **isomorphic** if there is a **one-to-one mapping** $\phi : V(G_1)\to V(G_2)$ such that:
> For each edge $\langle u,v \rangle \in E(G_1)$, there's an edge $\langle \phi(u),\phi(v)\rangle \in E(G_2)$ and vice versa. (We call $\phi(u)$ and $\phi(v)$ the corresponding vertices of $u$ and $v$ in $G_2$)


---
References: