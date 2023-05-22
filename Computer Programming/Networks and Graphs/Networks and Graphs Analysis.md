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
	- The clustering coefficient $cc(v)$ is: $$cc(v)\overset{\text{def}}{=}$$
- A graph $G$ has $n_{\Delta}(G)$ distinct triangles and $n_{\wedge}(G)$ distinct triples. The **network transitivity** $\tau(G)$ is defined as $\frac{n_{\Delta}(G)}{n_{\wedge}(G)}$
- $n_{\Delta}(G)=\frac13\sum_{v\in V^*}n_{\Delta}(v)$
- $n_{\wedge}(v)=\begin{pmatrix}\delta(v)\\ 2\end{pmatrix}=\frac12\delta(v)(\delta(v)-1)$
- $cc(v)=\frac{n_{\Delta}(v)}{n_{\wedge}(v)}$
# Erdös Rényi model
`ER(n,p)` descrives how we can create random networks:
- Start with $n$ isolated nodes
- Pick a node pair $(v,u)$ and generate a random number between 0 and 1. If the number is lower than $p$, then add a link between $v$ and $u$
- Repeat step 2 until all possible pairs are considered
- Probability that $v\in V(G)$ is connected to all nodes $\{u_1,...,u_k\}$: $$\overbrace{p\times p\times \cdots \times p}^{k\text{ times}}=p^k$$
- Probability that $v$ is connected to *only* to $\{u_1,...,u_k\}$: $$\overbrace{p\times p\times \cdots \times p}^{k\text{ times}}\times\overbrace{(1-p)\times(1-p)\times \cdots \times (1-p)}^{n-1-k \text{ times}}=p^k(1-p)^{n-1-k}$$
- $v$ can be incident to $\begin{pmatrix}n-1\\ k\end{pmatrix}$ different sets of $k$ nodes, thus: $$P(\delta(v)=k)=\begin{pmatrix}n-1\\ k\end{pmatrix}p^k(1-p)^{n-1-k}$$
# Probability
- Bernoulli trail: A random experiment with two outcomes with a fixed probability
- The **binomial distribution** $B(m,p)$ is the probability distribution of **the number of successes** in $m$ Bernoulli trials where each trial has success probability $p$
	- Mean/expectation: $mp$ (expexted number of "successes")
	- Probability of exactly $k$ successes: $\begin{pmatrix}m\\ k\end{pmatrix}p^k(1-p)^{m-k}$
- **expected average degree** of a network in $ER(n,p)$: $$\bar\delta =(n-1)p$$
- **expected number of edges**: $$\frac12n\bar\delta$$
# Average path length
- For large $G\in ER(n,p)$ the diameter $d_{\text{max}}$ tends to $\approx \frac{\ln (n)}{\ln(\bar\delta)}$ where $\ln(n)$ is the natural logarithm constant
- Expected number of nodes at distance $d=i$: $\bar \delta^i$
- After $d=\frac{\ln(n)}{\ln(\delta)}$ steps we would see all the nodes, since $\bar \delta^{\frac{\ln(n)}{\ln(\delta)}}\geq n$
- A slight increase of the average degree can lower the average shortest path significantly
# Phase transition
- ER networks have a **phase transition** at $p=\frac1n$
- If $p<\frac1n$, then the connected components are typically of size $O(\log(n))$
- If $p>\frac1n$, then there is typically a connected component of size $O(\log(n))$
- ![[Phase transition graph.png|500]]
# Clustering Coefficient
> [!info] THEOREM
> The expected clustering coefficient of a network $G\in ER(n,p)=p$
> > [!info] Proof
> > Recall that the clustering coefficient is the ration between the current and maximum number of edges between the neighbors
> > - Let $n_v$ be the number of neighbors of a node $v$ in $G$
> > - The expected number of edges between the neighbors of $v$ is $p\begin{pmatrix}n_v\\2\end{pmatrix}=p\frac{n_v(n_v-1)}{2}$
> > - The maximum number of edges of edges between the neighbors is $\begin{pmatrix}n_v\\2\end{pmatrix}$
> > - $\frac{p\begin{pmatrix}n_v\\2\end{pmatrix}}{\begin{pmatrix}n_v\\2\end{pmatrix}}=p$
- Random graphs tend to have a low clustering coefficient

---
References: