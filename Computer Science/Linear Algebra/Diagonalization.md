---
Tags: Math
Created: 2023-07-03 01:11:37
---
(Links:: [[Linear Algebra]])
- A square matrix $A$ is said to be **diagonalizable** if $A$ is similar to a diagonal matrix, that is, if $A=PDP^{-1}$ for some invertible matrix $P$ and some diagonal matrix $D$.

> [!info] THEOREM 5
> **The Diagonalization Theorem**
> An $n\times n$ matrix $A$ is diagonalization iff $A$ has $n$ linearly independent eigenvectors.
> In fact, $A=PDP^{-1}$, with $D$ a diagonal matrix, iff the columns of $P$ are $n$ linearly independent eigenvectors of $A$. In this case, the diagonal entries of $D$ are eigenvalues of $A$ that correspond, respectively, to the eigenvectors in $P$.
- $A$ is diagonalizable iff there are enough eigenvectors to form a basis (called **eigenvector basis**) of $\Bbb R^n$
# Diagonalizing Matrices

> [!info] THEOREM 6
> An $n\times n$ matrix with $n$ distinct eigenvalues is diagonalizable.

# Matrices Whose Eigenvalues Are Not Distinct
> [!info] THEOREM 7
> Let $A$ be an $n\times n$ matrix whose distinct eigenvalues are $\lambda_1,...,\lambda_p$
> 1. For $1\leq k\leq p$, the dimension of the eigenspace for $\lambda_k$ is less than or equal to the multiplicity of the eigenvalue $\lambda_k$
> 2. The matrix $A$ is diagonalizable iff the sum of the dimensions of the eigenspaces equals $n$, and this happens iff the characteristic polynomial factors completely into linear factors and the dimension of the eigenspace for each $\lambda_k$ equals the multiplicity of $\lambda_k$
> 3. If $A$ is diagonalizable and $\mathcal{B}_k$ is a basis for the eigenspace corresponding to $\lambda_k$ for each $k$, then the total collection of vectors in the sets $\mathcal{B}_1,...,\mathcal{B}_p$ forms an eigenvector basis for $\Bbb R^n$

---
References: