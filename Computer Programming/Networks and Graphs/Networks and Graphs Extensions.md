---
Tags: 
Created: 2023-04-21 15:40:14
---
(Links:: [[Networks and Graphs]])
# Directed graphs
## Basics of directed graphs 
- A **directed graph** or **digraph** $D$ consists of a collection **vertices** $V$, and a collection of **arcs** $A$, for which we write $D=(V,A)$
- Each arc $a=(u,v)$ is said to join vertex $u\in V$ (**tail**) to another vertex $v$ (**head**)
- The **orientation** of a simple undirected graph $G$ is a digraph $D$ in which **each edge of $G$ has been assigned a direction** 

> [!question]- Let $G$ be a simple graph with $m$ edges. How many different orientations does $G$ have?
> $$2^m$$


- The number of arcs with head $v$ is called the **indegree** $\delta_{in}(v)$, the number of arcs having $v$ as their tail is the **outdegree** $\delta_{out}(v)$

> [!info] THEOREM 3.1
> In any digraph $D$: $$\sum_{v\in V(D)}\delta_{in}(v)=\sum_{v \in V(D)}\delta_{out}(v)=|A(D)|$$
> > [!info]- Proof
> > Every edge has exactly one head and tail
> > - the sum of all indegrees is the same as counting all arc heads
> > - the sum of all outdegrees is the same as counting all tails
> > -> both are equal to the number of arcs

