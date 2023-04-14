---
Tags: 
Created: 2023-04-04 22:01:21
---
(Links:: [[Networks and Graphs]])


> [!definition] Walk
> Given a graph $G$ and two vertices $v_0, v_k$ of $G$, a **$(v_0,v_k)$-walk** in $G$ is a sequence **$[v_0,e_1,v_1,e_2,...,e_k,v_k]$** of vertices and edges such that $e_i=[v_{i-1},v_i]$ is an edge in $G$, for all $i=1,2,...k$.

> [!definition] Path
> Given a graph $G$ and two vertices $v_0,v_k$ of $G$, a **$(v_0,v_k)$-path** in $G$ is a **$[v_0,v_k]$-walk in $G$ but over distinct vertices**

> [!definition] Cycle
> In a simpel graph, a **cycle** is a **$(u,u)$-path of at least three edges**. A simple graph is called acyclic if it does not contain any cycles

> [!definition] Connectivity
> In a graph $G$, two vertices $u$ and $v$ are **connected** if **there exists a $(u,v)$-path in $G$**. In addition, a graph **$G$ is called a connected graph** if **all pairs of vertices in $G$ are connected**
> > [!definition] Component
> > A **component** of a graph $g$ is a **connected subgraph that is not strictly contained** in another connected subgraph of $G$

> [!definition] Cuts
> > [!definition] Vertex
> > Given a graph $G$ and a subset $V^* \subseteq V(G)$, the set $V^*$ is called a **vertex cut** in $G$ if **removing the vertices $V^*$ (and their edges) from $G$ will make $G$ disconnected**
> 
> > [!definition] Edge
> > Given a graph $G$ and a subset $E^* \subseteq E(G)$, the set $E^*$ is called an **edge cut** in $G$ if **removing the edges $E^*$ from $G$ makes $G$ disconnected**
> 
> Given a graph $G$, $\kappa(G)$ (kappa) is the **minimum number of vertices** that we need to remove in order to disconnect $G$
> Given a graph $G$, $\lambda(G)$ (lambda) is the **minimum number of edges** that we need to remove in order to disconnect $G$
> > [!abstract] Theorem
> > In any graph $G$, we have that $$\kappa(G)\leq \lambda(G)\leq \text{min}_{v\in V(G)}\{\delta(v)\}$$




---
References: