---
Tags: Math
Created: 2023-06-30 22:45:56
---
(Links:: [[Linear Algebra]])
# Cramer's Rule

- For any $n\times n$ matrix $A$ and any $b$ in $\Bbb R^n$, let $A_i(b)$ be the matrix obtained from $A$ by replacing column $i$ by the vector $b$. $$A_i(b)=\begin{bmatrix}a_{1}&\cdots&b&\cdots&a_{n}\end{bmatrix}$$

> [!info] THEOREM 7
> **Cramer's Rule**
> Let $A$ be an invertible $n\times n$ matrix. For any $b$ in $\Bbb R^{n}$, the unique solution $x$ of $Ax=b$ has entries given by $$x_{i}=\frac{\text{det }A_i(b)}{\text{det }A},\qquad i=1,2,...,n$$

^df6ce0

# A Forumla for $A^{-1}$
- **TODO**
# Determinants as Area or Volume
> [!info] THEOREM 9
> If $A$ is a $2\times2$ matrix, the area of the parallelogram determined by the columns of $A$ is $|\text{det }A|$. If $A$ is a $3\times3$ matrix, the volume of the parallelepiped determined by the columns of $A$ is $|\text{det }A|$.

$$\left|\text{det }\begin{bmatrix}a&0\\0&d\end{bmatrix}\right| = |ad| = \{\text{area of rectangle}\}$$

# Linear Transformations
> [!info] THEOREM 10
> Let $T:\Bbb R^{2}\to \Bbb R^{2}$ be the linear transformation determined by a $2\times2$ matrix $A$. If $S$ is a parallelogram in $\Bbb R^{2}$, then $$\{\text{area of }T(S)\}=|\text{det }A|\cdot \{\text{area of }S\}$$
> If $T$ is determined by a $3\times3$ matrix $A$, and if $S$ is a parallelepiped in $\Bbb R^{3}$, then $$\{\text{volume of }T(S)\}=|\text{det }A|\cdot \{\text{volume of }S\}$$


---
References: