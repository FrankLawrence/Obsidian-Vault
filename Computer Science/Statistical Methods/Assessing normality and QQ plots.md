---
Tags: Math
Created: 2023-11-10 18:32:03
---
(Links:: [[Statistical Methods]])
# QQ Plot
Consider a dataset $x_{1}, ..., x_{n}$.
- Ordered values $x_{(1)},...,x_{(n)}$ plotted vs. theoretical quantiles $z_{a_{1}},...,x_{a_{n}}$ of $N(0,1)$.
  Here, $z_{a_{i}}$ is the z-score with $\frac{2i-1}{2n}$ ($\approx \frac{i}{n}$) of the $N(0,1)$ area to the left.
- If points follow approximately a straight line, then the distribution could be modelled with $N(\mu, \sigma^{2})$
- If the plot shows a line $y=a+bx$, then $\mu\approx a$ and $\sigma\approx b$

The larger the sample size, the more reliable results you get back.

**Definition of a location-scale family of probability distribution**:
Each member is obtained from another by
- shifting (change in location) and/or
- stretching/squeezing (change in scale).

> [!important] 
> Random variable $X$ and $Y$ have probability distributions that are in the same location-scale family *iff* the QQ-plot shows a straight line $Y=a+bX$

## Types of QQ-plots
1. $x$-axis: theoretical quantiles of probability distribution.
   $y$-axis: sample quantiles of a dataset
   Used to assess whether the particular distribution could be used as model distribution.
2. $x$-axis: theoretical quantiles of a probability distribution
   $y$-axis: theoretical quantiles of another probability distribution
   Used to compare the shape of two probability distributions, for instance to verify whether they belong to the same location-scale family.
3. $x$-axis: sample quantiles of a dataset
   $y$-axis: sample quantiles of another dataset
   Used to compare the shape of the two data distributions and assess whether they could possibly originate from two model distributions belonging to the same location-scale family.

> [!example]+
> ![[SCR-20231110-rjrt.png|500]]
> ![[SCR-20231110-rjte.png|500]]

## How to interpret QQ plots
Draw (an imaginary) straight line through the middle of QQ plot.
- Are there points on the left side below the straight line?
  => Left tail of sample is heavier than left tail of $N(0,1)$
- Are there points on the left side above the straight line?
  => Left tail of $N(0,1)$ is heavier than left tail of sample
- Are there points on the right side below the straight line?
  => Right tail of sample is heavier than right tail of $N(0,1)$
- Are there points on the right side above the straight line?
  => Right tail of $N(0,1)$ is heavier than right tail of sample
  
> [!summary]- How to assess normality of data with QQ plot
> - Make normal QQ plot(`qqnorm()`)
> - If points follow approximately straight line $y=a+bx$ (with slope $b>0$), then $N(a,b^{2})$ is reasonable as model distribution.
> - If points don't follow straight line: sample most likely not from normal distribution.
> 
> In latter case: sample most likely from location-scale family with *lighter or heavier tails* than those of normal distribution, depending on shape of QQ plot.

---
References: [[Statistical Methods Lecture4.pdf]]