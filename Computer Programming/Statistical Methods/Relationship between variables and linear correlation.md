---
Tags: 
Created: 2023-12-06 03:26:39
---
(Links:: [[Statistical Methods]])

When looking at two variables $A$ and $B$, we sometimes ask ourselves: How do they relate to each other? Does a high $A$ cause a high $B$? 
To answer these questions, we must measure the variables of $n$ subjects. First, we investigate the relationship with the help of a **scatterplot**.

- **Correlation** is some association between variables 
- **Positive corr.**: higher $x$ values are associated with higher $y$ values
- **Negative corr.**: higher $x$ values are associated with lower $y$ values
- If **correlated** and plotted variables follow some straight line, one speaks of a linear relationship (**linear correlation**) between them.

![[Correlation.png|600]]

The **theoretical linear correlation coefficient** is denoted by $\rho$. The **sample linear correlation coefficient** is denoted by $$r=\frac{1}{n-1}\frac{\sum_{i=1}^{n}(x_{i}-\bar x)(y_{i}-\bar y)}{s_xs_y}$$ where $s_x$ and $s_{y}$ are sample standard deviations of $x_{1},...,x_{n}$ and $y_{1},...,y_{n}$, and $r$ is an estimate of the theoretical correlation coefficient $\rho$.

> [!example] Sample correlation coefficient examples
> ![[Sample correlation coefficient.png|600]]

> [!warning] Sample correlation coefficient $r$ is very sensitive to outliers

> [!danger] Causality (correlation does not imply causality)
> - 3rd-variable problem: unmeasured variable that effects both variables. E.g., ice cream sales and murder rate correlated. 3rd variable: sunshine.
> - Direction of causality: $r$ does not tell which variable causes the other to change. E.g., wind mill rotation and wind speed correlated; but wind mills do not cause wind.

$R$ is the random variable that corresponds to $r$. For a test $H_0:p=0$ vs. $H_{1}:p\neq 0$ the test statistic is $$T_{p}=\frac{R}{\sqrt{\frac{1-R^{2}}{n-2}}}\sim t_{n-2}$$ a $t$-distribution with $n-2$ degrees of freedom. ^2fdaf3

---
References: