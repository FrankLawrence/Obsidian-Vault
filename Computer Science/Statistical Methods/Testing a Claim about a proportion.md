---
Tags: 
Created: 2023-12-05 15:31:59
---
(Links:: [[Statistical Methods]])

We create a test, we use the template: [[Basics of Hypothesis Testing#^8af4c1|Five steps of Hypothesis Testing]]
> [!example]+ Heads and Tails
> 1. Population parameter: population proportion $p$
> 2. Collect data: 37 x H, 63 x T
> 3. Choose test statistic and identify its distribution under $H_0$: Use that the sampling distribution of the sample proportion approximately has a $N\left(p, \frac{p(1-p)}{n}\right)$ distribution. A good test statistic is $$Z=\frac{\hat P_{n}-p}{\sqrt{\frac{p(1-p)}{n}}}$$ where $p=0.5$ is the claimed value from $H_0$.
>    Under $H_{0}, Z\sim N(0,1)$ approximately (if $n$ is large enough)
> 4. Calculate the value of the test statistic based on data:
>    It follows from the data that $\hat p_n=0.37$, so the observed value of the test statistic is $$z=\frac{0.37-0.5}{\sqrt{\frac{0.5\cdot 0.5}{100}}}=-2.6$$
> 5. Find $P$-value:
>    We observed $z=-2.6$. What is the probability of "test statistic takes even more extreme values"?
>    Out test is left-tailed: $H_1:p<0.5$
>    That means we have to compute $$P(Z\leq -2.6)=0.0047$$
> 6. Conclude: The $P$-value is smaller than the significance level $\alpha=0.05$, so $H_0$ is rejected. We have enough evidence to confirm the coin is biased towards tails.

---
References: