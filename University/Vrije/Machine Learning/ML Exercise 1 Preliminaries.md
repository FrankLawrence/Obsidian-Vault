---
title: Week 1 Answers
tags: [math, machine-learning, linear-algebra, calculus, probability]
---

This note reviews key mathematical concepts for machine learning: **Linear Algebra, Calculus, and Probability Theory**. We focus on **sums, expectations, logarithms**, and basic **vector/matrix operations**.

---

# Sums, Expectations, and Logarithms

## Sums

Sigma ($\sum$) notation represents summation:

$$ \sum_{i=1}^{5} a_i = a_1 + a_2 + a_3 + a_4 + a_5 $$

$$ \sum_{i,j} a_i b_j = \sum_i \sum_j a_i b_j $$

> [!question] Question 1
> Compute the following given $x_1 = 5, x_2 = 1, x_3 = 4, x_4 = 1, x_5 = 3$:
> - $\sum_i x_i$
> - $\sum_i 2x_i$
> - $2 \sum_i x_i$
> - $\sum_{i=1}^{3} x_i$
> - $\sum_{i=1}^{x_5} x_i$
> - $\sum_{i=1}^{3} i x_i$

## Expectations

Expectation is a weighted sum:

$$
\mathbb{E}[V] = \sum_i p_i v_i
$$

$$
\mathbb{E}[cV] = c \mathbb{E}[V]
$$

> [!question] Question 3
> Prove the properties of expectation:
> - $\mathbb{E}[cV] = c\mathbb{E}[V]$ (homogeneity)
> - $\mathbb{E}[V] + \mathbb{E}[W] = \mathbb{E}[V+W]$ (additivity)
> - $\mathbb{E}[(V - \mathbb{E}[V])^2] = \mathbb{E}[V^2] - (\mathbb{E}[V])^2$

## Logarithms

Logarithms are the inverse of exponentiation:

$$
\log_b(ab) = \log_b a + \log_b b
$$

$$
\log_b (a/b) = \log_b a - \log_b b
$$

> [!question] Question 4
> Which of the following are true for all $a, b$?  
> - $\log_b (b^a) = a$  
> - $b^{\log_b a} = a$  
> - $\log_c a + \log_c b = \log_c(ab)$  
> - $\log(a+b) = \log(a) \cdot \log(b)$ (counterexample needed)

---

# Linear Algebra

Vectors are ordered lists of numbers. Example vector:

$$
\mathbf{x} = \begin{bmatrix} 3 \\ 3.1 \\ -2.5 \\ 0.1 \end{bmatrix}
$$

The **dot product** of two vectors:

$$
\mathbf{x} \cdot \mathbf{y} = \sum_i x_i y_i
$$

Matrix multiplication generalizes the dot product:

$$
AB = C \quad \text{where} \quad C_{ij} = \sum_k A_{ik} B_{kj}
$$

> [!question] Question 5
> Explain these notations:
> - $f: \mathbb{R}^3 \to \mathbb{R}^2$
> - $y = Wx$
> - $z = y^T x$
> - $W \in \mathbb{R}^{5 \times 4}$

> [!question] Question 6 
> Which operations are valid?
> - Multiplying a $5 \times 4$ matrix by another $5 \times 4$ matrix  
> - Element-wise multiplication of $5 \times 4$ matrices  
> - Multiplying a $5 \times 4$ matrix by a $4 \times 1$ matrix  
> - Multiplying a matrix by its transpose  

> [!question]- Question 7
> Which of the following is true?
> 1. Let $U=VW$ for matrices $U,V,W$. $U_{ij}$ is the product of the $i$-th column of $V$ and the $j$-th column of $W$. **False**
> 2. Let $U=VW$ for matrices $U,V,W$. $U_{ij}$ is the product of the $i$-th row of $V$ and the $j$-th column of $W$. **True**
> 3. Matrix multiplication is *commutative*. **False. It is not always true that $VW=WV$**
> 4. Matrix multiplication is *distributive*. **True, Multiplication can always be distributed over a sum: $U(V +W) = UV +UW$ and $(V +W)U = VU + WU$. Note that the order needs to be maintained: $U(V + W) = VU + WU$ does not hold for all matrices.**
> 5. Matrix multiplication is associative. **True. Multiplying $U$ by $V$ and multiplying the result by $W$ is the same as multiplying $U$ by the result of multiplying $V$ by $W$: $(UV)W) = U(VW)$**
> 6. There exist matrices $U$ and $V$ such that $UV = VU$. **True. For instance, if one of them is the identity matrix, this always holds**

