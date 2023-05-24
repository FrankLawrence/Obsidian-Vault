---
Tags: 
Created: 2023-05-19 19:49:00
---
(Links:: [[Networks and Graphs]])
# Distance statistics
Specifies how easy it is to reach the other nodes in the network
- The **distance** between $u$ and $v$, denoted as $d(u,v)$, is the length of the shortest $(u,v)$-path
- **eccentricity $\epsilon(u)$**: $max\{d(u,v)|v\in V(G)\}$ (how far is the farthest vertex from $u$)
- **radius $rad(G)$**: $min\{\epsilon(u)|u\in V(G)\}$ (minimum over all eccentricity values)
- **diameter $diam(G)$**: $max\{d(u,v)|u,v\in V(G)\}$ (maximal distance in a network)
- Let $\bar d(u)$ denote the average length of shortest paths from $u$ to any other $v$: $$\bar d(u) \overset{\text{def}}{=} \frac{1}{|V(G)|-1}\sum_{v\in V(G)-\{u\}}d(u,v)$$
- The **average path length** $\bar d(G)$ is defined as  $$\bar d(G) \overset{\text{def}}{=} \frac{1}{|V(G)|}\sum_{u\in V(G)}\bar d(u)$$
- The **characteristic path length** is the median over all $\bar d(u)$

> [!example]- Eccentricity and average path lengths
> ```mermaid
> graph LR;
> a -- 1 --- b
> a -- 3 --- e
> a -- 2 --- b
> b -- 2 --- c
> b -- 5 --- d
> c -- 3 --- f
> c -- 2 --- g
> e -- 1 --- f
> f -- 5 --- g
> ```
> | Vertex | a   | b   | c   | d   | e   | f   | g   | $\epsilon(u)$ | $\bar d(u)$    |
> | ------ | --- | --- | --- | --- | --- | --- | --- | ------------- | -------------- |
> | a      |     | 1   | 5   | 3   | 3   | 7   | 2   | 7             | 3.50            |
> | b      | 1   |     | 5   | 2   | 4   | 7   | 3   | 7             | 3.67            |
> | c      | 5   | 5   |     | 7   | 4   | 2   | 3   | 7             | 4.33            |
> | d      | 3   | 2   | 7   |     | 6   | 9   | 5   | 9             | 5.33            |
> | e      | 3   | 4   | 4   | 6   |     | 6   | 1   | 6             | 4.00            |
> | f      | 7   | 7   | 2   | 9   | 6   |     | 5   | 9             | 6.00            |
> | g      | 2   | 3   | 3   | 5   | 1   | 5   |     | 5             | 3.17            |
# Clustering coefficient
## Local view
A measure of community structure and connections among neighbors. Different with global connectivity
- **Motivation**: The probability that two of your friends know each is higher than the probability that two arbitrary people know each other
- Let $G$ be a simple graph, $v\in V(G)$ with neighbor set $N(v)$. Let $n_v=|N(v)|$ and $m_v$ be the number of edges in the subgraph induced by $N(v)$, i.e., $|E(G[N(v)])|$
	- The clustering coefficient $cc(v)$ is defined as: $$cc(v)\overset{\text{def}}{=}\begin{cases}\frac{m_v}{\binom{n_v}{2}}=\frac{2\cdot m_v}{n_v(n_v-1)}& \text{if }\delta(v)>1\\ \text{undefined}&\text{otherwise}\end{cases}$$
- Let $V^*$ denote the set of vertices $\{v\in V(G)|\delta(v)>1\}$. The clustering coefficient $CC(G)$ is defined as: $$CC(G)\overset{\text{def}}{=}\frac{1}{V^*}\sum_{v\in V^*}cc(v)$$ 
## Global view
- A graph $G$ has $n_{\Delta}(G)$ distinct triangles and $n_{\wedge}(G)$ distinct triples. The **network transitivity** $\tau(G)$ is defined as $\frac{n_{\Delta}(G)}{n_{\wedge}(G)}$
- $n_{\Delta}(G)=\frac13\sum_{v\in V^*}n_{\Delta}(v)$
- $n_{\wedge}(v)=\binom{\delta(v)}{2}=\frac12\delta(v)(\delta(v)-1)$
- $cc(v)=\frac{n_{\Delta}(v)}{n_{\wedge}(v)}$

---
References: