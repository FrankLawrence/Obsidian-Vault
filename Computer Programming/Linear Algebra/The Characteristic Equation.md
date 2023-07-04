---
Tags: 
Created: 2023-06-29 00:55:08
---
(Links:: [[Linear Algebra]])
> [!example]- Find eigenvalues of $A=\begin{bmatrix}\;2&\;\;\;3\;\\\;3&-6\;\end{bmatrix}$
> We must find all scalars $\lambda$ such that the matrix equation $$(A-\lambda I)x=0$$ has a nontrivial solution.
> This is equivalent to finding all $\lambda$ such that the matrix $A-\lambda I$ is *not* invertible, where $$A-\lambda I=\begin{bmatrix}2-\lambda&3\\3&-6-\lambda\end{bmatrix}$$
> The matrix fails to be invertible when its determinant is zero: $$\text{det}(A-\lambda I)=\text{det}\begin{bmatrix}2-\lambda&3\\3&-6-\lambda\end{bmatrix}=0$$

# Determinants
> [!info] THEOREM 3
> **Properties of Determinants**
> Let $A$ and $B$ be $n\times n$ matrices.
> 1. $A$ is invertible iff $\text{det }A\neq 0$
> 2. $\text{det }AB=(\text{det }A)(\text{det }B)$
> 3. $\text{det }A^T=\text{det }A$
> 4. If $A$ is triangular, then $\text{det }A$ is the product of the entries on the main diagonal of $A$
> 5. A row replacement operation on $A$ does not change the determinant. A row interchange changes the sign of the determinant. A row scaling also scales the determinant by the same scalar factor.

> [!info] THEOREM
> [[Characterizations of Invertible Matrices#^286bae|The Invertible Matrix Theorem]] (continued)
> Let $A$ be an $n\times n$ matrix. Then $A$ is invertible iff
> 18. The number 0 is *not* an eigenvalue of $A$
# The Characteristic Equation
- A scalar $\lambda$ is an eigenvalue of an $n\times n$ matrix $A$ iff $\lambda$ satisfies the characteristic equation $$\text{det}(A-\lambda I)=0$$

> [!tip] Reasonable Answers
> Row reduce $A-\lambda I$. If every column is a pivot column, then the scalar $\lambda$ is not an eigenvalue of $A$ (since $A-\lambda I$ would be invertible and the determinant would not be 0!)
- $\text{det }(A-\lambda I)$ is a polynomial of degree $n$ called the **characteristic polynomial** of $A$
- The **multiplicity** of an eigenvalue $\lambda$ is its multiplicity as a root of the characteristic equation
# Similarity
- $A$ **is similar to** $B$ if there is an invertible matrix $P$ such that $P^{-1}AP=B$

> [!info] THEOREM 4
> If $n\times n$ matrices $A$ and $B$ are similar, then they have the same characteristic polynomial and hence the same eigenvalues (with the same multiplicities)

---
References: