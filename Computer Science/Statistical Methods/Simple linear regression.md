---
Tags: Math
Created: 2023-12-06 04:03:04
---
(Links:: [[Statistical Methods]])

Our goal now is to find the approximate line on which the data points lie / where the points are closest to the line: $$Y_{i}=\beta_{0}+\beta_{1}x_{i}+e_{i}$$
This is a **simple linear regression model**, where
- $Y$ is the **dependent/response/outcome** variable,
- $X$ is the **independent/explanatory/predictor** variable,
- $\beta_{0},\beta_{1}$ are the unknown model parameters, describing the (true) line.
- The (measurement) **error** terms $e_i$ are independent $N(0,1)$ random variables.

We try to find values for $\beta_{0},\beta_{1}$ such that $e_{i}$ is minimal. Let $b_{0}$, and $b_{1}$ be the estimates of $\beta_{0},\beta_{1}$. To find the best line, we minimize the sum of squared vertical distances between the observations $y_i$ and the line $\beta_{0}-\beta_{1}x$: $$\min\limits_{\beta_{0},\beta_{1}}\sum\limits_{i=1}^{n}(y_{i}-\beta_{0}-\beta_{1}x_{i})^{2}=\sum\limits_{i=1}^{n}(y_{i}-b_{0}-b_{1}x_{i})^{2}=SSE \quad \text{(Sum of Squared Errors)}$$ 
The least squares estimates $b_0$ and $b_1$ that minimize SSE are called **intercept** and **slope**: with the sample correlation $r$ between samples $x$ and $y$ $$b_{0}=\bar y-\hat \beta_{1}\bar x, \qquad b_{1}=r\frac{s_{y}}{s_{x}}=\frac{\sum_{i=1}^{n}(x_{i}-\bar x)(y_{i}-\bar y)}{\sum_{i=1}^{n}(x_{i}-\bar x)^{2}}$$
The fitted **regression equation** is $$\hat Y_{i}=b_{0}+b_{1}X_{i}$$ and it is the *predicted* response.
The **estimated error variance** is $$\hat \sigma^{2}=s^{2}=\frac{1}{n-2}\sum\limits_{i=1}^{n}(y_{i}-b_{0}-b_{1}x_{i})^2=\frac{SSE}{n-2}$$
For any new value $x^*$ of explanatory variable, $$y^*=b_{0}+b_{1}x^*$$ is the predicted value of the response variable. 
> [!question] When can we apply this linear model?
> - The fitted line $\hat y=b_{0}+b_{1}x$ gives a *reasonable fit*
> - **Coefficient of determination** $R^2$ is large
> - Linear relationship is significant: test $\beta_{1}=0$ vs. $\beta\neq 0$
> 
> To test $\beta_{1}=0$ vs. $\beta\neq 0$, we also need that the data follows the simple linear regression model: $Y_{i}=\beta_{0}+\beta_{1}X_{i}+e_{i}$ where there errors should be independent and normally distributed with mean 0 and fixed standard deviation: $$e_{i}\overset{ind}{\sim}N(0,\sigma^{2})$$
> We check this with [[Relationship between variables and linear correlation|scatter plots]] and normal [[Assessing normality and QQ plots|QQ plots]].

The **coefficient of determination** is the proportion of variation in the response ($y$) variable that is explained by the regression equation: $$R^2=\frac{\text{exlained variation}}{\text{total variation}}=\frac{SS_{y}-SSE}{SS_{y}}=\frac{\sum_{i=1}^{n}(Y_{i}-\bar Y)^{2}-\sum_{i=1}^{n}(Y_{i}-\hat Y_{i})^{2}}{\sum_{i=1}^{n}(Y_{i}-\bar Y)^{2}}$$
It is also equal to the squared value of the sample correlation coefficient $r^2$.

# Testing linearity and constructing $(1-\alpha)$-CI for $\beta_1$
We test for $H_{0}:\beta_{1}=0$ vs. $H_{1}:\beta_{1}\neq 0$ (Does the covariate $X$ influence the response $Y$?). Under $H_{0}$, the test statistic is $$T_{\beta}=\frac{b_{1}-\beta_{1}}{s_{b_{1}}}\sim t_{n-1}$$ where $s_{b_{1}}$ is the **standard error** of $b_1$, i.e. the estimated standard deviation of $b_1$. The test for $\beta_{1}=0$ is the *same* as the [[Relationship between variables and linear correlation#^2fdaf3|correlation test]] for $\rho=0$ between the samples $Y$ and $X$.

> [!example]+ There is a linear relationship between weight and price of a diamond
> 1. **Indicate population parameter**: The slope coefficient $\beta_{1}$ of the linear regression model
> 2. **Formulate $H_0$ and $H_1$ and choose significance level $\alpha$**: $H_0:\beta_{1}=0$ vs. $H_{1}:\beta_{1}\neq 0$ with significance level $\alpha=0.01$
> 3. **Collect data**: We have $b_1=6648.3$ and $s_{b_{1}}=1199.0$ (given in exercise). We must *check requirements using plots*
> 4. **Test statistic and $P$-value or critical region**: We use the test statistic under $H_0$ $$T_{\beta}=\frac{b_{1}}{s_{b_{1}}}\sim t_{n-2}=t_{20}$$The observed value is $$t_{\beta}=\frac{6648.3}{1199.0}\approx 5.545$$The test is two-tailed -> Critical values: $-t_{20,0.005}=-2.845$ and $t_{20,0.005}=2.845$. Since $t_{\beta}=5.538>2.845$, it is in the critical region
> 5. $H_0$ is rejected. We have enough evidence to confirm that there is a linear relationship between the weight and price of a diamond

# Checking requirements for testing linearity
We need to check if $$e_{i}\overset{ind}{\sim}N(0,\sigma^2)$$ It is hard to check *independence*, so we assume this to be true. 
We don't know the values of $e_i$, but we can estimate them with residuals: $$\hat e_{i}=y_{i}-\hat y_{i}=y_{i}-b_{0}-b_{1}x_{i}$$
**To** check *normality*, we create a normal QQ plot of residuals. To check for a *fixed standard deviation*, we create a residual plot by plotting the predictor values $x$ against the residuals: there should not be any obvious pattern in this plot.

![[Checking model assumption, Testing linearity.png|700]]

---
References: