---
Tags: Math
Created: 2023-12-05 19:54:09
---
(Links:: [[Statistical Methods]])

> [!example] Mean exam grade (right-sided test)
> $H_0:\mu=6$ vs. $H_{1}:\alpha>6$, $\alpha=0.05$
> $n=13$, $\bar x_{13}=7.03$, and assume that $\sigma=1.93$
> Observed value of test statistic: $$z=\frac{7.03-6.00}{1.93/\sqrt{13}}\approx1.93$$
> $P$-value is $P(Z\geq 1.93)=1-0.9732=0.0268$. 
> Or, $z$ is in the critical region: $z=1.93>1.65=z_{\alpha/2}$
> Conclusion: reject $H_0$

If **$\sigma^2$ is unknown**, it is estimated by the sample standard variance $S_n^2$. Then, approximately, under $H_0$, $$T=\frac{\bar X_{n}-\mu}{S_{n} / \sqrt{n}}\sim t_{n-1}$$
This is the **t-distribution with $n-1$ degrees of freedom**. If we want to then find the critical values for a t-distribution, we need to find the upper t-quantile, where $P(T\geq t_{n-1,\alpha})=\alpha$

> [!info] Critical regions for tests with sample size $n$ and significance level $\alpha$ 
> - Left-sided test: critical region to the left of $-t_{n-1,\alpha}$
>   -> $(-\infty,-t_{n-1,\alpha})$
> - Right-sided test: critical region to the right of $t_{n-1,\alpha}$
>   -> $(t_{n-1,\alpha},+\infty)$
> - Two-sided test: critical region is $(-\infty,-t_{n-1,\alpha/2}) \cup (t_{n-1,\alpha/2},+\infty)$

In addition to this, we can create [[Estimating a Population Mean|Confidence Intervals]]. If we **know $\sigma$** we replace $s_n$ with it for the confidence interval. However, when we **do not know $\sigma$**, instead of using $z$-scores we use a *t-distribution*: $$\left[\bar x_{n} - t_{n-1,\alpha/2} \frac{s_{n}}{\sqrt{n}}, \bar x_{n} + t_{n-1,\alpha/2} \frac{s_{n}}{\sqrt{n}}\right]$$

---
References: