---
Tags: Math
Created: 2023-07-01 10:39:28
---
(Links:: [[Linear Algebra]])
# The Null Space of a Matrix
- The set of all $x$ that satisfy $Ax=0$ is called the **solution set**

> [!definition] Null space
> The null space of an $m\times n$ matrix $A$, written as $\text{Nul }A$, is the set of all solutions of the homogeneous equation $Ax=0$. In set notation, $$\text{Nul }A=\{x:x\text{ is in }\Bbb R^{n}\text{ and }Ax=0\}$$

> [!info] THEOREM 2
> The null space of an $m\times n$ matrix $A$ is a subspace of $\Bbb R^n$. Equivalently, the set of all solutions to a system $Ax=0$ of $m$ homogeneous linear equations in $n$ unknowns is a subspace of $\Bbb R^n$
# An Explicit Description of $\text{Nul }A$
> [!example] Finding a spanning set for the null space of a matrix $A$
> Write the solution set of $Ax=0$ in [[Solution Sets of Linear Systems#Parametric Vector Form|parametric vector form]] to get the vectors that span the set for $\text{Nul }A$

- Finding the spanning set automatically produces a linearly independent set
- The number of vectors in the spanning set for $\text{Nul }A$ equals the number of free variables in the equation $Ax=0$
# The Column Space of a Matrix
> [!definition] Column space
> The column space of an $m\times n$ matrix $A$, written as $\text{Col }A$, is the set of all linear combinations of the columns of $A$. If $A=\begin{bmatrix}a_{1} & \cdots & a_{n}\end{bmatrix}$, then $$\text{Col }A=\text{Span }\{a_{1},...,a_{n}\}$$

> [!info] THEOREM 3
> The column space of an $m\times n$ matrix $A$ is a subspace of $\Bbb R^m$

- $\text{Col }A$ is the **range** of the linear transformation $x\mapsto Ax$
- The column space of an $m\times n$ matrix $A$ is all of $\Bbb R^m$ iff the equation $Ax=b$ has a solution for each $b$ in $\Bbb R^{m}$
# The Row Space
- The set of all linear combinations of the row vectors is called the **row space** of $A$ is denoted by $\text{Row }A$ and can also be written as $\text{Col }A^T$
# [[Contrast Between Nul A and Col A for an m × n Matrix A]]
# Kernel and Range of a Linear Transformation
> [!definition] Linear Transformation
> A linear transformation $T$ from a vector space $V$ into a vector space $W$ is a rule that assigns to each vector $x$ in $V$ a unique vector $T(x)$ in $W$, such that
> 1. $T(u+v)=T(u)+T(v)\qquad\text{for all } u,v\text{ in } V,\text{ and}$
> 2. $T(cu)=cT(u)\qquad\qquad\qquad \text{for all } u\text{ in } V,\text{ and all scalars }c$

- ![[Subspaces associated with a linear transformation.png|500]]
	- **kernel**(**null space**) of $T$: set of all $u$ in $V$ such that $T(u)=0$
	- **range** of $T$: set of all vectors in $W$ of the form $T(x)$ for some $x$ in $V$
	- If $T(x)$ is a matrix transformation, then the kernel and the range are the null space and the column space of $A$

---
References: