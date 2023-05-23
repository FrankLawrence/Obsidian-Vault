---
Tags: 
Created: 2023-05-19 19:49:00
---
(Links:: [[Networks and Graphs]])
# Metrics
- **Probability distribution** is a function that maps the outcome of a random process to possible random outputs

## Degree distribution
Useful to evalute the robustness of a network and whether there are nodes that can compromise the functioning
## Path length
Specifies how easy it is to reach the other nodes in the network
- The **distance** between $u$ and $v$, denoted as $d(u,v)$, is the length of the shortest $(u,v)$-path
- **eccentricity $\epsilon(u)$**: $max\{d(u,v)|v\in V(G)\}$ (how far is the farthest vertex from $u$)
- **radius $rad(G)$**: $min\{\epsilon(u)|u\in V(G)\}$ (minimum over all eccentricity values)
- **diameter $diam(G)$**: $max\{d(u,v)|u,v\in V(G)\}$ (maximal distance in a network)
- Let $\bar d(u)$ denote the average length of shortest paths from $u$ to any other $v$: $$\bar d(u) \overset{\text{def}}{=} \frac{1}{|V(G)|-1}\sum_{v\in V(G)-\{u\}}d(u,v)$$
  $\bar d(G)$ denotes the **average path length** $$\frac{1}{|V(G)|}\sum_{u\in V(G)}\bar d(u)$$
- the **characteristic path length** is the median over all $\bar d(v)$

## Clustering
A measure of community structure and connections among neighbors. Different with global connectivity
- **Motivation**: The probability that two of your friends know each is higher than the probability that two arbitrary people know each other
- Let $G$ be a simple graph, $v\in V(G)$ and $m_v$ be the number of edges in the subgraph induced by $N(v)$, i.e., $|E(G[N(v)])|$
	- The clustering coefficient $cc(v)$ is defined as: $$cc(v)\overset{\text{def}}{=}\begin{cases}\frac{m_v}{\begin{pmatrix}n_v\\2\end{pmatrix}}=\frac{2\cdot m_v}{n_v(n_v-1)}& \text{if }\delta(v)>1\\ \text{undefined}&\text{otherwise}\end{cases}$$
	- Let $V^*$ denote the set of vertices $\{v\in V(G)|\delta(v)>1\}$. The clustering coefficient $CC(G)$ is defined as: $$CC(G)\overset{\text{def}}{=}\frac{1}{V^*}\sum_{v\in V^*}cc(v)$$ 
- A graph $G$ has $n_{\Delta}(G)$ distinct triangles and $n_{\wedge}(G)$ distinct triples. The **network transitivity** $\tau(G)$ is defined as $\frac{n_{\Delta}(G)}{n_{\wedge}(G)}$
- $n_{\Delta}(G)=\frac13\sum_{v\in V^*}n_{\Delta}(v)$
- $n_{\wedge}(v)=\begin{pmatrix}\delta(v)\\ 2\end{pmatrix}=\frac12\delta(v)(\delta(v)-1)$
- $cc(v)=\frac{n_{\Delta}(v)}{n_{\wedge}(v)}$
# Vertex Degrees
## Degree distribution
# Distance statistics
# Clustering coefficient
## Local view
## Global view

---
References: