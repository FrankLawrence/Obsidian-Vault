---
Tags: 
Created: 2023-12-05 22:34:59
---
(Links:: [[Statistical Methods]])

There are two different sample types: **Dependet** and **independet** samples:
- Two samples are **dependet** if values in one sample are related to values in the other sample, or form natural pairs.
- Two samples are independet if there is no relationship between two samples. In this case sample size can be different.

> [!example]- What sample types are these?
> > [!question]- Diet helps you loose 10kg in 2 months: Choose 19 adults and weigh them now and in two months
> > **Dependent** 
> 
> > [!question]- Two M&M tpyes have the same weight: Take 8 random yellow M&Ms and 8 random brown M&Ms
> > **Independent**
> 
> > [!question]- Husbands taller than wives: Select 20 couples and measure husbands and wives
> > **Dependent**

> [!info]- Notation for two samples
> - Population 1
> 	- sample size $n_1$
> 	- population (theoretical) mean $\mu_1$
> 	- population (theoretical) standard deviation $\sigma_1$
> 	- population proportion $p_1$
> 	- sample mean $\bar x_1$
> 	- sample standard deviation $s_1$
> 	- sample proportion $\hat p_1$
> - Population 2
> 	- sample size $n_2$
> 	- population (theoretical) mean $\mu_2$
> 	- population (theoretical) standard deviation $\sigma_2$
> 	- population proportion $p_2$
> 	- sample mean $\bar x_2$
> 	- sample standard deviation $s_2$
> 	- sample proportion $\hat p_2$
> 
> If the samples are couples/matched, then $n_1=n_2$

# Two dependent samples

In the case where the two samples are dependet we need:
- that they are paired in a natural way
- that the number of pairs is large ([[Central Limit Theorem]] is applicable), or the differences between paired observations come from an approx. normal distribution.

When we test claims about 2 means, we actually focus on their **difference**: $H_0:\mu_{1}- \mu_{2}=d_{0}$ where $d_0$ is the claimed value under $H_0$. Depending on claim, the *alternative hypothesis* is $$H_{a}: \begin{cases} \mu_{1}-\mu_{2} < d_{0} & \text{ left-tailed test} \\
\mu_{1}-\mu_{2} > d_{0} & \text{ right-tailed test}\quad  \\
\mu_{1}-\mu_{2} \neq d_{0} & \text{ two-tailed test}
\end{cases}$$ 
$$\text{if } d_{0}=0, \text{ then }\; H_{a}:\begin{cases}\mu_1 < \mu_2 \\ \mu_{1} > \mu_{2} \\ \mu_{1} \neq \mu_{2}\end{cases}$$

Take an exmple of paired samples: $(X_{1},Y_{1}),(X_{2},Y_{2}),...,(X_{n},Y_{n})$. We calculate the differences $$D_1=X_{1}-Y_{1}, D_{2}=X_{2}-Y_{2},...,D_{n}=X_{n}-Y_{n}$$
and get the sample mean of differences $\bar D$ and the sample standard deviation of differences $S_d$. The test statistic is then $$T_{d}=\frac{\bar D-d_{0}}{S_{d}/\sqrt{n}}$$ which under $H_0$ has a t-distribution with $n-1$ degrees of freedom.

> [!info]+ Confidence interval for $\mu_{1}-\mu_{2}$
> $E=t_{n-1,\alpha/2} \frac{s_d}{\sqrt{n}}$ is the margin of error, and the interval $$\left[\bar d - t_{n-1,\alpha/2} \frac{s_{d}}{\sqrt{n}}, \bar d + t_{n-1,\alpha/2} \frac{s_{d}}{\sqrt{n}}\right]$$ is a $(1-\alpha)$-confidence interval for $\mu_{1}-\mu_{2}$