- every undirected graph can be turned into an equivalent directed graph by replacing every edge with two arcs
- we cannot always represent a directed graph with an equivalent undirected one
- an [[Networks and Graphs Foundations#^5eab95|adjacency matrix]] $A[i,j]$ is equal to the number of arcs joining vertex $v_i$ to $v_j$
- **Properties of adjacency matrices**:
	- not symmetric: $A_D[i,j]\neq A_D[j,i]$
	- digraph $D$ is **strict** iff $\forall i,j:A[i,j]\leq 1$ and $A[i,i]=0$
	- For each vertex $v_i$, $$\sum_jA[i,j]=\delta_{out}(v_i)$$ and $$\sum_jA[j,i]=\delta_{in}(v_i)$$
- Give a digraph $D$, the [[Networks and Graphs Foundations#^3419b1|adjacency-list]] of $D$ consists of an array $Adj$ of $|V|$ lists. Each list corresponds to one vertex in $V$. For each $u\in V(D)$, the list $Adj[u]$ contains all vertices $v$ such that there is an arc $(u,v)$
## Connectivity for directed graphs
- A **directed** $(v_0,v_k)$**-walk** in $D$ is a sequence $[v_0,a_0,v_1,a_1,...,a_{k-1},v_k]$ of vertices and arcs with $a_i=(v_{i-1},v_i)$
- A **directed trail** is a directed walk in which all arcs are distinct; a **directed path** is a directed trail in which all vertices are also distinct
- A strict digraph (**directed cycle**) is a **$(u,u)$-path of at least two edges**
- two vertices $u,v \in V(D)$ are **connected** in $D$ if there exists a **path from $u$ to $v$** or a **path from $v$ to $u$**
- $u$ and $v$ are **strongly connected** if there is a **path to and from $u$ to $v$**
	- Digraph $D$ is strongly connected is there exsits a directed path between every pair of distinct vertices (every pair of vertices is strongly connected)

> [!info] THEOREM
> A simple graph $G$ has a strongly connected orientation iff $\lambda(G) \geq 2$
> > [!info]- Proof
> > Assume $\lambda(G)=1$
> > - an edge $(u,v)$ connects two subgraphs $G_1$ and $G_2$
> > - If we assign the direction of $(u,v)$ from $u$ to $v$, then no vertex in $G_2$ can reach to a vertex in $G_1$ -> not strongly connected orientation
> > - If we assign the direction of $(u,v)$ from $v$ to $u$, then no vertex in $G_1$ can reach to a vertex in $G_2$ -> not strongly connected orientation
> > 
> > Assume that $\lambda(G)\geq 2$
> > 1. Pick a vertex $v_1$ in $G$ and set $V=\{v_1\}$
> > 2. While $V\neq V(G)$, repeat the following:
> > 3. Pick a vertex $w\not \in V$ (From [[Networks and Graphs Foundations#^e20f09|Menger's theorem]] there are $\geq 2$ edge-independent paths, $P_1$ and $P_2$ between $v_1$ and $w$)
> > 4. Orient either $P_1$ or $P_2$ away and the other one toward $w$

# Colorings
## Edge colorings
- Given a graph $G$, assign **colors** to edges of $G$ so that **edges incident to the same vertex** get different colors, and using the minimum number of colors.
- A simple graph $G$ is **$r$-edge colorable** if the edges of $G$ can be colored with **$r$ different colors**
- The edge-chromatic number $\chi'(G)$ is the minimum $r$ such that $G$ is $r$-edge colorable
- For any graph $G$, define $\Delta (G)=\text{max}_{v\in V(G)}\delta(v)$

> [!info] THEOREM
> In any graph $G$, we have that $\chi'(G)=\Delta(G)$ or $\Delta(G)+1$

## Vertex colorings
- A simple connected graph $G$ is **$k$-vertex colorable** if the vertices of $G$ can be colored with **$r$ different colors** such that the **adjacent vertices get each a different color**
- The chromatic number $\chi(G)$ is the minimum $k$ such that $G$ is $k$-vertex colorable

> [!question]- What is the $\chi(K_n)$?
> $$n$$

> [!info] THEOREM
> In any (simple, connected) graph $G$, we have that $\chi(G)\leq \Delta(G)+1$
> > [!info]- Proof 
> > **Base case**: If $n=2$, then we have $\geq 2$ colors and thus we can color each of the two vertices with a different color
> > **Induction hypothesis**: Assume that any graph $G$ with at most $n$ vertices, for some integer $n$, can be "vertex-colored" with $\leq \Delta(G)+1$ colors
> > **Inductive step**: We need to show that any graph $G$ with $n+1$ vertices can be "vertex-colored" with $\leq \Delta(G)+1$ colors
> > - Take an arbitrary $G$ with $n+1$ vertices and pick one of its vertices, say $v$, arbitrarly. Remove $v$ from $G$ and call the resulting graph $G^*$
> > - $G^*$ can be colored with $\leq \Delta(G^*)+1$ colors
> > - Since $\Delta(G^*)\leq\Delta(G)\Rightarrow G^*$ can be colored with $\leq\Delta(G)+1$ colors
> > - Since vertex $v$ has at most $\Delta(G)$ neighbors but we have $\Delta(G)+1$ colors, vertex $v$ can be colored with a color different those of its neighbors

> [!info] THEOREM
> For any planar graph $G$, we have that $\chi(G)\leq 4$
> > [!info]- ==Proof== $\chi(G)\leq 5$
> > **Base case**: If $n=2$, we can color the vertices of $G$ with 2 colors and $2 \leq 5$
> > **Induction hypothesis**: Assume any planar graph with at most $n$ vertices can be "vertex colored" with <= 5 colors
> > **Inductive step**: We have to show that any planar $G$ with $n+1$ vertices can be "vertex colored" with <= 5 colors
> > - Take an arbitrary graph $G$ with $n+1$ vertices
> > - $G$ has at least one vertex, say $v$, with degree <= 5
> > - Let $G^*=G-\{v\}$. The vertices of $G^*$ can be colored with <= 5 colors, by induction hypothesis. Assume $c_1,...,c_5$ are colors used in $G^*$
> > - If $|N(v)|<5$, then $v$ can be colored with an unused color and we're done
> > Assume that $|N(v)|=5$ and that all 5 colors are used by $N(v)$
> > - Let $N(v)=\{v_1,v_2,...,v_5\}$, clockwise around $v$, and $color (v_i)=c_i$
> > Assume that no $[v_1,v_3]$-path in $G^*$ has only colors $c_1$ and $c_3$
> > - Let $H$ be the subgraph induced by all $(v_1,w)$-paths in $G^*$ that have only colors $c_1$ and $c_3$

---
References: