---
Tags: 
Created: 2023-07-04 18:35:52
---
(Links:: [[Linear Algebra]])
1. [x] Find a basis for the Column Space of A.
	- Row reduce $Ax$ and find the vectors which are a linear combination of the others
	- The basis consists of the vectors in the pivot columns
2. [x] Find a basis for the Null Space of A.
	- Solve the equation $Ax=0$. The basis is a set with the vectors from the parametric vector form of the solution set
3. [x] Find the $x$ and $z$ components of the solution of the system by using [[Cramer's Rule, Volume, and Linear Transformations#^df6ce0|Cramer’s rule]].
4. [x] Give the volume of the parallelepiped generated by the columns of the coefficient matrix of the system.
	- $\text{det }A$
5. [x] Find the inverse of a coefficient matrix of a system.
	- Row reduce $[AI]$ to $[IA^{-1}]$
	- $A^{-1}=\frac{1}{\text{det }A}\text{adj }A$
6. [x] Show that $V$ is a subspace of a vector space based on properties of the entires of the vectors.
	- Prove that (1) $V$ contains the $\vec 0$ vector, (2) $V$ is closed under vector addition (3) $V$ is closed under multiplication by scalar

$$\begin{bmatrix}s+3t \\ s-t \\ 2s-t \\ 4t\end{bmatrix}=s\begin{bmatrix}1 \\ 1 \\ 2 \\ 0\end{bmatrix}+t\begin{bmatrix}3 \\ 1 \\ -1 \\ 4\end{bmatrix}$$
1. [x] Show that $v_1$ is an eigenvector of $A$ and find its corresponding eigenvalue $\lambda_1$
	- Multiply $A$ with $v_1$ to find $\lambda_{1}v_{1}$
2. [x] [[The Characteristic Equation#^26dc57|Determine the remaining eigenvalues of A]] and give a [[Eigenvectors and Eigenvalues#^824f22|basis for every eigenspace]] of $A$.
3. [x] Determine whether $A$ is diagonalizable. If so, give the diagonalization, i.e., an invertible matrix $P$ and a diagonal matrix $D$ such that $A = PDP−1$. If not, explain why $A$ is not diagonalizable.
	- $A$ is diagonalizable iff there are $n$ distinct eigenvalues
	- $D$ contains the eigenvalues along the diagonal (easier to computer powers with)
	- $P$ contains the eigenvectors (belonging to the correct eigenvalues)
4. [x] Use the given basis to produce an orthonormal basis for $\Bbb R^3$.
$$\begin{bmatrix} 1 \\ -4 \\ 0 \\ 1 \end{bmatrix},\begin{bmatrix} 7 \\ -7 \\ -4 \\ 1 \end{bmatrix}$$
$$v_{1}=x_{1} = \begin{bmatrix} 1 \\ -4 \\ 0 \\ 1 \end{bmatrix}$$
$$v_{2}=\begin{bmatrix} 7 \\ -7 \\ -4 \\ 1 \end{bmatrix}-\frac{36}{18}\begin{bmatrix} 1 \\ -4 \\ 0 \\ 1 \end{bmatrix}=\begin{bmatrix}5 \\ 1 \\ -4 \\ -1 \end{bmatrix}$$
$$u_{1}=\frac{1}{\mid\mid v_{1} \mid\mid}v_{1}=\frac{1}{\sqrt{18}}\begin{bmatrix}1 \\ -4 \\ 0 \\ 1\end{bmatrix}=\begin{bmatrix} 1/\sqrt{18} \\ -4/\sqrt{18} \\0 \\ 1/\sqrt{18} \\\end{bmatrix}$$
$$u_{2}=\frac{1}{\mid\mid v_{2} \mid\mid}v_{2}=\frac{1}{\sqrt{43}}\begin{bmatrix}5 \\ 1 \\ -4 \\ -1\end{bmatrix}=\begin{bmatrix} 5/\sqrt{43} \\ 1/\sqrt{43} \\-4/\sqrt{43} \\ -1/\sqrt{43} \\\end{bmatrix}$$

11. [x] Find the distance between $v_3$ and $\text{Span }\{v_1,v_2\}$.

Let $v_1 = (1, 2, 1)$, $v_2 = (0, 1, 1)$, and $v_3 = (2, 3, 4)$. Find the distance between the vector $v_3$ and the subspace $\text{Span }\{v_1, v_2\}$.

Solution:
To find the distance between a vector and a subspace, we need to find the orthogonal projection of the vector onto the subspace and then calculate the distance between the vector and its projection.

Step 1: Find an orthonormal basis for the subspace $\text{Span }\{v_1, v_2\}$ using the Gram-Schmidt process.

$$v_1 = \begin{bmatrix}1  \\ 2 \\ 1\end{bmatrix}$$
$$v_2 = \begin{bmatrix}0  \\ 1 \\ 1\end{bmatrix} - \frac{3}{6}\begin{bmatrix}1  \\ 2 \\ 1\end{bmatrix} = \begin{bmatrix}-1/2  \\ 0 \\ 1/2\end{bmatrix}$$

Normalize $u_1$ and $u_2$:

$$u_1 = \frac{v_1}{||v_1||} = \begin{bmatrix}1/\sqrt{6} \\  2/\sqrt{6} \\  1/\sqrt{6}\end{bmatrix}$$
$$u_2 = \frac{v_2}{||v_2||} = \begin{bmatrix}-1/\sqrt{2}  \\ 0 \\  1/\sqrt{2}\end{bmatrix}$$

Step 2: Find the orthogonal projection of $v_3$ onto $\text{Span }\{v_1, v_2\}$ using the orthonormal basis $\{e_1, e_2\}$.

$$\begin{align}
v_3 &= \begin{bmatrix}2\\3\\4\end{bmatrix}-(x_3 \cdot u_1)u_1 - (x_3 \cdot u_2)u_{2}\\
&= \begin{bmatrix}2\\3\\4\end{bmatrix}-\frac{12}{\sqrt{6}} \begin{bmatrix}1/\sqrt{6} \\  2/\sqrt{6} \\  1/\sqrt{6}\end{bmatrix} - \frac{2}{\sqrt{2}}\begin{bmatrix}-1/\sqrt{2}  \\ 0 \\  1/\sqrt{2}\end{bmatrix}\\
&= \begin{bmatrix}2\\3\\4\end{bmatrix}-\begin{bmatrix}2 \\ 4 \\ 2\end{bmatrix} - \begin{bmatrix} -1 \\ 0 \\ 1\end{bmatrix} = \begin{bmatrix}1 \\ 0 \\ 1\end{bmatrix}
\end{align}$$

Step 3: Calculate the distance between $v_3$ and its projection.

$$dist(v_{3},x_{3})=\sqrt{(2-1)^{2}+(3-0)^{2}+(4-1)^2}=\sqrt{19}$$

1. [x] The map $T$ defined by $T ((x1 , x2 )) = ( x1 + x2 , x1 − x2 , 2 )$ is linear.
	- Test if $$T(x_1+x_2,y_1+y_2)=T(x_1,y_1)+T(x_2,y_2)$$ and $$T(cx,cy)=cT(x,y)$$
$$T((x_{1}+x_{2}), (y_{1}+y_{2}))=T(x_{1}+x_{2}, x_{1}+x_{2}+y_{1}+y_{2}, 2)$$
$$\begin{align}
T(x_{1},y_{1})+T(x_{2},y_{2}) & =(x_{1},x_{1}+y_{1},2)+(x_{2},x_{2}+y_{2},2) \\
& =T(x_{1}+x_{2},x_{1}+x_{2}+y_{1}+y_{2},4) \\
& \neq T(x_{1}+x_{2}, x_{1}+x_{2}+y_{1}+y_{2}, 2)
\end{align}$$
$$T\left(\begin{bmatrix}x\\ y \end{bmatrix}\right)=\begin{bmatrix}x+y \\ 2x \\ 3y \\ 2x+3y \end{bmatrix}=x\begin{bmatrix}1 \\ 2 \\ 0 \\ 2 \end{bmatrix}=y\begin{bmatrix}1 \\ 0 \\ 3 \\ 3 \end{bmatrix}\to A=\begin{bmatrix}1 & 1 \\ 2 & 0 \\ 0 & 3 \\ 2 & 3 \end{bmatrix}$$
1. [x] Finding the standard matrix $A$ of $T$
2. [x] Determine the LU-factorization of a coefficient matrix
3. [x] Let $x=\begin{bmatrix}1\\0\\2\end{bmatrix}$. Find $[x]_\mathcal{B}$ where $\mathcal{B}$ is the basis made out of the columns of a coefficient matrix $B$
	- $B[x]_\mathcal{B}=x\;\to\;[x]_\mathcal{B}=B^{-1}x$
4. [x] Find an orthogonal basis for $W$
	- Use Gram-Schmidt algorithm
5. [ ] Find the best approximation of $y=\begin{bmatrix}3\\-7\\2\\3\end{bmatrix}\notin W$ by vectors of $W$

$$W=Span\left\{\begin{bmatrix}2 \\ -1 \\ -3 \\ 1\end{bmatrix},\begin{bmatrix}1 \\ 1 \\ 0 \\ -1\end{bmatrix}\right\}$$
$$v_1=\begin{bmatrix}1 \\ 1 \\ 0 \\ -1\end{bmatrix} \quad v_2=\begin{bmatrix}2 \\ -1 \\ -3 \\ 1\end{bmatrix}$$
$$\begin{align}v_{3}&=\begin{bmatrix}3 \\ -7 \\ 2 \\ 3\end{bmatrix}-\frac{10}{15}\begin{bmatrix}2 \\ -1 \\ -3 \\ 1\end{bmatrix}+ \frac{7}{3}\begin{bmatrix}1 \\ 1 \\ 0 \\ -1\end{bmatrix}\\
& = \begin{bmatrix}11/3 \\ -16/3 \\ 3 \\ -2/3\end{bmatrix}
\end{align}$$

18. [x] Determine if $\{v_{1},v_{2}\}$ is a basis for $\Bbb R^3$
	- $A$ doesn't have a pivot position in every row -> doesn't span $\Bbb R^3$