# Two means: independent samples
We will add another notation: For population 1, $x_1$ depicts the number of successes in that sample.
## $\sigma_{1}$ and $\sigma_{2}$ are unknown
### $\sigma_{1}=\sigma_{2}$
In this case we use the following test statistic: $$T_{eq}=\frac{(\bar X_{1}-\bar X_{2})-d_{0}}{\sqrt{\frac{S_{p}^{2}}{n_{1}} +\frac{S_{p}^{2}}{n_{2}}}}\sim t_{n_{1}+n_{2}-2}, \quad \text{under }H_{0}$$ 
where $$S_{p}^{2}=\frac{(n_{1}-1)S_{1}^{2}+(n_{2}-1)S_{2}^{2}}{n_{1}+n_{2}-2}$$ is the **pooled sample variance**
> [!info]+ $(1-\alpha)$-confidence interval for $\mu_{1}-\mu_{2}$ 
> Then the $(1-\alpha)$-confidence interval for $\mu_{1}-\mu_{2}$ is $$\left[(\bar x_{1}-\bar x_{2})-E,(\bar x_{1}-\bar x_{2})+E\right]$$ where $$E=t_{n_{1}+n_{2}-2,\alpha/2}\sqrt{\frac{s_{p}^{2}}{n_{1}}+\frac{s_{p}^{2}}{n_{2}}}$$ is the margin.
### $\sigma_{1}\neq\sigma_{2}$
In this case we use the following test statistic: $$T_{eq}=\frac{(\bar X_{1}-\bar X_{2})-d_{0}}{\sqrt{\frac{S_{1}^{2}}{n_{1}} +\frac{S_{2}^{2}}{n_{2}}}}\sim t_{ñ}, \quad \text{under }H_{0}$$ 
where $$ñ=\frac{(s_{1}^{2}/n_{1}+s_{2}^{2}/n_{2})^2}{(s_{1}^{2}/n_{1})^{2}/(n_{1}-1)+(s_{2}^{2}/n_{2})^{2}/(n_{2}-1)}$$
We approximate it with $ñ=\text{min}\{n_{1}-1, n_{2}-1\}$
> [!info]+ $(1-\alpha)$-confidence interval for $\mu_{1}-\mu_{2}$
> Then the $(1-\alpha)$-confidence interval for $\mu_{1}-\mu_{2}$ is $$\left[(\bar x_{1}-\bar x_{2})-E,(\bar x_{1}-\bar x_{2})+E\right]$$ where $$E=t_{ñ,\alpha/2}\sqrt{\frac{s_{1}^{2}}{n_{1}}+\frac{s_{2}^{2}}{n_{2}}}$$ is the margin.

# Two proportions: independent samples

With proportions, the **"claimed value"** of the difference is always 0, so $H_{0}:p_{1}=p_{2}$. For this test, 3 requirements must be met:
- both samples are independent
- $n_{1}> 30$ and $n_{2}>30$ or the data is normally distributed
- for each sample, there are at least 5 success and failures

$\hat P_{1}$ and $\hat P_{2}$ are the sample proportions and $\bar P$ is the **pooled sample proportion**: $$\hat P_{1}=\frac{x_{1}}{n_{1}},\qquad \hat P_{2}=\frac{x_{2}}{n_{2}},\qquad \bar P=\frac{x_{1}+x_{2}}{n_{1}+n_{2}}$$
The test statistic is then $$Z_{p}=\frac{(\hat P_{1}-\hat P_{2})}{\sqrt{\frac{\bar P(1-\bar P)}{n_{1}}+\frac{\bar P(1-\bar P)}{n_{2}}}}\sim N(0,1)\quad \text{under }H_{0}\text{ (approx.)}$$

> [!info]+ $(1-\alpha)$-confidence interval for $p_{1}-p_{2}$
> The confidence interval is $$\left[(\hat p_{1}-\hat p_{2})-E, (\hat p_{1}-\hat p_{2})+E\right]$$ where the margin is $$E=z_{\alpha/2}\sqrt{\frac{\hat p_{1}(1-\hat p_{1})}{n_{1}}+\frac{\hat p_{2}(1-\hat p_{2})}{n_{2}}}$$

---
References: