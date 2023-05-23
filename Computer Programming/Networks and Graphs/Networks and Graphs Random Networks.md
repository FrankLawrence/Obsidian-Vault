---
Tags: 
Created: 2023-05-22 21:12:02
---
(Links:: [[Networks and Graphs]])
# Classical random networks
- **Erdös-Rényi model** (**ER random graph**): undirected graph in which two vertices are connected by an edge with probability $p$
- $ER(n,p)$ denotes the set of *all* ER random graphs with $n$ vertices and probability $p$ that two vertices are joined
- ER graphs are [[Networks and Graphs Foundations#^4cc75e|simple]]
## Degree distribution
- a vertex $u$ can be a neighbor to at max $n-1$ other vertices -> there are $\begin{pmatrix}n-1\\k\end{pmatrix}$ possibilites for choosing $k$ different vertices to be adjacent to $u$
- Probability that $v\in V(G)$ is connected to all nodes $\{u_1,...,u_k\}$: $$\overbrace{p\times p\times \cdots \times p}^{k\text{ times}}=p^k$$
- Probability that $v$ is connected *only* to $\{u_1,...,u_k\}$: $$\overbrace{p\times p\times \cdots \times p}^{k\text{ times}}\times\overbrace{(1-p)\times(1-p)\times \cdots \times (1-p)}^{n-1-k \text{ times}}=p^k(1-p)^{n-1-k}$$
- The probability that $u$ has exactly $k$ other vertices can be defined as: $$P(\delta(v)=k)=\begin{pmatrix}n-1\\ k\end{pmatrix}p^k(1-p)^{n-1-k}$$
- The mean vertex degree of an $ER(n,p)$ graph is computed as: $$\bar\delta\overset{\text{def}}{=}\Bbb E[\delta]\overset{\text{def}}{=}\sum_{k=1}^{n-1}k\cdot P(\delta=k)$$

> [!info] THEOREM 7.1
> The expected value for the vertex degree of an $ER(n,p)$ graph is equal to $p(n-1)$

- **expected number of edges**: $$\frac12n\bar\delta$$
- larger graphs have a better approximation to the theoretical degree distribution (fluctuation in the expected vertex degrees decreases)

## Other metrics for random graphs
- **Average path length** for large graphs $H\in ER(n,p)$: $$\bar d(H)=\frac{\ln(n)-\gamma}{\ln(pn)}+0.5$$ with $\gamma$ is the *Euler constant*
- Since $\bar \delta=p(n-1)$: $$\bar d(H)=\frac{\ln(n)-\gamma}{\ln(\bar\delta)}+0.5\approx \frac{\ln n}{\ln \bar\delta}$$
- keeping the vertex count the same and adding more edges (increase of $\bar\delta$) drops the average path length logarithmically
	- ![[Average path length of ER graphs with varying average vertex degree.png|500]]

> [!info] THEOREM 7.2
> Let $G\in ER(n,p)$, then $CC(G)=p$
> > [!info] Proof
> > Recall that the clustering coefficient is the ratio between the current and maximum number of edges between the neighbors
> > - Let $n_v$ be the number of neighbors of a node $v$ in $G$
> > - The expected number of edges between the neighbors of $v$ is $p\begin{pmatrix}n_v\\2\end{pmatrix}=p\frac{n_v(n_v-1)}{2}$
> > - The maximum number of edges between the neighbors is $\begin{pmatrix}n_v\\2\end{pmatrix}$
> > - $\frac{p\begin{pmatrix}n_v\\2\end{pmatrix}}{\begin{pmatrix}n_v\\2\end{pmatrix}}=p$

- Random graphs tend to have a low clustering coefficient
- networks usually have a **giant component** where most vertices of the graph are contained in after $p$ passes a certain value
- ER networks have a **phase transition** at $p=\frac1n$
- If $p<\frac1n$, then the connected components are typically of size $O(\log(n))$
- If $p>\frac1n$, then there is typically a connected component of size $O(n)$
- ![[Phase transition graph.png|500]]

# Small worlds
- A network of size $n$ follows the small world phenomenon if the average shortest-path lengths in the network is in $O(\log n)$

> [!info] ALGORITHM 7.1 (Watts-Strogatz)
> Consider a set of $n$ vertices $\{v_1,v_2,...,v_n\}$ and an (even) number $k$. In order to ensure that the grahp will have relatively few edges, choose $n$ and $k$ such that $n>>k>>\ln(n)>>1$
> 1. Order the $n$ vertices into a ring and connect each vertx to its first $k/2$ left-hand neighbors, and its $k/2$ right-hand neighbors, leading to graph $G^1$
> 2. With probability $p$, replace each edge $\langle u,v\rangle$ with an edge $\langle u,w\rangle$ where $w$ is a randomly chosen vertex from $V(G)$ other that $u$, and such that $\langle u,w\rangle$ is not already contained in edge set of $G$

- for a graph in $WS(n,k,0)$ the maximum distance is equal to $\left\lceil \frac{n}{k}\right\rceil$
- many vertices stay close together but most vertices will also have a link to a vertex far away
- even for relatively large values of $p$ the WS graphs have the same clustering coefficient as with $p=0$

> [!info] THEOREM 7.3
> For any Watts-Strogatz graph $G$ from $WS(n,k,0)$ the clustering coefficient for $G$ is equal to $CC(G)=\frac{3(k-2)}{4(k-1)}$
> > [!info] Proof
> > - how far apart are two nodes $i$ and $j$ on the "outer ring" of a $WS(n,k,0)$ graph is defined as: $$d_2^n(i,j)\overset{\text{def}}{=}\text{min}\{|i,j|,n-|i-j|\}$$

- the clustering coefficient of a $WS(n,k,0)$ graph is **independent of its size** and for large values of $k$ it is close to $\frac34$
- Generally, $WS$-graphs are considered to have a high clustering coefficient, as for $p$ significantly smaller than 1, may edges aren't replaces
- if $G\in WS(n,k,0)$ and $k=2$, then $cc(G)=0$

> [!info] THEOREM 7.4
> For any Watts-Strogatz graph $G$ from $WS(n,k,0)$ the average shortest-path length $\bar d(u)$ from a given vertex $u$ to any other vertex in $G$ is approximated by $$\bar d(u)\approx\frac{(n-1)(n+k-1)}{2kn}\approx \frac{n}{2k}$$

- when increasing $p$ slightly, the average path length of a WS-graph drops rapidly
	- ![[Relation between clustering coefficient and average path length.png|500]]
	- the clustering coefficient stays relatively high except when $p$ becomes large
- If $k$ is relatively small, then the expected average shortest-path length in $WS(n,k,0)$ graphs is high -> no small world

# Scale-free Networks
- real world networks have few high-degree nodes and where the number of nodes with high degree decreases exponentially
- It is extremely unlikely that hubs exist in networks whose degree distribution is based on an exponential function, e.g., Bionomial and Poisson distributions.
- A **scale-free function** can model the degree distribution of a graph: $$f(x)=Cx^{-\alpha}$$ where $C$ and $\alpha$ are two constant numbers and $2\leq \alpha\leq3$
- A network is called a **scale-free network** if its degree distribution can be expressed by a **scale-free function**
- In random graphs with exponential degree distribution the size of the hubs increases logarithmically as $n$ increases
- With power-law distribution functions the size of the hubs increases almost linearly as $n$ increases
- **preferential attachment**: new vertices prefer to be connected to the "more connected" vertices

> [!info] ALGORITHM 7.2 (Barabási-Albert)
> Consider a (relatively small) ER random graph $G_0$ with $n_0$ vertices $V_0$. At each step $s>0$:
> 1. Add a new vertex $v_s$ to $V_{s-1}$ (i.e., $V_S\leftarrow V_{s-1}\cup\{v_s\}$)
> 2. Add $m\leq n_0$ edges to the graph, each edge being incident with $v_s$ and a vertex $u$ from $V_{s-1}$ chosen with probability $$P(\text{select } u)=\frac{\delta(u)}{\sum_{w\in V_{s-1}}\delta(w)}$$ that is, choosing a vertex $u$ is proportional to the current vertex degree of $u$. Vertex $u$ must not have been previously chosen during this step.
> 3. Stop when $n$ vertices have been aded, otherwise repeat the previous two steps

> [!info] THEOREM 10.1
> Let $G\in BA(n,n_0,m)$ and $v$ be an arbitrary vertex of $G$. Then: $$P[\delta(v)=k]\approx \frac{2m^2}{k^3}\propto \frac{1}{k^3}=k^{-3}$$

> [!info] THEOREM 10.2
> Let $G$ be a generalized $BA(n,n_0,m)$ and $v$ be an arbitrary vertex of $G$. Then: $$P[\delta(v)=k]\propto k^{-(2+\frac{1}{1+2c})}$$
- If $c=0$, then $P[\delta(v)=k]\propto k^{-3}$
  If $c\to \infty$, then $P[\delta(v)=k]\propto k^{-2}$

> [!info] THEOREM 10.3
> Let $G\in BA(n,n_0,m)$. Then, the expected average shortest-path length in $G$ is: $$\bar d(G)=\frac{\ln n-\ln (m/2)-1-\gamma}{\ln\ln n+\ln(m/2)}+1.5$$ where $\gamma\approx 0.5772$ is the Euler's constant

- BA-graphs have shorter expected average shortest-path lengths than $ER$ graphs

> [!info] THEOREM 10.4
> Let $G\in BA(n,n_0,m)$ and $v_t$ be the vertex of $G$ that was added to $G$ at time $t$. Then: $$cc(v_t)=\frac{m-1}{8(\sqrt)}$$


---
References: