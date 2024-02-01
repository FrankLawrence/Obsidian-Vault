---
Tags: 
Created: 2023-12-06 15:13:19
---
(Links:: [[Statistical Methods]])

Suppose we observe counts of data per category, and note the frequency distribution for those categories (in a table). We want to investigate whether the data comes from some claimed distribution of categories.

> [!important] General approach
> - $k$ categories/cells for a sample of size $n$
> - $O_i$ is the *observed count of category $i$*
> - $p_i$ is the *claimed probability of category $i$*
> - Test $H_0$: **Frequency counts agree with claimed distribution** vs. $H_1$: **Frequency counts don't agree with claimed distribution**
> - *Expected count* $E_i$ of category $i$ under $H_0$ is $$E_{i}=n\cdot p_i$$ with $E_{i}\geq 5$ for all $i$
> - Idea of test: test statistic is the sum of squared and scaled differences between expected and observed counts: $$x^{2}=\sum\limits_{\text{all categories}}\frac{(\text{Observed frequency}\;-\;\text{Expected frequency})^{2}}{\text{Expected fequency}}=\sum\limits_{i=1}^{k}\frac{(O_{i}-E_{i})^{2}}{E_{i}}$$
> - $X^{2}$ has approximately has a $\chi^2_{k-1}$-distribution
> 	- A chi-square distribution only has positive values and is asymmetric: ![[Chi-square distribution.png|500]]
> - $H_{0}$ is rejected for **large values** of the observed score $x^2$:
> 	- Critical value method: reject $H_0$ if $x^2>\chi_{k-1,\alpha}^{2}$
> 	- $P$-value method: reject $H_0$ if $P(x^{2}\geq\chi^{2})< \alpha$

> [!example]
> # Claimed distribution
> | Color  | Frequency |
> | ------ | --------- |
> | Red    | 13%       |
> | Orange | 20%       |
> | Yellow | 14%       |
> | Brown  | 13%       |
> | Blue   | 24%       |
> | Green  | 16%       |
> 
> # Frequency distribution
> | Color  | Observed Frequency | Expected Frequency |
> | ------ | ------------------ | ------------------ |
> | Red    | 20                 | 19.5               |
> | Orange | 37                 | 30                 |
> | Yellow | 12                 | 21                 |
> | Brown  | 12                 | 19.5               |
> | Blue   | 41                 | 36                 |
> | Green  | 28                 | 24                 |
> 
> # Test
> - $H_{0}:p_{1}=0.13, p_{2}=0.20, p_{3}=0.14, p_{4}=0.13, p_{5}=0.24, p_{6}=0.16$
> - $H_{1}:p_{i}\neq e_{i}$ for at least one $i$, with $\alpha=0.05$
> - Check requirement $e_i\geq5$ for all $i$
> - The test statistic is $\chi_{n-1}^2=\chi_{5}^{2}$-distributed
> - $$x^{2}=\sum\limits_{i=1}^{6}\frac{(o_{i}-e_{i})^{2}}{e_{i}}=\frac{(20-19.5)^{2}}{19.5}+\cdots +\frac{(28-24)^{2}}{24}\approx 9.75$$
> - The test is always right-sided -> $x^{2}=9.75 \not >11.07=\chi_{5,0.05}^{2}$
> - $H_{0}$ is not rejected. There is not sufficient evidence to reject the claim about the color distribution.

---
References: