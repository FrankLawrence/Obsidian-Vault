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


# Pivot Positoins
- Since the reduced echelon form is unique, leading entries are always in the same positions in any echelon form obtained from a given matrix

> [!definition] Pivot position
> A pivot position in a matrix $A$ is a location in $A$ that corresponds to a leading $1$ in the reduced echelon form of $A$. A **pivot column** is a column of $A$ that contains a pivot position

- A **pivot** is a nonzero number in a pivot position that is used as needed to create zeros via row operation
# The Row Reduction Algorithm

> [!abstract] General Algorithm
> 1. Begin with the leftmost nonzero column. This is a pivot column. The pivot position is at the top.
> 2. Select a nonzero entry in the pivot column as a pivot. If necessary, interchange rows to move this entry into the pivot position

---
References: