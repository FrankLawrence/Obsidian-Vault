---
Tags: 
Created: 2023-04-25 00:55:30
---
(Links:: [[Networks and Graphs]])
# Euler tours
## Contructing an Euler tour
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
> > **Induction hypothesis**: Assume the theorem holds for all graphs with less than $m$ edges, for some integer $m$
> > **Inductive step**: Take an arbitrary graph $G$ with $m$ edges. Graph $G$ has a cycle, since a graph where the degree of each vertex is at least 2 contains a cycle.
> > - $C$ is a cycle of $G$ and graph $G-C$ consists of a set of components connected by $C$
> > - Each vertex of $G-C$ has still an even degree 
> >   => Each component $\sigma$ of $G-C$ has an Euler tour, by induction hypothesis as each of them has less than $m$ edges

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
- 

---
References: