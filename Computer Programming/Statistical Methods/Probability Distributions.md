---
Tags: 
Created: 2023-11-09 20:59:31
---
(Links:: [[Statistical Methods]])
**Random variable**: A *random variable* is a variable that assigns a numerical value to each outcome of a probability experiment ($X,Y,...$).

> [!example] Two coin tosses $HH$
> Throw a fair coin twice. Let the random variable $X$ be the number of heads. $$X(TT)=0 \quad X(HT) = 1 \quad X(TH)=1\quad X(HH)=2$$ $$P(X=0)=\frac{1}{4} \quad P(X=1)=\frac{1}{2}\quad P(X=2)=\frac{1}{4}$$

A **probability distribution** determines probabilities of values of a random variable. It is given by a table, formula or graph. A **discrete random variable** has finite many different values. Its probability distribution is the collection of all their individual probabilities. A **continuous random variable** has uncountably many different values. Its probability distribution is given by the [[Standard normal distribution#Probability density function|probability density function]]. The probability is computed by the area under this function.
Another way of describing $P(X=x)$ is through all outcomes: $$P(X=x)=P(\{\omega \in \Omega : X(\omega)=x\})$$ ^4bef25
> [!tip] Recipe for finding probability distribution of discrete random variable
> 1. Determine sample space of underlying probability experiment and probabilities of outcomes $\omega$
> 2. List values $X(\omega)$ for all $\omega$ in $\Omega$ 
> 3. For each value $x$ of $X$, find all simple events $\{\omega\}$ with value $x$. 
   > Unify: $\{X=x\}=\{\omega : X(\omega)=x\}$
> 4. Probabilities $P(\{\omega\})$ determine probability of $\{X=x\}$: $$P(\{X=x\})=P(\{\omega : X(\omega)=x\})=\sum_{\omega : X(\omega)=x} P(\{w\})$$
> 5. Tabulate results

___
References: [[Statistical Methods Lecture3.pdf]]
