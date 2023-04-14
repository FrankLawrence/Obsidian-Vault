---
Tags: 
Created: 2023-04-10 23:31:15
---
(Links:: [[Linear Algebra]])

> [!definition] Linear Independence
> An indexed set of vectors $\{v_1,...,v_p\}$ in $\Bbb R^n$ is said to be **linearly independent** if the vector equation $$x_1v_1+x_2v_2+\cdots+x_pv_p=0$$ has only the trivial solution. The set $\{v_1,...,v_p\}$ is said to be **linearly dependent** if there exist weights $c_1,...,c_p$, not all zero, such that $$c_1v_1+c_2v_2+\cdots+c_pv_p=0$$

# Linear Independence of Matrix Columns
- The columns of a matrix $A$ are linearly independent iff the equation $Ax=0$ has *only* the trivial solution
- if the equation $Ax=0$ has only basic variables and no free variables, the only there exists only the trivial solution
# Sets of One or Two Vectors
- A set of two vectors $\{v_1,v_2\}$ is linearly dependent if at least one of the vectors is a multiple of the other. The set is linearly independent iff neither of the vectors is a multiple of the other
# Sets of Two or More Vectors
> [!info] THEOREM 7 
> **Characterization of Linearly Dependent Sets**
> An indexed set $S=\{v_1,...,v_p\}$ of two or more vectors is linearly dependent iff at least one of the vectors in $S$ is a linear combination of the others. In fact, if $S$ is linearly dependent and $v_1\neq0$, then some $v_j$ (with $j>1$) is a linear combination of the preceding vectors $v_j,...,v_{j-1}$

> [!info] THEOREM 8
> If a set contains more vector than there are entries in each vector, then the set is linearly dependent. That is, any set $\{v_1,...,v_p\}$ in $\Bbb R^n$ is linearly dependent if $p>n$
- if $p>n$ then there are more variables than equations -> there is a free variable -> $Ax=0$ has a nontrivial solution

> [!info] THEOREM 9
> If a set $S = \{v_1,...,v_p\}$ in $\Bbb R^n$ contains the zero vector, then the set is linearly dependent.

---
References: