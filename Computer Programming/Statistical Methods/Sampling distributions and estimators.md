---
Tags: 
Created: 2023-11-10 20:10:09
---
(Links:: [[Statistical Methods]])

When we take a sample proportion $\hat p_{n}$ of a population proportion $p$, it sometimes doesn't represent the entire population properly. 
We cannot say whether $\bar x_{n}$ (sample mean) is close to $\mu$, or whether $\hat p_{n}$ is close to $p$, but we can study the distribution of all possible values of $\bar X_{n}$ or $\hat P_{n}$ for fixed sample size $n$.

> [!definition] Sampling distribution of the sample mean
> Let the random variable $\bar X_{n}$ denote the sample mean of a sample of size $n$. The sampling distribution of the sample mean consists of all possible values of $\bar X_{n}$, based on all possible samples of size $n$, and corresponding probabilities.

> [!caution]- Sampling distribution (of random variable) != sample distribution (of dataset)
> With a sampling distribution we look at a random variable (eg. the sample mean) and plot it's distribution.
> A sample distribution is the distribution of a single sample of data.

> [!example] Approximating sampling distribution
> We have 2692 files of different file sizes. We take a sample of size $n$ and calculate the sample mean. We do this 10000 times. Below are the distributions for sample sizes (from left to right) $n=5$, $n=100$ and $n=500$.
> ![[approximating sampling distribution example.png|500]]

> [!definition] Sampling distribution of the sample proportion
> This is the probability of the random variable $\hat P_{n}$: it consists of all possible values of $\hat p_{n}$ based on all possible samples of size $n$ and corresponding probabilities.

In the special case where we only have two possible outcomes for $X_i$ (1 and 0), we say that $P(X=1)=p$ and $P(X=0)=1-p$, where $p=\text{population proprotion}$. If $n$ people are surveyed, we get $x_{1},x_{2},...,x_{n}$: 
$$x_{i} =
\begin{cases}
1, & \text{if subject $i$ said 'yes' / has the property} \\
0, & \text{if subject $i$ said 'no' / does not have the property}
\end{cases}$$
Then $\hat p_{n}=(x_{1}+x_{2}+\cdots+x_{n})/n$.
To find a distribution for the sample proportion, we need the population mean and population standard deviation. $$\mu=1\times p+0\times (1-p)=p$$ $$\sigma =\sqrt{(1-p)^{2}\times p + (0-p)^{2}\times (1-p)} = \sqrt{p(1-p)}$$
For $n>30$ the sample proportion $\hat P_{n}$ with population proportion $p$ is approximately normal: $$\hat P_{n}\sim N\left(p,\frac{p(1-p)}{n}\right)$$

---
References: [[Statistical Methods Lecture5.pdf]]