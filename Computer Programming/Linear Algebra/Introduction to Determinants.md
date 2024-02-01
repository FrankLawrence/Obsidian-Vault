---
Tags: 
Created: 2023-06-30 00:06:40
---
(Links:: [[Linear Algebra]])
> [!definition] Determinant
> For $n\geq 2$, the **determinant** of an $m\times n$ matrix $A=[a_{ij}]$ is the sum of $n$ terms of the form $\pm a_{1j}\text{ det }A_{1j}$, with plus and minus signs alternating, where the entries $a_{11},a_{12},...,a_{1n}$ are form the first row of $A$. In symbols, $$\begin{align}\text{det }A&=a_{11}\text{ det }A_{11}-a_{12}\text{ det }A_{12}+\cdots +(-1)^{1+n}\text{ det } A_{1n} \\ &=\sum\limits_{j=1}^{n}(-1)^{1+j}a_{1j}\text{ det }A_{1j}\end{align}$$

- Given $A=[a_ij]$, the **$(i,j)$-cofactor** of $A$ is the number $C_{ij}$ given by $$C_{ij}=(-1)^{i+j}\text{ det }A_{ij} \tag{1}$$ ^ff8c79
- Then $$\text{det }A=a_{11}C_{11}+a_{12}C_{12}+\cdots+a_{1n}C_{1n}$$

> [!info] THEOREM 1
> The determinant of an $n\times n$ matrix $A$ can be computed by a cofactor expansion across any row or down any column. The expansion across the $i$th row using the cofactors in [[#^ff8c79|(1)]] is $$\text{det }A=a_{i1}C_{i1}+a_{i2}C_{i2}+\cdots+a_{in}C_{in}$$
> The cofactor expansion down the $j$th column is $$\text{det }A=a_{1j}C_{1j}+a_{2j}C_{2j}+\cdots+a_{nj}C_{nj}$$

- The factor $(-1)^{i+j}$ determines the following checkerboard pattern of signs: $$\begin{bmatrix}+&-&+&\cdots \\ -&+&- \\ +&-&+ \\ \vdots&&&\ddots\end{bmatrix}$$

> [!info] THEOREM 2
> If $A$ is a triangular matrix, then $\text{det }A$ is the product of the entries on the main diagonal of $A$

- $\text{det }I=1$


---
References: