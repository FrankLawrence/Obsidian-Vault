---
Tags: 
Created: 2023-04-06 10:58:57
---
(Links:: [[Linear Algebra]])
> [!definition]
> If $A$ is an $m\times n$ matrix, with columns $a_1,...,a_n$ and if $x$ is in $\Bbb R^n$, then the **product of $A$ and $x$**, denoted by $Ax$, is **the linear combination of the columns of $A$ using the corresponding entries in $x$ as weights**; that is, $$Ax=[a_1\quad a_2 \quad \cdots \quad a_n]\begin{bmatrix}x_1\\\vdots\\x_n\end{bmatrix}=x_1a_1+x_2a_2+\cdots +x_na_n$$

> [!info] THEOREM 3
> If $A$ is an $m\times n$, matrixm with columns, $a_1,...,a_n$, and if $b$ is in $\Bbb R^m$, the matrix equation $$Ax=b$$ has the same solution set as the vector equation $$x_1a_1+x_2a_2+\cdots+x_na_n=b$$ which, in turn, has the same solution set as the system of linear equations whose augmented matrix is $$[a_1\quad a_2\quad \cdots \quad a_n \quad b]$$

# Existence of Solutions

- The equation $Ax=b$ has a solution if and only if $b$ is a linear combination of the columns of $A$

> [!info] THEOREM 4
> Let $A$ be an $m\times n$ matrix. Then the following statements are logically equivalent. That is, for a particular $A$, either they are all true statements or they are all false.
> 1. For each $b$ in $\Bbb R^m$, the equation $Ax=b$ has a solution.
> 2. Each $b$ in $\Bbb R^m$ is a linear combination of the columns of $A$
> 3. The columns of $A$ span $\Bbb R^m$
> 4. $A$ has a **pivot position in every row**

- **Identity matrix**: matrix with 1's on diagonal: $$\begin{bmatrix}1&0&0\\ 0&1&0\\ 0&0&1\end{bmatrix}$$
# Properties of the Matrix-Vector Product $Ax$
> [!info] THEOREM 5
> If $A$ is an $m\times n$ matrix, $u$ and $v$ are vectors in $\Bbb R^n$, and $c$ is a scalar, then:
> 1. $A(u+v)=Au+Av$
> 2. $A(cu)=c(Au)$


---
References: