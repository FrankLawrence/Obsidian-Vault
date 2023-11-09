---
Tags: 
Created: 2023-11-10 00:41:30
---
(Links:: [[Statistical Methods]])
# Expected value
Expected value of a discrete random variable $X$ with possible values $x_{1},...,x_{k}$, weighted average of all possible values of $X$: $$\mu =E(X)=\sum_{i=1}^{k}x_{i}\times P(X=x_{i})$$

> [!example]- $X = \text{Maximum of two dice throughs}$
> What is $E(X)$ (On average, what maximum do we expect)?
> 
> | $x$ | $P(X=x)$        | num. $P(X=x)$ | $x\times P(X=x)$ |
> | --- | --------------- | ------------- | ---------------- |
> | $1$ | $\frac{1}{36}$  | $0.028$       | $0.028$          |
> | $2$ | $\frac{1}{12}$  | $0.083$       | $0.167$          |
> | $3$ | $\frac{5}{36}$  | $0.139$       | $0.417$          |
> | $4$ | $\frac{7}{36}$  | $0.194$       | $0.778$          |
> | $5$ | $\frac{1}{4}$   | $0.250$       | $1.250$          |
> | $6$ | $\frac{11}{36}$ | $0.306$       | $1.833$          |
>
> $$E(X)=\sum\limits_{i=1}^{6}i\times P(X=i)\approx 4.472=\mu$$

# Variance
The **variance** of a discrete random variable $X$ with values $x_{1},..., x_{k}$: $$\begin{align}\sigma^{2}=Var(X)&=E(X-EX)^{2}=\sum\limits_{i=1}^{k}\left[(x_{i}-\mu)^{2}P(X=x_{i})\right]\\&=EX^{2}-(EX)^{2}=\sum\limits_{i=1}^{k}\left[x_{i}^{2}P(X=x_{i})\right]-\mu^{2}\end{align}$$
The **standard deviation** of $X$ is $$\sigma = SD(X) = \sqrt{Var(X)}$$

> [!example]- $X = \text{Maximum of two dice throughs}$
> What is $Var(X)$ and $SD(X)$?
> 
> | $x$ | $P(X=x)$        | num. $P(X=x)$ | $x\times P(X=x)$ | $x^{2}\times P(X=x)$ |
> | --- | --------------- | ------------- | ---------------- | ---- |
> | $1$ | $\frac{1}{36}$  | $0.028$       | $0.028$          | $0.028$ |
> | $2$ | $\frac{1}{12}$  | $0.083$       | $0.167$          | $0.333$ | 
> | $3$ | $\frac{5}{36}$  | $0.139$       | $0.417$          | $1.250$ |
> | $4$ | $\frac{7}{36}$  | $0.194$       | $0.778$          | $3.110$ |
> | $5$ | $\frac{1}{4}$   | $0.250$       | $1.250$          | $6.250$ |
> | $6$ | $\frac{11}{36}$ | $0.306$       | $1.833$          | $11.000$ |
>
> $$\sum\limits_{i=1}^{6}i^{2}\times P(X=i)\approx 21.972$$
> $$Var(X)=\sigma^{2}=\sum\limits_{i=1}^{6}i^{2}\times P(X=i)-\mu^{2}\approx21.972-20.000=1.972$$ 
> $$SD(X)=\sqrt{Var(X)}\approx\sqrt{1.972}\approx1.404$$

# Law of Large Numbers
Let $X_{1},...,X_{n}$ be $n$ independent versions of the random variable $X$; let $\mu=E(X)$. Their mean $\frac{1}{n}(X_{1}+\cdots+X_{n})$ tends to approach $\mu$.

___
References: [[Statistical Methods Lecture3.pdf]]