---
Tags: Math
Created: 2023-11-25 02:31:12
---
(Links:: [[Statistical Methods]])

With hypothesis testing, we try to reject the **null hypothesis** (main claim) and prove that the **alternative hypothesis** is correct by looking at the outcome and checking if it fits the claim. To test a hypothesis we need data. There are two types of hypotheses: 
- Null hypothesis: $$H_{0}: \text{ population parameter} = \text{hypothetical value}$$
- Alternative hypothesis: Depending on the claim we choose one out of three $$H_{a}: \begin{cases} \text{population parameter} < \text{hypothetical value} & \text{ left-tailed test} \\
\text{population parameter} > \text{hypothetical value} & \text{ right-tailed test} \\
\text{population paramter} \neq \text{hypothetical value} & \text{ two-tailed test}
\end{cases}$$ 
Left- and right-tailed tests:  one-sided tests,
Two-tailed tests: two-sided test

> [!example]+
> Claim: coin is fair (unbiased vs. claim: coin favours tails)
> Experiment: toss coin 100 times
> Outcome: 37 x heads, 63 x tails
> Probability of 37 heads **or less** assuming claim is **true**?
> If coin is fair, this probability is < $0.01$
> => Result "37 heads" **unlikely** with a fair coin.

The aim of "fitting", is to find the probability of "**if $H_0$ were true**, the experiment outcome is at least as extreme as observation.". 
> [!example] Assumption under the null hypothesis
>  Is $\hat p_{100}=0.37$ likely **if we assume that $p=0.5$**? 

The problem is that we need the parameter (in this case $p$) to be tested to find the sample statistic's distribution. To test the fitting, we must compare the results of the sampled statistic to those of under the null hypothesis. We have to convert the sample statistic to a standard normal distributed variable. This is possible, since the sample statistic ($p$) has a sample distribution which is approximately $N\left(n, \frac{p(1-p)}{n}\right)$-distributed. 
$$\text{sample statistic } \hat P_{n}\quad \Rightarrow \quad \text{test statistic } \frac{\hat P_{n}-p_{0}}{\sqrt{\frac{p_{0}(1-p_{0})}{n}}}$$ 

> [!summary] Five steps of Hypothesis testing
> 1. Identify population parameter
> 2. Formulate $H_{0}$ and $H_{a}$ and choose significance level $\alpha$ 
> 3. Collect data
> 4. Choose test statistic and identify it's distribution under $H_{0}$
> 5. Calculate the value of test statistic based on data
> 6. Find $P$-value (depending on which alternative): probability under $H_{0}$ that test statistic has more extreme values than observed value
> 7. Formulate, based on $P$-value and $\alpha$, conclusion regarding $H_{0}$: reject (small $P$-value) or not (large $P$-value)

^8af4c1

> [!caution] Hypothesis testing is asymmetric
> If $H_0$ is not rejected, we still **do not** "accept it"!

In step 6, we must calculate the $P$-value. If the test is 
- left-tailed: $P$-value = area to the **left** of $z$,
- right-tailed: $P$-value = area to the **right** of $z$,
- two-tailed & $z<0$: $P$-value = $2\times$ area to the **left** of $z$,
- two-tailed & $z>0$: $P$-value = $2\times$ area to the **right** of $z$,

After that, we must find a conclusion:
- If $P$-value $\leq \alpha$: **reject** $H_{0}$
- If $P$-value $> \alpha$: **do not reject** $H_{0}$


---
References: