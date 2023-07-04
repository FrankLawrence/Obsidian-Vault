---
Tags: 
Created: 2023-06-28 21:13:32
---
(Links:: [[Linear Algebra]])
> [!definition] Eigenvector / Eigenvalue
> An **eigenvector** of an $n\times n$ matrix $A$ is a nonzero vector **x** such that $Ax=\lambda x$ for some scalar $\lambda$. A scalar $\lambda$ is called an **eigenvalue** of $A$ if there is a nontrivial solution **x** of $Ax=\lambda x$; such an **x** is called an *eigenvector corresponding to $\lambda$*.

- An eigenvector is a vector that under transformation of $A$ is turned into a scalar multiple of itself

> [!example]- Find eigenvector for a matrix with eigenvalue 7
> $Ax=7x \to Ax-7x=0 \to (A-7I)x=0$
> $$A-7I=\begin{bmatrix}1&6\\5&2\end{bmatrix}-\begin{bmatrix}7&0\\0&7\end{bmatrix}=\begin{bmatrix}-6&6\\5&-5\end{bmatrix}$$
> $A-7I$ is linearly dependent -> the are nontrivial solutions -> 7 *is* an eigenvalue of $A$
> 
> Finding corresponding eigenvectors: $$\begin{bmatrix}-6&6&0\\5&-5&0\end{bmatrix}\sim \begin{bmatrix}1&-1&0\\0&0&0\end{bmatrix}$$
> $$x=x_2\begin{bmatrix}1\\1\end{bmatrix}$$
> Each vector $x$ with $x_2\neq 0$ is an eigenvector
> A basis for the eigenspace is $\left\{\begin{bmatrix}1\\1\end{bmatrix}\right\}$

- $\lambda$ is an eigenvalue of an $n \times n$ matrix $A$ iff the equation $$(A- \lambda I)x=0$$ has a non trivial solution / free variables

> [!hint]- Reasonable Answers
> Multiply the matrix $A$ by the eigenvector and check if the result is a multiple of the eigenvector by the eigenvalue.

> [!info] THEOREM 1
> The eigenvalues of a triangular matrix are the entries on its main diagonal.

- 0 *is an eigenvalue of $A$ iff $A$ is not invertible*

> [!info] THEOREM 2
> If $v_{1},...,v_{r}$ are eigenvectors that correspond to distinct eigenvalues $\lambda_{1},...,\lambda_{r}$ of an $n\times n$ matrix $A$, then the set $\{v_{1},...,v_{r}\}$ is linearly independent.

---
References: