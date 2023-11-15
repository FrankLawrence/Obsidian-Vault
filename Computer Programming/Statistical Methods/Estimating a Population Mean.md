---
Tags: 
Created: 2023-11-15 23:45:21
---
(Links:: [[Statistical Methods]])

The sampling distribution of a sample mean (estimator) is approximately normally distributed. For a given sample, the **estimator** yields an **estimate** of the population mean $\mu$. If we only have one estimate, then that isn't as accurate. The accuracy of the estimator is given by the standard deviation of the sampling distribution. This is why we need the distributions (to describe how good an estimate will be).
We can use the approximate sampling distribution to construct **confidence intervals**, which help describe how accurate the estimates are. The **95% confidence interal for $\mu$** is the range of estimator values, where we are 95% confident that this interval actually contains $\mu$. We know that if we have 100 independent samples of the same size, and we calculate the confidence intervals, then 95 of them contain $\mu$.
We know from the [[Central Limit Theorem]], that the sample mean $\bar X_{n}$ is approximately a $N\left(\mu, \sigma^{2}/n\right)$-distribution. If $\sigma$ is unkown, we replace it with $s_{n}$ (the sample standard deviation). Since $\bar X_{n}$ is not standard normally distributed, we need to find the z-score: $$Z=\frac{\bar X_{n}-\mu}{s_{n} / \sqrt{n}}\sim N(0,1)$$
We want a symmetric region (since $\mu$ should be in the middle) so we look for the z-score in the standard normal distribution where the value is 0.975 -> 1.96. The distribution is symmetric, so we can use the z-score on either side: $$P(-1.96\leq Z \leq 1.96)=0.95$$
When inserting the formula for $Z$ and rewriting in terms of $\bar X_{n}$, we get $$P\left(\mu - 1.96 \frac{s_{n}}{\sqrt{n}}\leq \bar X_{n}\leq \mu + 1.96 \frac{s_{n}}{\sqrt{n}}\right)$$
For approximately 95 out of 100 independent samples of size $n$ $$\mu - 1.96 \frac{s_{n}}{\sqrt{n}}\leq \bar x_{n}\leq \mu + 1.96 \frac{s_{n}}{\sqrt{n}}$$
This is equivalent to $$\bar x_{n} - 1.96 \frac{s_{n}}{\sqrt{n}}\leq \mu \leq \bar x_{n} + 1.96 \frac{s_{n}}{\sqrt{n}}$$

> [!definition] 95% confidence interval for $\mu$ 
> $E=1.96\frac{s_{n}}{\sqrt{n}}$ is called the margin of error, and the interval $$\left[\bar x_{n} - 1.96 \frac{s_{n}}{\sqrt{n}}, \bar x_{n} + 1.96 \frac{s_{n}}{\sqrt{n}}\right]$$ is called a 95% confidence interval for $\mu$. (If $\sigma$ is known, use it instead of $s_{n}$)

$z_{\alpha/2}=1.96$ for $\alpha=0.05=1-0.95$
$z_{\alpha/2}=$ z score separating an area of $\alpha/2$ in the right tail of $N(0,1)$: $$P(Z\geq z_{\alpha/2})=\alpha/2 \quad \text{and}\quad P(Z\leq -z_{\alpha/2})=\alpha/2$$ so by properties of probability $$P(-z_{\alpha/2}\leq Z\leq z_{\alpha/2})=1-a$$
If we want the margin of error $E$ to be smaller than a certain $E_\text{max}$, then we can choose a desired value for $n$: $$E=1.96\frac{\sigma}{\sqrt{n}}\leq E_\text{max}\quad\Leftrightarrow\quad n\geq\left(\frac{1.96\times\sigma}{E_\text{max}}\right)^{2}$$

---
References: [[Statistical Methods Lecture5.pdf]]