> [!question]- Question 8
> 1. The linear function $f({\color{#F08}a}, {\color{#58F}b}, {\color{orange}c})=\alpha {\color{#F08}a} + \beta{\color{#58F}b} +\gamma{\color{orange}c}$ can be written as the dot product of two vectors. What are the vectors? **If $x=({\color{#F08}a}, {\color{#58F}b}, {\color{orange}c})^{\top}$ and $w=(\alpha, \beta, \gamma)^{\top}$, then $f(x)=w^{\top} x$** 
> 2. The linear function $f({\color{#F08}a}, {\color{#58F}b}, {\color{orange}c})=\alpha {\color{#F08}a} + \beta{\color{#58F}b} +\gamma{\color{orange}c}+\delta$ can also be written as the dot product of two vectors. What are the vectors? **If $x=({\color{#F08}a}, {\color{#58F}b}, {\color{orange}c},1)^{\top}$ and $w=(\alpha, \beta, \gamma, \delta)^{\top}$, then $f(x)=w^{\top} x$** 
> 3. The *quadratic* function $f({\color{#58F}a}, {\color{orange}b})=\alpha{\color{#58F}a}^{2}+\beta{\color{#58F}a}{\color{orange}b}+\gamma {\color{orange}b}{\color{#58F}a} + \delta {\color{orange}b}^{2}$ can be written as $f(x)=x^{\top}Wx$ (also known as a bilinear product). What should $x$ and $W$ be? **$x=\begin{bmatrix}{\color{#58F}a} \\ {\color{orange}b}\end{bmatrix}, W=\begin{bmatrix}\alpha & \beta \\ \gamma & \delta\end{bmatrix}$**

---

# Calculus

Derivatives:

$$ \frac{df(x)}{dx} = \lim_{\Delta x \to 0} \frac{f(x + \Delta x) - f(x)}{\Delta x} $$

Partial derivatives:

$$ \frac{\partial f}{\partial x_i} $$

> [!question]- Question 9
> 1. What is the derivative of $f({\color{#58F}x})$?  $$\frac{{\color{#888}d} f({\color{#58F}x})}{{\color{#888}d}{\color{#58F}x}} = \frac{{\color{#888}d}3{\color{#58F}x}^{2}+5{\color{#58F}x}+1}{{\color{#888}d}{\color{#58F}x}} = \frac{{\color{#888}d}3{\color{#58F}x}^{2}}{{\color{#888}d}{\color{#58F}x}} + \frac{{\color{#888}d}5{\color{#58F}x}}{{\color{#888}d}{\color{#58F}x}} + \frac{{\color{#888}d}1}{{\color{#888}d}{\color{#58F}x}} = 3 \frac{{\color{#888}d}{\color{#58F}x}^{2}}{{\color{#888}d}{\color{#58F}x}}+ 5 \frac{{\color{#888}d}{\color{#58F}x}}{{\color{#888}d}{\color{#58F}x}}+ \frac{{\color{#888}d}1}{{\color{#888}d}{\color{#58F}x}} = 3 \cdot 2{\color{#58F}x} + 5 \cdot 1 + 0 = 6{\color{#58F}x} +5$$
> 2. For which ${\color{#58F}x}$ is $f({\color{#58F}x})$ at its minimum? $$\frac{{\color{#888}d}f({\color{#58F}x})}{{\color{#888}d}{\color{#58F}x}} = 0, 6{\color{#58F}x} + 5 = 0, {\color{#58F}x} = - \frac{5}{6}$$
> 3. Let $h({\color{#58F}x}) = g(f({\color{#58F}x}))$, with $f$ defined as above. Let $\frac{{\color{#888}d}g({\color{#58F}x})}{{\color{#888}d}{\color{#58F}x}} = sin({\color{#58F}x})/{\color{#58F}x}$. Without knowing what $g({\color{#58F}x})$ is (or working it out), can we find the derivative of $h({\color{#58F}x})$? **Yes, using the chain rule:** $$\frac{{\color{#888}d}p(q({\color{#58F}x}))}{{\color{#888}d}{\color{#58F}x}} = \frac{{\color{#888}d}p(q({\color{#58F}x}))}{{\color{#888}d}p({\color{#58F}x})} \frac{{\color{#888}d}q({\color{#58F}x})}{{\color{#888}d}{\color{#58F}x}}$$ Thus $$\frac{{\color{#888}d}h({\color{#58F}x})}{{\color{#888}d}{\color{#58F}x}} = \frac{{\color{#888}d}g(f({\color{#58F}x}))}{{\color{#888}d}{\color{#58F}x}} = \frac{{\color{#888}d}g(f({\color{#58F}x}))}{{\color{#888}d}f({\color{#58F}x})} \frac{{\color{#888}d}f({\color{#58F}x})}{{\color{#888}d}{\color{#58F}x}} = \frac{\sin f({\color{#58F}x})}{f({\color{#58F}x})} (6{\color{#58F}x}+5)=\frac{\sin(3{\color{#58F}x}^{2}+5{\color{#58F}x}+1)}{3{\color{#58F}x}^{2}+5{\color{#58F}x}+1}(6{\color{#58F}x}+5)$$

> [!question]- Question 10
> Let ${\color{#58F}x}\in \mathbb{R}^{2}$ and let $f({\color{#58F}x}) = 3{{\color{#58F}x}_1}^2 + 4{\color{#58F}x_{1}}^{}{\color{#58F}x_{2}}^{} - {{\color{#58F}x_2}}^2$
> 1. What is the *partial* derivative of $f({\color{#58F}x})$ with respect to ${\color{#58F}x_{1}}$? $$\frac{{\color{#888}\partial} \left(3{{\color{#58F}x_{1}}}^{2} + 4{\color{#58F}x_{1}}^{}{\color{#58F}x_{2}}^{} - {{\color{#58F}x_{2}}}^{2}\right)}{{\color{#888}\partial} {\color{#58F}x_{1}}} = \frac{{\color{#888}\partial} 3{{\color{#58F}x_{1}}}^{2}}{{\color{#888}\partial} {\color{#58F}x_{1}}} + \frac{{\color{#888}\partial} 4{\color{#58F}x_{1}}{\color{#58F}x_{2}}}{{\color{#888}\partial} {\color{#58F}x_{1}}} = \frac{{\color{#888}\partial} {{\color{#58F}x_{2}}}^{2}}{{\color{#888}\partial} {\color{#58F}x_{1}}} = 6{\color{#58F}x_{1}}+4{\color{#58F}x_{2}}$$
> 2. What is the partial derivative of $f({\color{#58F}x})$ with respect to ${\color{#58F}{\color{#58F}x_{2}}}$? $$\frac{{\color{#888}\partial} 3{{\color{#58F}x_{1}}}^{2}}{{\color{#888}\partial} {\color{#58F}x_{2}}} + \frac{{\color{#888}\partial} 4{\color{#58F}x_{1}}{\color{#58F}x_{2}}}{{\color{#888}\partial} {\color{#58F}x_{2}}} - \frac{{\color{#888}\partial} {{\color{#58F}x_{2}}}^{2}}{{\color{#888}\partial} {\color{#58F}x_{2}}}= 4 {\color{#58F}x_{1}}-2{\color{#58F}x_{2}}$$
> 3. What is the *gradient* of $f({\color{#58F}x})$? **The gradient is the vector of all partial derivatives: $\nabla f({\color{#58F}x})=(6{\color{#58F}x_{1}}+4{\color{#58F}x_{2}}, 4{\color{#58F}x_{1}}-2{\color{#58F}x_{2}})$. For practical reasons, the gradient is usually defined as a row vector (when the input to f is a column vector).**
> 4. The gradient is a function derived from $f$, just like the derivative is a function. What are the domain and range of the gradient of $f({\color{#58F}x})$? **The gradient has the same input as $f$, so the domain is also $\mathbb{R}^{2}$. The gradient defines a vector of length two for each input, so its range is $\mathbb{R}^{2}$. In other words $\nabla f: \mathbb{R}^{2} \to \mathbb{R}^{2}$**

---

# Additional Resources

- [Khan Academy: Sigma Notation](https://www.khanacademy.org/math/algebra2/sequences-and-series/alg2-sigma-notation/e/evaluating-basic-sigma-notation)
- [Khan Academy: Expected Value](https://www.khanacademy.org/math/probability/probability-geometry/expected-value-geo/a/expected-value-basic)
- [Better Explained: Linear Algebra Guide](https://betterexplained.com/articles/linear-algebra-guide/)
- [Multivariable Calculus - Gradient](https://betterexplained.com/articles/vector-calculus-understanding-the-gradient/)

