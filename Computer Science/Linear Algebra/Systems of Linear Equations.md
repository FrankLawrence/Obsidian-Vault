---
Tags: 
Created: 2023-04-04 11:35:16
---
(Links:: [[Linear Algebra]])

- A **linear equation** in the variables $x_1,...x_n$ is an equation that can be written in the form $$a_1 x_1+a_2 x_2+\cdots+a_n x_n=b$$ where $b$ and the **coefficients** $a_1,...,a_n$ are real or complex numbers
- A **system of linear equations** is a collection of one or more linear equations involving the same variables.
- A **solution** of the system is a list of numbers that makes each equation a true statement when the values $s_1,...,s_n$ are substituted for $x_1,...,x_n$, respectively
- Two linear systems are called **equivalent** if they have the same solution set (each solution of the first system is a solution of the second system, and inversely)

> [!warning]
> A system of linear equations has either:
> 1. no solution
> 2. exactly one solution
> 3. infinitely many solutions
> 
> A system of linear equations is **consistent** if it has either one solution or infinitely many solutions; a system is **inconsistent** if it has no solution

# Matrix Notation
$$
\begin{aligned} 
x_1-2x_2+\;\;x_3 &=0 \\ 
2x_2-8x_3 &=8 \\ 
5x_1 \;\qquad\;-5x_3 &= 10 
\end{aligned} \tag{1}
$$
$$
\left[
\begin{array}{rrr}
1&-2&1\\
0&2&-8\\
5&0&-5
\end{array}
\right]
$$
is called the **coefficient matrix** of system $(1)$
$$
\left[
\begin{array}{rrrr}
1&-2&1&0\\
0&2&-8&8\\
5&0&-5&10
\end{array}
\right]
$$
is called the **augmented matrix** of the system
- **$m\times n$ matrix** is a rectangular array of numbers with $m$ rows and $n$ columns

# Solving a Linear System
- use $x_1$ term in first equation to eliminate $x_1$ terms in other equations and so on until you obtain a very simple equivalent system of equations

> [!hint] Elementary Row Operations
> 1. **Replacement**: Replace one row by the sum of itself and a multiple of another row
> 2. **Interchange**: Interchange two rows
> 3. **Scaling**: Multiply all entries in a row by a nonzero constant

- two matricies are **row equivalent** if there is a sequence of elementary row operations that transforms one matrix into the other
- If the augmented matrices of two linear systems are row equivalent, then the two systems have the same solution set
# Existence and Uniqueness Questions
> [!question]
> 1. Is the system consistent (Does at least one solution exist)?
> 2. If a solution exists, is it the only one?

- $0x_1+0x_2+0x_3=b$ where $b\neq 0$ is a contradiction
	- the equation $0=b$ is never true, hence the orginal system is inconsistent (it has no solution)

> [!hint]- Reasonable Answers
> Substitute your solution into the original equations. 
> If upon re-checking your arithmetic, you find the correct values, you can be confident you have the correct solution to the system of equations. Otherwise there is an error in the original calculations


---
References: