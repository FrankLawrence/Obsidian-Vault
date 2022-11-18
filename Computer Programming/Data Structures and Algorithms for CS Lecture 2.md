---
Tags: lecture
Created: 2022-11-15 03:32:49
---
(Links:: [[Data Structures and Algorithms for CS]])
# Merge sort
## Worst-case running time
- calculations of indicies in elementary time
- for-loop of length $n_1 = q-p+1$ and for-loop of length $n_2 =r-q$
	- total in $n_1 + n_2 = r-p+1$ steps (length we consider of the part of A)
	- for-loop with $r-p+1$ steps (comparisons)
- **running time of merge in $\Theta(n)$ with $n=r-p+1$**
- $T_{merge}(n)=c\cdot n$
## Worst-case running time via tree analysis
- **merging $n$ elements has cost $T_{merge}(n)=c\cdot n$ and stop has cost $d$**
- height is $\log n$ for $n$ the length of the input-array
- number of levels is $(\log n)+1$
- number of leaves is $n$
- **Total:** $c\cdot n \cdot \log n \; \text{(upper log n levels)}+ d \cdot n \; \text{(from leaves)}$ 
- **Worst-case running time of merge sort is in $\Theta(n\log n)$**
## Solving recurrence equation
- divide is easy, in $\Theta(1)$, cost $d$
- conquer uses recursion, two subproblems of size half
- combine is difficult, in $\Theta(n)$
- first assume $n=2^i$

$$\begin{align}
T(n) & = 2T\left(\frac{n}{2}\right)+c\cdot n \\
& = 2 \cdot \left(2 \cdot T\left(\frac{n}{4}\right)+ c\cdot \frac{n}{2}\right)+c\cdot n \\
& = 4 \cdot T \left(\frac{n}{4}\right)+2\cdot c\cdot n \\
& = 4 \cdot \left(2 \cdot T\left(\frac{n}{8}\right)+ c\cdot \frac{n}{4}\right)+2\cdot c\cdot n \\
& = 8 \cdot T \left(\frac{n}{8}\right)+3\cdot c\cdot n \\
& = \dots \\
& = 2i \cdot T\left(\frac{n}{2^i}\right)+i\cdot c\cdot n
\end{align}$$
base case: if $\frac{n}{2^i} = 1$ so $i = \log n$; substitute this:
$T(n) = n\cdot T(1) + (\log n) \cdot c \cdot n = n \cdot d + (\log n) \cdot c \cdot n = c \cdot n \cdot \log n + d\cdot n$
**$T$ is in $\Theta(n\log n)$**

**drawback: space complexity**

---
References: