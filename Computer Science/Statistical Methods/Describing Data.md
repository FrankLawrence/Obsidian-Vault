---
Tags: 
Created: 2023-11-08 11:43:44
---
(Links:: [[Statistical Methods]])
There are two ways to describe data: **quantitatively** (numerical summaries of location and variation) and **qualitatively** (shape, location, spread of data distribution).

Qualitative description is for example **shape**: smooth approximation of histogram, relating the data distribution to familiar distributions.

![[Describing data example.png|500]]

The *location* is the position on the $x$ axis. The *spread* is the measure of variation within a dataset.

![[Qualitative description location.png]]
![[Qualitative description spread.png]]

# Numerical summaries
We can describe the distribution of data with values for: location, spread, skewness,...
# Measures of center
We can measure the center of a dataset using either the [[Mean]] or **Median** ("middle" value after sorting) value. A *sample mean* is denoted by $\bar x=(\sum_{i=1}^{n}x_{i})/n$. The *population mean* is denoted by $\mu=(\sum_{i=1}^{N}x_{i})/N$. We can also use **mode** which gets the value with the highest frequency.

> [!example] 
> $grades=(10,7,6,10,8,5,8,7,5,9,7)$
> $sorted=(5,5,6,7,7,7,8,8,9,10,10)$
> 
> $mean\approx 7.5$
> $median=7$
> $mode=7$

# Measure of variation
If we take two datasets and plot them, they could have the same [[Mean]] and median values and it could be hard to distinguish them apart.
```r
bank1 = (4.1, 5.2, 5.6, 6.2, 6.7, 7.2, 7.7, 7.7, 8.5, 9.3, 11.0)
bank2 = (6.6, 6.7, 6.7, 6.9, 7.1, 7.2, 7.3, 7.4, 7.7, 7.8, 7.8)
```

![[Histogram bank1.png|400]]
![[Histogram bank2.png|400]]

![[Bank1 and bank2 histogram.png|1000]]

The **sample standard deviation** $s$ and the **sample variance** $s^2$ ("mean quadratic deviation from $\bar x$") are common measures of variation (or deviation from $\bar x$): $$s=\sqrt{\frac{\sum_{i=1}^{n}(x_{i}-\bar x)^{2}}{n-1}}, \qquad s^2=\frac{\sum_{i=1}^{n}(x_{i}-\bar x)^{2}}{n-1}$$
In R these are computed by the command `sd()` and `var()` respectively.
Do not confuse $s$ and $s^{2}$ with **population standard deviation** $\sigma$ and the **population variance** $\sigma^{2}$.
# Measures of relative standing and box plots
Alternative measures of location *and* spread are **percentiles**. Percentile $P_{i}: i\% \text{ of data values } < P_{i}$ *and* $(100-i)\% \text{ of values }\geq P_{i}$.
Special percentiles: **quartiles** $Q_{1}, Q_{2} \text{ and } Q_{3}$. Divide data set into four groups of $\approx 25\%$ of data values each.

___
References: [[Statistical Methods Lecture1.pdf]]