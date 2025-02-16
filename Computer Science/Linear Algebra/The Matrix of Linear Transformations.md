---
Tags: Math
Created: 2023-04-10 23:37:18
---
(Links:: [[Linear Algebra]])
> [!info] THEOREM 10
> Let $T:\Bbb R^{n}\to\Bbb R^{m}$ be a linear transformation. Then there exists a unique matrix $A$ such that $$T(x)=Ax\qquad\text{for all } x \text{ in } \Bbb R^{n}$$
> In fact, $A$ is the $m\times n$ matrix whose $j$th column is the vector $T(e_{j})$ where $e_{j}$ is the $j$th column of the identity matrix in $\Bbb R^n$: $$A=[T(e_{1})\quad \cdots \quad T(e_{n})]$$

> [!definition] Mapping
> A mapping $T:\Bbb R^{n}\to \Bbb R^{m}$ is said to be **onto** $\Bbb R^{m}$ if each $b$ in $\Bbb R^{m}$ is the image of *at least one $x$* in $\Bbb R^{n}$
- $T$ is onto $\Bbb R^m$ when the range of $T$ is all of the codomain $\Bbb R^{m}$
- The mapping $T$ is *not onto* when there is some $b$ in $\Bbb R^{m}$ for which the equation $T(x)=b$ has no solution
	- This occurs when $m>n$
	- ![[Is the range of T all of R^m.png|500]]

- [[Geometric Linear Transformations of R2]]

> [!definition] Mapping
> A mapping $T:\Bbb R^{n}\to \Bbb R^{m}$ is said to be **one-to-one** if each $b$ in $\Bbb R^{m}$ is the image of *at most one $x$* in $\Bbb R^{n}$

- $T$ is *not* one-to-one when some $b$ in $\Bbb R^m$ is the image of more than one vector in $\Bbb R^{n}$
- ![[Is every b the image of at most one vector?.png|500]]

> [!question]- Let T:$$A=\begin{bmatrix}\;\;\;1&-4&\;\;\;8&\;\;\;1 \\ \;\;\;0&\;\;\;2&-1&\;\;\;3 \\ \;\;\;0&\;\;\;0&\;\;\;0&\;\;\;5 \end{bmatrix}$$ Does $T$ map $\Bbb R^{4}$ onto $\Bbb R^{3}$? Is $T$ a one-to-one mapping?
> - $A$ has a pivot position in every row -> consistent
> - It maps $\Bbb R^{4}$ onto $\Bbb R^{3}$
> - $Ax=b$ has free variables -> $b$ is the image of more than one $x$ -> not one-to-one
> - The columns of $A$ are linearly dependent

> [!info] THEOREM 11
> Let $T:\Bbb R^{n}\to\Bbb R^{m}$ be a linear transformation. Then $T$ is one-to-one iff the equation $T(x)=0$ has only the trivial solution.

> [!info] THEOREM 12
> Let $T:\Bbb R^{n}\to\Bbb R^{m}$ be a linear transformation, and let $A$ be the standard matrix for $T$. Then:
> 1. $T$ maps $\Bbb R^{n}$ onto $\Bbb R^{m}$ iff the columns of $A$ span $\Bbb R^{m}$;
> 2. $T$ is one-to-one iff the columns of $A$ are linearly independent.

---
References: