---
Tags: 
Created: 2023-04-05 22:35:10
---
(Links:: [[Linear Algebra]])
- **leading entry**: the leftmost nonzero entry

> [!definition] Echelon Form
> A rectangluar matrix is in echelon form if it has the following three properties:
> 1. All nonzero rows are above any rows of all zeros
> 2. Each leading entry of a row is in a column to the right of the leading entry of the row above it
> 3. All entries in a column below a leading entry are zeros
> 
> > [!definition] Reduced Echelon Form
> > If a matrix in echelon satisfies the following additional conditions, then it is in **reduced echelon form**:
> > 1. The leading entry in each nonzero row is 1
> > 2. Each leading 1 is the only nonzero entry in its column

> [!example]- Echelon Form
> Leading entries ($\bullet$) may have any nonzero value
> starred entries ($*$) may have any value
> $$\left[
> \begin{array}{}
> \bullet&*&*&*\\
> 0&\bullet&*&*\\
> 0&0&0&0\\
> 0&0&0&0
> \end{array}
> \right]$$
> $$\left[
> \begin{array}{}
> 0&\bullet&*&*&*&*&*&*&*&*\\
> 0&0&0&\bullet&*&*&*&*&*&*\\
> 0&0&0&0&\bullet&*&*&*&*&*\\
> 0&0&0&0&0&\bullet&*&*&*&*\\
> 0&0&0&0&0&0&0&0&\bullet&*
> \end{array}
> \right]$$


> [!example]- Reduced Echelon Form
> Reduced echelon form has 0's below and above each leading 1
> $$\left[
> \begin{array}{}
> 0&1&*&0&0&0&*&*&0&*\\
> 0&0&0&1&0&0&*&*&0&*\\
> 0&0&0&0&1&0&*&*&0&*\\
> 0&0&0&0&0&1&*&*&0&*\\
> 0&0&0&0&0&0&0&0&1&*
> \end{array}
> \right]$$

> [!Info] THEOREM 1
> **Uniqueness of the Reduced Echelon Form**
> Each matrix is row equivalent to one and only one reduced echelon matrix


# Pivot Positions
- Since the reduced echelon form is unique, leading entries are always in the same positions in any echelon form obtained from a given matrix

> [!definition] Pivot position
> A pivot position in a matrix $A$ is a location in $A$ that corresponds to a leading $1$ in the reduced echelon form of $A$. A **pivot column** is a column of $A$ that contains a pivot position

- A **pivot** is a nonzero number in a pivot position that is used as needed to create zeros via row operation
# The Row Reduction Algorithm

> [!abstract] General Algorithm
> 1. Begin with the leftmost nonzero column. This is a pivot column. The pivot position is at the top.
> 2. Select a nonzero entry in the pivot column as a pivot. If necessary, interchange rows to move this entry into the pivot position
> 3. Use row replacement operations to create zeros in all positions below the pivot
> 4. Cover (or ignore) the row containing the pivot position and cover all rows, if any, above it. Apply steps 1-3 to the submatrix that remains. Repeat the process until there are no more nonzero rows to modify
> 5. Beginning with the rightmost pivot and working upward and to the left, create zeros above each pivot. If a pivot is not 1, make it 1 by scaling operation
> steps 1-4 are the *forward phase*; step 5 is the *backward phase*

# Solutions of Linear Systems
$$
\left[
\begin{array}{rrrr}
1&0&-5&1\\
0&1&1&4\\
0&0&0&0
\end{array}
\right]
$$
The associated system of equations is 
$$
\begin{aligned}
x_1-5x_3&=1\\
x_2+x_3&=4\\
0&=0
\end{aligned} \tag{a}
$$
- the variables $x_1$ and $x_2$ are **basic variables**, $x_3$ is a **free variable**
- solve *reduced* system of equations for basic variables in terms of free variables: 
$$
\begin{cases}
x_1=1+5x_3\\
x_2=4-x_3 \\
x_3 \text{ is free}
\end{cases}
$$
- $x_3$ can have any value
- *each different choice of $x_3$ determines a (different) solution of the system, and every solution of the system is determined by a choice of $x_3$*

## Parametric Descriptions of Solution Sets
- *parametric descriptions*: solution sets have free variables that act as parameters
- Whenever a system is consistent and has free variables, the solution set has many parametric descriptions
- alternative equation system for $\eqref{a}$:
$$
\begin{aligned}
x_1+5x_2&=21\\
x_2+x_3&=4\\
\end{aligned}
$$

# Back-Substitution
- solve the last equation for $x_n$ and substitute the expression $x_n$ into the second last equation, and so on

# Existence and Uniqueness Questions
- with free variables the solution is *not unique*
	- each different choice of the parameters determines a different solution -> infinitely many solutions

> [!info] THEOREM 2
> **Existence and Uniqueness Theorem**
> A linear system is consistent if and only if the rightmost column of the augmented matrix is *not* a pivot column-that is, if and only if an echelon form of the augmented matrix has *no* row of the form $$[0\;\cdots\; 0 \quad b]\qquad \text{ with $b$ nonzero}$$
> If a linear system is consistent, then the solution set constains either (i) a unique solution, when there are no free variables, or (ii) inifinitely many solutions, when there is at least one free variable

---
References: