---
Tags: Math
Created: 2023-04-10 23:40:13
---
(Links:: [[Linear Algebra]])
- factorization of a matrix $A$ is an equation that expresses $A$ as a product of two or more matrices
# The LU Factorization
- Assume that $A$ is an $m\times n$ matrix that can be row reduced to echelon form without row interchanges
- $A$ can be written in the form $A=LU$ where 
	- $L$ is an $m\times m$ lower triangular matrix with 1's on the diagonal and
	- $U$ is an $m\times n$ echelon form of $A$
- $L$ is invertible and is called a *unit* lower triangular matrix
- LU factorization: $$A=\underset{L}{\begin{bmatrix} 1&0&0&0 \\ *&1&0&0 \\ *&*&1&0 \\ *&*&*&1 \end{bmatrix}}\underset{U}{\begin{bmatrix} \blacksquare&*&*&*&* \\ 0&\blacksquare&*&*&* \\ 0&0&0&\blacksquare&* \\ 0&0&0&0&0 \end{bmatrix}}$$

# An LU Factorization Algorithm
> [!tip] Algorithm for an LU factorization
> 1. Reduce $A$ to an echelon form $U$ by a sequence of row replacement operations, if possible.
> 2. Place entries in $L$ such that the *same sequence of row operations* reduces $L$ to $I$.

> [!example] Finding an LU factorization
> Let $$A=\begin{bmatrix}\;\;\;2&\;\;\;4&-1&\;\;\;5&-2 \\ -4&-5&\;\;\;3&-8&\;\;\;1 \\ \;\;\;2&-5&-4&\;\;\;1&\;\;\;8 \\ -6&\;\;\;0&\;\;\;7&-3&\;\;\;1\end{bmatrix}$$
> - *highlight the entries* in each matrix that are used to determine the sequence of row operations that transform $A$ into $U$
> $$A=\begin{bmatrix}
> \;\;\;2&\;\;\;4&-1&\;\;\;5&-2 \\ 
> -4&-5&\;\;\;3&-8&\;\;\;1 \\ 
> \;\;\;2&-5&-4&\;\;\;1&\;\;\;8 \\ 
> -6&\;\;\;0&\;\;\;7&-3&\;\;\;1
> \end{bmatrix} 
> \sim 
> \begin{bmatrix}
> \;\;\;2&\;\;\;4&-1&\;\;\;5&-2 \\ 
> \;\;\;0&\;\;\;3&\;\;\;1&\;\;\;2&-3 \\ 
> \;\;\;0&-9&-3&-4&\;10 \\ 
> \;\;\;0&\;12&\;\;\;4&\;12&-5
> \end{bmatrix}$$
> $$\sim
> \begin{bmatrix}
> \;\;\;2&\;\;\;4&-1&\;\;\;5&-2 \\ 
> \;\;\;0&\;\;\;3&\;\;\;1&\;\;\;2&-3 \\ 
> \;\;\;0&\;\;\;0&\;\;\;0&\;\;\;2&\;\;\;1 \\ 
> \;\;\;0&\;\;\;0&\;\;\;0&\;\;\;4&\;\;\;7
> \end{bmatrix}
> \sim
> \begin{bmatrix}
> \;\;\;2&\;\;\;4&-1&\;\;\;5&-2 \\ 
> \;\;\;0&\;\;\;3&\;\;\;1&\;\;\;2&-3 \\ 
> \;\;\;0&\;\;\;0&\;\;\;0&\;\;\;2&\;\;\;1 \\ 
> \;\;\;0&\;\;\;0&\;\;\;0&\;\;\;0&\;\;\;5
> \end{bmatrix}$$
> At each pivot column, divide the entries by the pivot and place the result into $L$:
> $$L=\begin{bmatrix}
> \;\;\;1&\;\;\;0&\;\;\;0&\;\;\;0  \\ 
> -2&\;\;\;1&\;\;\;0&\;\;\;0 \\ 
> \;\;\;1&-3&\;\;\;1&\;\;\;0 \\ 
> -3&\;\;\;4&\;\;\;2&\;\;\;1 \\ 
> \end{bmatrix}$$

---
References: