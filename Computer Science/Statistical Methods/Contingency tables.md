---
Tags: Math
Created: 2023-12-06 16:05:11
---
(Links:: [[Statistical Methods]])

Contingency tables are tables with frequency counts of categorical data corresponding to both variables. Each row has **$r$ categories** (width) and each column has **$c$ categories** (height). In total there are $r\times c$ cells, where $O_{ij}$ is the number of subjects in $i$-th row and $j$-th column.

# Testing for independence and homogeneity
- **Test for independence**: want to test whether the two variables (row and column variables) are *independent* (or not): $$H_{0}:\text{row variable and column variable are independent}$$
- Testing independence of two variables is typically within one big population.
- Example **Lefties**: Think of the data as a sample from one common population. **Test independence** here: are gender and left-handedness dependent?
- **Test for homogeneity**: test whether the distributions of columns (rows) are *homogeneous* over rows (or columns): $$H_{0}:\text{the distributions of columns over row categories are equal}$$
- Now think of data as coming from different populations (with respect to categories of one variable) and we want to investigate whether the distribution over the categories (of another variable) in these different populations is different or not.
- Example **Lefties**: Think of *men* and *women* as two different populations. **Test homogeneity**: men and women have the same proportion of left-handedness?
- Although the motivation, sampling and hypothesis are different for testing independence and homogeneity, the tests for both are carried out **exactly in the same way**. So basically, there is **no difference between these tests**.

Let $n$ be the total number of observations. Under the null hypothesis of independence (or homogeneity), the counts are expected to be in proportion: $$E_{ij}= n\frac{o_{i.}}{n}\frac{o_{.j}}{n}=\frac{(i-\text{th row total})\cdot (j-\text{th columnn total})}{\text{grand total}}$$
There are however some requirements for the test:
- $2\times 2$ table: all $E_{ij}\geq 5$
- larger tables: all $E_{ij}\geq 1$ and 80% of $E_{ij}\geq 5$

If these requirements are met, then the test statistic is $$X^{2}=\sum\limits_{\text{all cells}}\frac{(O-E)^{2}}{E}=\sum\limits_{(i,j)}\frac{(O_{ij}-E_{ij})^{2}}{E_{ij}}\sim \chi^{2}_{(r-1)(c-1)}$$

> [!example] Lefties
> Test claim $H_0$: "gender and left-handedness are independent", $\alpha=0.05$
> 
> |       | Left-hand | Right-hand | total |
> | ----- | --------- | ---------- | ----- |
> | men   | 23        | 217        | 240   |
> | women | 65        | 455        | 520   |
> | total | 88        | 672        | 760   |
> 
> We expect $760 \cdot P(\text{men and left-handed})$ in cell $(1,1)$. We have $P(\text{men})=\frac{240}{750}$ and $P(\text{left-handed})=\frac{88}{760}$. Under $H_0$, gender and left-handedness are independent, thus $P(\text{men and left-handed})=P(\text{men})\cdot P(\text{left-handed})=\frac{240}{760}\cdot \frac{88}{760}$, and the expected number of persons in this sample which are men and left-handed is $$e_{11}=760\cdot P(\text{men and left-handed})=760\cdot \frac{240}{760}\cdot \frac{88}{760}=\frac{240\cdot 88}{760}\approx 27.8$$
> 
> We obtained entry $(1,1)$ and we do the same for the other entries:
> 
> **Expected counts table**:
> 
> |       | Left-hand | right-hand | total |
> | ----- | --------- | ---------- | ----- |
> | men   | 27.8      | 212.2      | 240   |
> | women | 60.2      | 459.8      | 520   |
> | total | 88        | 672        | 760   |
> We then calculate the observed value of the test statistic: $$X^{2}=\frac{(23-27.8)^{2}}{27.8}+\frac{(217-212.2)^{2}}{212.2}+\frac{(65-60.2)^{2}}{60.2}+\frac{(455-459.8)^{2}}{459.8}\approx 1.36$$
> The test is right-sided, so the critical value is $\chi^{2}_{1,0.05}=3.84$ -> $1.36\not > 3.84$
> Conclude that we cannot reject $H_0$. There is not sufficient evidence to reject the claim that gender and left-handedness are independent variables.

---
References: