---
Tags: 
Created: 2023-07-01 16:52:20
---
(Links:: [[Linear Algebra]])
> [!info] THEOREM 8
> **The Unique Representation Theorem**
> Let $\mathcal{B}=\{b_1,...,b_n\}$ be a basis for a vector space $V$. Then for each $x$ in $V$, there exists a unique set of scalars $c_1,...,c_n$ such that $$x=c_{1}b_{1}+\cdots +c_{n}b_{n}$$

> [!definition] 
> Suppose $\mathcal{B}=\{b_1,...,b_n\}$ is a basis for a vector space $V$ and $x$ is in $V$. The **coordinates of $x$ relative to the basis $\mathcal{B}$** (or the **$\mathcal{B}$-coordinates of $x$**) are the weights $c_1,...,c_n$ such that $x=c_1b_1+\cdots+c_nb_n$

- If $c_{1},...,c_{n}$ are the $\mathcal{B}$-coordinates of $x$, then the vector in $\Bbb R^{n}$ $$[\;x\;]_{\mathcal{B}}=\begin{bmatrix}c_{1}\\\vdots\\c_{n}\end{bmatrix}$$ is the **coordinate vector of $x$** (**relative to $\mathcal{B}$**), or the **$\mathcal{B}$-coordinate vector of $x$**
- The entries in the vector $x=\begin{bmatrix}1\\6\end{bmatrix}$ are the coordinates of $x$ relative to the *standard basis* $\mathcal{E}=\{e_1,e_2\}$, since $$\begin{bmatrix}1 \\ 6\end{bmatrix}=1\begin{bmatrix}1\\0\end{bmatrix}+6\begin{bmatrix}0\\1\end{bmatrix}=1e_{1}+6e_{2}$$
	- If $\mathcal{E}=\{e_{1},e_{2}\}$, then $[x]_{\mathcal{E}}=x$
# A Graphical Interpretation of Coordinates
- The coordinate vector $[x]_{\mathcal{B}}$ gibes the location of $x$ on the new coordinate system described by the vectors of the basis $\mathcal{B}$
# Coordinates in $\Bbb R^n$
- Let $$P_\mathcal{B} =\begin{bmatrix}b_{1}&b_{2}&\cdots&b_{n}\end{bmatrix}$$
  The the vector equation $$x=c_1b_1+c_2b_2+\cdots+c_nb_n$$ is equivalent to $$x=P_{\mathcal{B}}[x]_\mathcal{B}$$
- We call $P_\mathcal{B}$ the **change-of-coordinates matrix** from $\mathcal{B}$ to the standard basis in $\Bbb R^n$.
# The Coordinate Mapping
> [!info] THEOREM 9
> Let $\mathcal{B}=\{b_1,...,b_n\}$ be a basis for a vector space $V$. Then the coordinate mapping $x\mapsto [x]_\mathcal{B}$ is a one-to-one linear transformation from $V$ onto $\Bbb R^n$

> [!example] Using coordinate vectors to verify that the polynomials $1+2t^2$, $4+t+5t^2$, and $3+2t$ are linearly dependent in $\Bbb P_2$.

> [!example] Finding if a vector $x$ is in $H=\text{Span }\{v_1,v_2\}$ and finding the coordinate vector of $x$ relative to $\mathcal{B}=\{v_1,v_2\}$

---
References: