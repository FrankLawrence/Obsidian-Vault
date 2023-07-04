---
Tags: 
Created: 2023-07-02 00:38:54
---
(Links:: [[Linear Algebra]])
> [!info] THEOREM 10
> If a vector space $V$ has a basis $\mathcal{B}=\{b_1,...,b_n\}$, then any set in $V$ containing more than $n$ vectors must be linearly dependent.

> [!info] THEOREM 11
> If a vector space $V$ has a basis of $n$ vectors, then every basis of $V$ must consist of exactly $n$ vectors.

> [!definition] Dimension
> If a vector space $V$ is spanned by a finite set, then $V$ is said to be **finite-dimensional**, and the **dimension** of $V$, written as dim $V$, is the number of vectors in a basis for $V$. The dimension of the zero vector space $\{0\}$ is defined to be zero. If $V$ is not spanned by a finite set, then $V$ is said to be **infinite-dimensional**.

- The standard basis for $\Bbb R^n$ contains $n$ vectors, so dim $\Bbb R^n=n$.
- In general, dim $\Bbb P_n=n+1$
	- The space $\Bbb P$ of all polynomials is infinite-dimensional
# Subspaces of a Finite-Dimensional Space
> [!info] THEOREM 12
> Let $H$ be a subspace of a finite-dimensional vector space $V$. Any linearly independent set in $H$ can be expanded, if necessary, to a basis for $H$. Also, $H$ is finite-dimensional and $$\text{dim }H\leq \text{dim }V$$

> [!info] THEOREM 13
> **The Basis Theorem**
> Let $V$ be a $p$-dimensional vector space, $p\geq 1$. Any linearly independent set of exactly $p$ elements in $V$ is automatically a basis for $V$. Any set of exactly $p$ elements that spans $V$ is automatically a basis for $V$.
# The Dimensions of Nul $A$, Col $A$, and Row $A$
> [!definition] Rank and nullity
> The **rank** of an $m\times n$ matrix $A$ is the dimension of the column space and the **nullity** of $A$ is the dimension of the null space.

- The rank of $A$ is the number of pivot columns
- The nullity of $A$ is the number of free variables.
- The dimension of Row $A$ is equal to the number of pivot rows and it is also equal to the rank of $A$

> [!info] THEOREM 14
> **The Rank Theorem**
> The dimensions of the column space and the null space of an $m\times n$ matrix $A$ satisfy the equation $$\text{rank }A+\text{nullity }A=\text{number of columns in }A$$

# Rank and the Invertible Matrix Theorem
> [!info] THEOREM
> [[Characterizations of Invertible Matrices#^286bae|The Invertible Matrix Theorem]] (continued)
> Let $A$ be an $n\times n$ matrix. Then the following statements are each equivalent to the statement that $A$ is an invertible matrix.
> 13. The columns of $A$ form a basis of $\Bbb R^n$
> 14. Col $A=\Bbb R^n$
> 15. rank $A=n$
> 16. nullity $A=0$
> 17. Nul $A=\{0\}$

---
References: