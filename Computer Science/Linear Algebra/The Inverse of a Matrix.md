---
Tags: Math
Created: 2023-04-10 23:39:26
---
(Links:: [[Linear Algebra]])
- An $n\times n$ matrix $A$ is **invertible** if there is an $n\times n$ matrix $C$ such that $$CA=I\quad \text{and} \quad AC=I$$ where $I=I_{n}$
	- The unique inverse is denoted by $A^{-1}$ so that $$A^{-1}A=I\quad\text{and}\quad AA^{-1}=I$$

> [!info] THEOREM 4
> Let $A=\begin{bmatrix}a&b\\c&d\end{bmatrix}$. If $ad-bc\neq 0$, then $A$ is invertible and $$A^{-1}=\frac{1}{ad-bd}\begin{bmatrix}\;\;\;d&-b\\-c&\;\;\;a\end{bmatrix}$$
> If $ad-bc=0$, then $A$ is not invertible.

- $ad-bc$ is called the [[Introduction to Determinants|determinant]] of $A$
	- we write $\text{det }A=ad-bc$
	- $A$ is invertible iff $\text{det }A\neq 0$

> [!info] THEOREM 5
> If $A$ is an invertible $n\times n$ matrix, then for each $b$ in $\Bbb R^n$, the equation $Ax=b$ has the unique solution $x=A^{-1}b$

> [!info] THEOREM 6
> 1. If $A$ is an invertible matrix, then $A^{-1}$ is invertible and $$(A^{-1})^{-1}=A$$
> 2. If $A$ and $B$ are $n\times n$ invertible matrices, then so is $AB$, and the inverse of $AB$ is the product of the inverses of $A$ and $B$ in the reverse order. That is, $$(AB)^{-1}=B^{-1}A^{-1}$$
> 3. If $A$ is an invertible matrix, then so is $A^{T}$, and the inverse of $A^{T}$ is the transpose of $A^{-1}$. That is, $$(A^{T})^{-1}=(A^{-1})^{T}$$

- The product of $n\times n$ invertible matrices is invertible, and the inverse is the product of their inverse in the reverse order.
# Elementary Matrices
- An **elementary matrix** is one that is obtained by performing a single elementary row operation on an identity matrix.
- If an elementary row operation is performed on an $m\times m$ matrix $A$, the resulting matrix can be written as $EA$, where the $m\times m$ matrix $E$ is created by performing the same row operation on $I_{m}$
- Each elementary matrix $E$ is invertible. The inverse of $E$ is the elementary matrix of the same type that transforms $E$ back into $I$

> [!info] 
> An $n\times n$ matrix $A$ is invertible iff $A$ is row equivalent to $I_{n}$, and in this case, any sequence of elementary row operations that reduces $A$ to $I_n$ also transforms $I_n$ into $A^{-1}$
# An Algorithm for Finding $A^{-1}$
- Row reduce the augmented matrix $\begin{bmatrix}A&I\end{bmatrix}$. If $A$ is row equivalent to $I$, then $\begin{bmatrix}A&I\end{bmatrix}$ is row equivalent to $\begin{bmatrix}I&A^{-1}\end{bmatrix}$. Otherwise, $A$ does not have an inverse.

> [!hint]- Reasonable Answers
> Multiply the matrix $A$ by its inverse $A^{-1}$ and check if the resulting matrix is the identity matrix $I$
---
References: