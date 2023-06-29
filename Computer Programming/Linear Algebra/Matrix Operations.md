---
Tags: 
Created: 2023-04-10 23:38:45
---
(Links:: [[Linear Algebra]])
- A **diagonal matrix** is a square $n\times n$ matrix whose non-diagonal entries are zero.
# Sums and Scalar Multiples
> [!info] THEOREM 1
> Let $A$, $B$, and $C$ be matrices of the same size, and let $r$ and $s$ be scalars.
> 1. $A+B=B+A$
> 2. $(A+B)+C=A+(B+C)$
> 3. $A+0=A$
> 4. $r(A+B)=rA+rB$
> 5. $(r+s)A=rA+sA$
> 6. $r(sA)=(rs)A$
# Matrix Multiplication
> [!definition] Matrix multiplication
> If $A$ is an $m\times n$ matrix, and if $B$ is an $n\times p$ matrix with collumns $b_{1},...,b_{p}$, then the product $AB$ is the $m\times p$ matrix whose columsn are $Ab_{1},...,Ab_{p}$. That is, $$AB=A\begin{bmatrix}b_1&b_2&\cdots&b_{p}\end{bmatrix} = \begin{bmatrix}Ab_{1}&Ab_{2}&\cdots&Ab_{p}\end{bmatrix}$$

- Each column of $AB$ is a linear combination of the columns of $A$ using weights from the corresponding column of $B$
- **$AB$ has the same number of rows as $A$ and the same number of columns as $B$**
# Properties of Matrix Multiplication
> [!info] THEOREM 2
> Let $A$ be an $m\times n$ matrix, and let $B$ and $C$ have sizes for which the indicated sums and products are defined.
> 1. $A(BC) = (AB)C$
> 2. $A(B+C) = AB+AC$
> 3. $(B+C)A = BA+CA$
> 4. $r(AB)=(rA)B=A(rB)$ for any scalar $r$
> 5. $I_{m}A=A=AI_{n}$

# Powers of a Matrix
- $A^{k}$ denotes the product of $k$ copies of $A$
- $A^0$ is interpreted as the identity matrix

# The Transpose of a Matrix
- Given an $m\times n$ matrix $A$, the **transpose** of $A$ is the $n\times m$ matrix, denoted by $A^{T}$

> [!info] THEOREM 3
> Let $A$ and $B$ denote matrices whose sizes are appropriate for the following sums and products.
> 1. $(A^{T})^{T}=A$
> 2. $(A+B)^{T}=A^{T}+B^{T}$
> 3. For any scalar $r$, $(rA)^{T}=rA^{T}$
> 4. $(AB)^{T}=B^{T}A^{T}$

- The transpose of a product of matrices equals the product of their transposes in the *reverse* order

---
References: