---
Tags: Math
Created: 2023-07-01 12:10:15
---
(Links:: [[Linear Algebra]])
> [!info] THEOREM 4
> An indexed set $\{v_{1},...,v_{p}\}$ of two or more vectors, with $v_{1}\neq 0$, is linearly dependent iff some $v_{j}$ (with $j>1$) is a linear combination of the preceding vectors, $v_{1},...,v_{j-1}$.

> [!definition] Basis
> Let $H$ be a subspace of a vector space $V$. A set of vectors $\mathcal{B}$ in $V$ is a **basis** for $H$ if
> 1. $\mathcal{B}$ is a linearly independent set, and
> 2. the subspace spanned by $\mathcal{B}$ coincides with $H$; that is, $$H=\text{Span }\mathcal{B}$$

- The columns of an invertible $n\times n$ matrix $A$ form a basis for $\Bbb R^n$ because they are linearly independent and span $\Bbb R^n$
- The set $\{e_1,...,e_n\}$ of the columns of the $n\times n$ identity matrix $I_n$ is called the **standard basis** for $\Bbb R^n$
- To prove that a set of vectors span a vector space you must prove that the coefficient matrix is **invertible**
# The Spanning Set Theorem
> [!example]- Proof that $\text{Span }\{v_1,v_2,v_3\} = \text{Span }\{v_1,v_2\}$ and finding the basis for the subspace $H=\text{Span }\{v_1,v_2,v_3\}$
> - Find the coefficient values for 2 vectors that make up the "redundant" third vector e.g. $v_3=2v_1+5v_2$
> - Any vector $x$ in $H$ can be expressed by $$\begin{align}x&=c_1v_1+c_2v_2+c_3(2v_1+5v_{2)}\\ &= (c_1+2c_3)v_1+(c_2+5c_3)v_2\end{align}$$
> - $\{v_1,v_2\}$ is a basis of $H$ since it's linearly independent

> [!info] THEOREM 5
> **The Spanning Set Theorem**
> Let $S=\{v_{1},...,v_{p}\}$ be a set in a vector space $V$, and let $H=\text{Span }\{v_{1},...,v_{p}\}$.
> 1. If one of the vectors in $S$–say, $v_{k}$–is a linear combination of the remaining vectors in $S$, then the set formed from $S$ by removing $v_{k}$ still spans $H$.
> 2. If $H\neq \{0\}$ , some subset of $S$ is a basis for $H$.
# Bases for Nul $A$, Col $A$, and Row $A$
> [!info] THEOREM 6
> The pivot columns of a matrix $A$ form a basis for Col $A$

> [!info] THEOREM 7
> If two matrices $A$ and $B$ are row equivalent, then their row spaces are the same. If $B$ is in echelon form, the nonzero rows of $B$ form a basis for the row space of $A$ as well as for that of $B$.


---
References: