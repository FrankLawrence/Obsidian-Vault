---
Tags: 
Created: 2023-11-15 23:45:47
---
(Links:: [[Statistical Methods]])

Just like with the [[Estimating a Population Mean]], with the [[Central Limit Theorem]], we know that the sample proportion $\hat P_{n}$ is a $N\left(p,\frac{p(1-p)}{n}\right)$-distribution. We don't know the true value for the population proportion $p$, so we estimate it with the sample proportion: $$\hat P_{n}\sim N\left(p,\frac{\hat p_{n}(1-\hat p_{n})}{n}\right)$$

> [!definition] 95% confidence interval for $p$
> $E=1.96\sqrt{\frac{\hat p_n(1-\hat p_n)}{n}}$ is called the margin of error, and the interval $$\left[\hat p_{n} - 1.96 \sqrt{\frac{\hat p_n(1-\hat p_n)}{n}}, \hat p_{n} + 1.96 \sqrt{\frac{\hat p_n(1-\hat p_n)}{n}}\right]$$ is called a 95% confidence interval for $p$.

We can once again choose $n$ so that $E$ is as small as we want it to be. The population proportion is always between 0 and 1, so $p(1-p)\leq 0.25$. Then $$E=1.96\sqrt{\frac{\hat p_n(1-\hat p_n)}{n}}\leq 1.96 \sqrt{\frac{1}{4n}}\leq E_\text{max}\quad\Leftrightarrow\quad n\geq\left(\frac{1.96}{2\times E_\text{max}}\right)^{2}$$

---
References: [[Statistical Methods Lecture5.pdf]]