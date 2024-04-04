---
Tags: 
Created: 2023-12-05 19:32:15
---
(Links:: [[Statistical Methods]])

There are multiple ways of approaching a hypothesis test. We concentrate on the *$P$-value method*, and the **critical value method**. For the critical value method, we construct **critical regions**, which show for which scores we reject $H_0$. We construct them with $\alpha$. There are different critical regions depending on the type of test we are running: ![[Critical Region hypothesis testing.png]]
We would previously calculate the $P$-value for some $z$-score and then check if it is smaller than our significance level $\alpha$. Instead now, we check if the $z$-score (from the test statistic) is in the region marked.

> [!example] Testing a Claim about a proportion 
> With significance level $\alpha=0.01$ in a two sided test, we get a critical region of: $z\leq -2.575$ or $z\geq 2.575$ (See table 2).
> We compute $$z=\frac{0.95/750-0.1}{\sqrt{\frac{0.1\cdot 0.9}{750}}}\approx2.07$$
> The observed value is not in the critical region, so we do *not* reject $H_0$

Lastly, there are some *errors* than can occur with hypothesis testing.

|                     | $H_0$ true   | $H_0$ false   |
| ------------------- | ------------ | ------------- |
| Reject $H_0$        | Type I error | correct       |
| Do not reject $H_0$ | correct      | Type II error |

- Type I error: mistake of rejecting $H_0$ when it is true. Probability of a type I error $\leq \alpha$ (probability is **fixed**)
- Type II error: mistake of not rejecting $H_0$ when it is false. Its probability is denoted by $\beta$ (probability depends on $\alpha$, $n$ and **acutal value** of population parameter)

---
References: