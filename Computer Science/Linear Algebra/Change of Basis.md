---
Tags: Math
Created: 2023-07-02 13:53:56
---
(Links:: [[Linear Algebra]])
> [!info] THEOREM 15
> Let $\mathcal{B}=\{b_{1},...,b_{n}\}$ and $\mathcal{C}=\{c_{1},...,c_{n}\}$ be bases of a vector space $V$. Then there is a unique $n\times n$ matrix $\underset{\mathcal{C}\gets\mathcal{B}}{P}$ such that $$[x]_{\mathcal{C}}=\underset{\mathcal{C}\gets\mathcal{B}}{P}[x]_{\mathcal{B}}$$
> The columns of $\underset{\mathcal{C}\gets\mathcal{B}}{P}$ are the $\mathcal{C}$-coordinate vectors of the vectors in the basis $\mathcal{B}$. That is, $$\underset{\mathcal{C}\gets\mathcal{B}}{P}=\begin{bmatrix}[b_1]_{\mathcal{C}}&[b_2]_{\mathcal{C}}&\cdots&[b_n]_{\mathcal{C}}\end{bmatrix}$$

- $\underset{\mathcal{C}\gets\mathcal{B}}{P}$ is called the **change-of-coordinates matrix from $\mathcal{B}$ to $\mathcal{C}$**
- $\Big(\,\underset{\mathcal{C}\gets\mathcal{B}}{P}\,\Big)^{-1}$ is the matrix that converts $\mathcal{C}$-coordinates into $\mathcal{B}$-coordinates, that is $$\Big(\,\underset{\mathcal{C}\gets\mathcal{B}}{P}\,\Big)^{-1}=\underset{\mathcal{B}\gets\mathcal{C}}{P}$$
- To find $[b_1]_{\mathcal{C}}$
# Change of Basis in $\Bbb R^n$
> [!example] Find change-of-coordinates matrix from $\mathcal{B}$ to $\mathcal{C}$.
> Let $b_{1}=\begin{bmatrix}-9\\1\end{bmatrix}$, $b_{2}=\begin{bmatrix}-5\\-1\end{bmatrix}$, $c_{1}=\begin{bmatrix}1\\-4\end{bmatrix}$, $c_{2}=\begin{bmatrix}3\\-5\end{bmatrix}$, and consider the bases for $\Bbb R^2$ given by $\mathcal{B}=\{b_{1},b_{2}\}$ and $\mathcal{C}=\{c_{1},c_{2}\}$.
> Augment the coefficient matrix with $b_1$ **and** $b_2$ and row reduce: $$\begin{bmatrix}c_1&c_{2}\,\mid \,b_1&b_2\end{bmatrix}\sim[\;I\,\mid\,\underset{\mathcal{C}\gets\mathcal{B}}{P}\;]$$
- Recall that for each $x$ in $\Bbb R^n$, $$P_\mathcal{B}[x]_{\mathcal{B}}=x,\qquad P_\mathcal{C}[x]_{\mathcal{C}}=x,\qquad [x]_{\mathcal{c}}=P_\mathcal{C}^{-1}x$$ Thus $$[x]_{\mathcal{c}}=P_\mathcal{C}^{-1}x=P_\mathcal{C}^{-1}P_\mathcal{B}[x]_{\mathcal{B}}$$ In other words, to get the vector $[x]_\mathcal{B}$ in terms of the coordinate vectors of $\mathcal{C}$ we must first convert to the standard system with $P_\mathcal{B}$ and then to the basis $\mathcal{C}$ via $P_\mathcal{C}^{-1}$, or we simply use $\underset{\mathcal{C}\gets\mathcal{B}}{P}$

---
References: