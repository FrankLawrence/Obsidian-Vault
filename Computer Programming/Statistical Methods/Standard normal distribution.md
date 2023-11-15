---
Tags: 
Created: 2023-11-10 00:59:43
---
(Links:: [[Statistical Methods]])
# Continuous Random Variables
Recall the [[Probability Distributions#^4bef25|definition of a continuous random variable]]. Let $X$ be a continuous random variable. Consider $P(\text{values of }X \text{ lie in }I)$ ($I$ is some interval). The area under the probability density function is restricted to $I$.

> [!example]- Choose point in interval
> $X$ is a random point between $-2$ and $1$
> ![[Choose point in interval.png|500]]
> The probability of any point $x$ in this interval is $p(x)=\frac{1}{3}$
> What is the probability of $X\in[-1,\frac{1}{2}]$? $$P\left(X\in\left[-1,\frac{1}{2}\right]\right)=\left(\frac{1}{2}-(-1)\right)\times \frac{1}{3}=\frac{3}{2}\times \frac{1}{3}=\frac{1}{2}$$
# Probability density function
A probability density function $p(x)$ is a function such that
- $p(x)\geq 0$ for all $x$, and
- the total area under the curve is $1$

> [!example]- Bell-shaped density (with probability between $-1$ and $1$)
> ![[Bell-shaped density.png]]

# The standard normal distribution
A random variable $X$ has a **normal distribution** with parameters $\mu\in \Bbb{R}$ and $\sigma>0$ if its density is $$p(x)=\frac{1}{\sigma\sqrt{2\pi}}e^{-\frac{1}{2}\left(\frac{x-\mu}{\sigma}\right)^2}$$

%%$$p(x)'=\frac{\sqrt{2}(\mu-x)e^{-\frac{(\mu-x)^2}{2\sigma^2}}}{2\sqrt{\pi}\sigma^{3}}$$%%

Notice that $p(x)$ is continuous, bell-shaped and symmetric, $E(X)=\mu$ and $Var(X)=\sigma^{2}$. Normal distributions have the common notation $N(\mu, \sigma^{2})$ (with the mean $\mu$, and variance $\sigma^{2}$). The *standard normal distribution* is the case $N(0,1)$ (visible in the previous example).
**Determine probabilities of normal distribution**
$$\begin{align}
P(X\leq z)&=\text{area under density to the left of }z\\
P(X\in[a,b])&=P(X\leq b)-P(X\leq a)\\
P(X\geq b)&=1-P(X\leq b)
\end{align}$$

---
References: [[Statistical Methods Lecture4.pdf]]