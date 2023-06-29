---
Tags: 
Created: 2023-04-10 23:39:53
---
(Links:: [[Linear Algebra]])
> [!info] THEOREM 8
> **The Invertible Matrix Theorem**
> Let $A$ be a square $n\times n$ matrix. Then the following statements are equivalent. That is, for a given $A$, the statements are either all true or all false.
> 1. $A$ is an invertible matrix
> 2. $A$ is row equivalent to the $n \times n$ identity matrix
> 3. $A$ has $n$ pivot positions
> 4. The equation $Ax=0$ has only the trivial solution
> 5. The columns of $A$ form a linearly independent set
> 6. The linear transformation $x\mapsto Ax$ is one-to-one
> 7. The equation $Ax=b$ has at least one solution for each $b$ in $\Bbb R^n$
> 8. The columns of $A$ span $\Bbb R^n$
> 9. The linear transformation $x\mapsto Ax$ maps $\Bbb R^n$ onto $\Bbb R^n$
> 10. There is an $n\times n$ matrix $C$ such that $CA=I$
> 11. There is an $n\times n$ matrix $D$ such that $AD=I$
> 12. $A^T$ is an invertible matrix

- Let $A$ and $B$ be square matrices. If $AB=IA$, then $A$ and $B$ are both invertible, with $B=A^{-1}$ and $A=B^{-1}$.
# Invertible Linear Transformations
> [!info] THEOREM 9
> Let $T:\Bbb R^{n}\to\Bbb R^{n}$ be a linear transformation and let $A$ be the standard matrix for $T$. Then $T$ is invertible iff $A$ is an invertible matrix. In that case, the linear transformation $S$ given by $S(x)=A^{-1}x$ is the unique function satisfying equations $$S(T(x))=x \qquad\text{for all } x \text{ in } \Bbb R^n$$ $$T(S(x))=x \qquad\text{for all } x \text{ in } \Bbb R^n$$

---
References: