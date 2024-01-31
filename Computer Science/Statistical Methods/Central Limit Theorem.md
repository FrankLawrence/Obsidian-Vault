---
Tags: 
Created: 2023-11-10 15:28:12
---
(Links:: [[Statistical Methods]])

Usually, we can calculate the [[Mean]] of a certain test ([[Expectation and standard Deviation#Expected value]]). However, if we have a lot of different outcomes, it becomes tedious to compute all the values and their respective probabilities. The solution is the central limit theorem.

Take a sample of size $n > 30$ from a population with mean $\mu$ and standard deviation $\sigma$. Then $\bar X_{n}$ has **approximately** a $N(\mu, \frac{\sigma^{2}}{n})$-distribution, hence the standard deviation is $\frac{\sigma}{\sqrt{n}}$.
**The Central Limit Theorem for normal population**
Take a sample of size $n$ from a **normal** population with mean $\mu$ and standard deviation $\sigma$. Then $$\bar X_{n}\sim N\left(\mu, \frac{\sigma^{2}}{n}\right)$$

> [!example] SAT test scores of math section are approximately $N(500,10000)$-distributed.
> > [!question] A school of 100 students has an average score of 475. What percentage of schools performs better?
> > Distribution of mean SAT score of a school of 100 students is approx. $N(500, \frac{10000}{100})$
> > => $\mu=500$  and $\sigma=\frac{100}{10}=10$
> > The $z$ score of $x=450$ is $\frac{475-500}{10}=-2.5$
> > $$P(\bar X_{n}>475)=P(Z>-2.5)=1-0.0062=0.9938$$

> [!summary] Is a sample mean normally distributed?
> Consider a population distribution with mean $\mu$  and st. deviation $\sigma$. Take a sample of size $n$ from this population. The sample mean $\bar X$ has a normal distribution if 
> 1. Sample size $n > 30$. Then CLT applies and $X$ has approximately a normal distribution with mean $\mu$  and standard deviation $\frac{\sigma}{\sqrt{n}}$. 
> 2. The population distribution is a **normal distribution**. Then, $X$ has a normal distribution with mean $\mu$  and standard deviation $\frac{\sigma}{\sqrt{n}}$ for **any $n$**.

---
References: [[Statistical Methods Lecture4.pdf]]