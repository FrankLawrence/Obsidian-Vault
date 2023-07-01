---
Tags: 
Created: 2023-04-10 23:30:43
---
(Links:: [[Linear Algebra]])
# Homogeneous Linear Systems
- $Ax=0$ is said to be **homogeneous**
	- such a system *always* has one solution $x=0$ called the **trivial solution**
	- $Ax=0$ has a nontrivial solution iff the equation has at least one free variable
- Assume that for a given matrix equation the solution set is described by 
  $$\begin{cases}
  x_1= -\frac43 x_3 \\
  x_2=0\\
  x_3 \text{ is free}
  \end{cases}$$
  As a vector, the general solution of $Ax=0$ has the form
  $$x=\begin{bmatrix}x_1\\ x_2\\ x_3\end{bmatrix}=\begin{bmatrix}\frac43 x_3\\ 0\\ x_3\end{bmatrix}=x_3\begin{bmatrix}\frac43\\ 0\\ 1\end{bmatrix}=x_3v,\qquad \text{where }v=\begin{bmatrix}\frac43\\ 0\\ 1\end{bmatrix}\tag{1}$$

- The solution set of a homogeneous equation $Ax=0$ can always be expressed explicitly as Span$\{v_1,...,v_p\}$ for suitable vectors $v_1,...,v_p$
# Parametric Vector Form
- Equation $(1)$ is called a **parametric vector equation** usually written as $$x=su+tv\quad(x,t \text{ in } \Bbb R)$$ where $u$ and $v$ are vectors
# Solutions of Nonhomogeneous Systems
- The solutions of $Ax=b$ are obtained by adding the vector $p$ to the solutions of $Ax=0$
- adding a vector $p$ to the solution can be thought as a geometric *translation* -> $v$ is **translated by $p$** to $v+p$
- ![[Parallel solution sets of Ax=b and Ax=0.png|300]]
	- $L$ is the line through $0$ and $v$ described by $x=tv\quad (t\text{ in }\Bbb R)$. Adding $p$ to each point on a line $L$ produces the translated line described by the $x=p+tv\quad (t\text{ in }\Bbb R)$

> [!info] THEOREM 6
> Suppose the equation $Ax=b$ is consistent for some given $b$, and let $p$ be a solution. Then the solution set of $Ax=b$ is the set of all vectors of the form $w=p+v_h$, where $v_h$ is any solution of the homogeneous equation $Ax=0$

- the image of the solution set of a consistent system $Ax=b$ (with $b\neq 0$) is either a single nonzero point or a line or plane not passing through the origin

> [!info] Writing a solution set (of a consistent system) in parametric vector form
> 1. Row reduce the augmented matrix to reduced echelon form
> 2. Express each basic variable in terms of any free variables appearing in an equation.
> 3. Write a typical solution $x$ as a vector whose entries depend on the free variables, if any.
> 4. Decompose $x$ into a linear combination of vectors (with numeric entries) using the free variables as parameters.

> [!tip] Reasonable Answers
> To verify that the solutions you found are indeed solutions to $Ax=b$ or the homogeneous equation $Ax=0$, simply multiply the matrix by each vector in your solution and check that the result is the zero vector or $b$
> 	for $Ax=b$ the product of $A$ with the vector *not* part of the solution to the homogeneous equation should be $b$, the others $0$

---
References: