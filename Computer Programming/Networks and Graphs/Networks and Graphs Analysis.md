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
- **eccentricity $\epsilon(u)$**: $max\{d(u,v)|v\in V(G)\}$
- **radius $rad(G)$**: $min\{\epsilon(u)|u\in V(G)\}$
- **diameter $diam(G)$**: $max\{d(u,v)|u,v\in V(G)\}$
- The average length of shortest paths from $u$ to any other $v$, denoted as $\bar d(u)$, is defined as: $$\frac{1}{|V(G)|-1}\sum_{v\in V(G)-\{u\}}d(u,v)$$
  $\bar d(G)$ denotes the **average path length** $$\frac{1}{|V(G)|}\sum_{u\in V(G)}\bar d(u)$$
- the **characteristic path length** is the median over all $\bar d(v)$

## Clustering
A measure of community structure and connections among neighbors. Different with global connectivity
- **Motivation**: The probability that two of your friends know each is higher than the probability that two arbitrary people know each other
- A graph $G$ has $n_{\Delta}(G)$ distinct triangles and $n_{\wedge}(G)$ distinct triples. The **network transitivity** $\tau(G)$ is defined as $\frac{n_{\Delta}(G)}{n_{\wedge}(G)}$
- $n_{\Delta}(G)=\frac13\sum_{v\in V^*}n_{\Delta}(v)$
- $n_{\wedge}(v)=\frac12\delta(v)(\delta(v)-1)$
- $cc(v)=\frac{n_{\Delta}(v)}{n_{\wedge}(v)}$
# Erdös Rényi model
`ER(n,p)` descrives how we can create random networks:
- Start with $n$ isolated nodes
- Pick a node pair $(v,u)$ and generate a random number between 0 and 1. If the number is lower than $p$, then add a link between $v$ and $u$
- Repeat step 2 until all possible pairs are considered
- Probability that $v\in V(G)$ is connected to all nodes $\{u_1,...,u_k\}$: $$p\times p\times \cdots \times p=p^k$$
- Probability that $v$ is connected to *only* to $\{u_1,...,u_k\}$: $$$$

---
References: