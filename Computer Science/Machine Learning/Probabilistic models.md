---
Tags: 
Created: 2025-02-25 03:35:03
---
(Links:: [[Machine Learning]])
# Learning with probability
- **frequentist learning**: given data guess the model that generated some data
	- There is no probability, since repeating test should give the same results
- **Maximum likelihood principle**: pick model (or the parameters ${\color{#58F}\theta}$) with highest probability of seeing data we saw $${\color{#0C8}\hat\theta}=\underset{{\color{#58F}\theta}}{\text{arg max}}\, p(\,X\,\vert\, {\color{#58F}\theta}\,)$$
	- The function $L({\color{#58F}\theta})=p(\,X\,\vert\, {\color{#58F}\theta}\,)$ is called the *likelihood*
	- We often maximize the logarithm of the likelihood instead

> [!example]
> $$p(\,\text{Heads}\, \vert\, \text{Straight})=\frac{1}{2} \qquad p(\,\text{Heads}\, \vert\, \text{Bent}\,)=\frac{4}{5}$$
> $$p(\,\text{Tails}\, \vert\, \text{Straight})=\frac{1}{2} \qquad p(\,\text{Tails}\, \vert\, \text{Bent}\,)=\frac{1}{5}$$
> $$\underset{{\color{#F05}\text{Model}}\,\in \,\text{Model Space}}{\text{arg max}}p(\,\text{Data}\,\vert \,{\color{#F05}\text{Model}}\,)$$
> > [!question]- $$\underset{\text{Coin}\,\in\, \{\text{Bent,Straight}\}}{\text{arg max}}p(\,{\color{orange}H}{\color{#58F}T}{\color{orange}H}{\color{orange}H}{\color{orange}H}{\color{#58F}T}{\color{orange}H}{\color{orange}H}{\color{#58F}T}{\color{orange}H}{\color{#58F}T}{\color{orange}H}\,\vert\, \text{Coin}\,)$$
> > $$p(\,\text{D}\,\vert\,\text{Bent}\,)={\color{orange}\frac{4}{5}}{\color{orange}{\color{#58F}\frac{1}{5}}\frac{4}{5}}{\color{orange}\frac{4}{5}}{\color{orange}\frac{4}{5}}{\color{#58F}\frac{1}{5}}{\color{orange}\frac{4}{5}}{\color{orange}\frac{4}{5}}{\color{#58F}\frac{1}{5}}{\color{orange}\frac{4}{5}}{\color{#58F}\frac{1}{5}}{\color{orange}\frac{4}{5}}\approx 0.000268$$
> > $$p(\,\text{D}\,\vert\,\text{Straight}\,)={\color{orange}\frac{1}{2}}{\color{orange}{\color{#28F}\frac{1}{2}}\frac{1}{2}}{\color{orange}\frac{1}{2}}{\color{orange}\frac{1}{2}}{\color{#28F}\frac{1}{2}}{\color{orange}\frac{1}{2}}{\color{orange}\frac{1}{2}}{\color{#28F}\frac{1}{2}}{\color{orange}\frac{1}{2}}{\color{#28F}\frac{1}{2}}{\color{orange}\frac{1}{2}}\approx 0.000244$$

> [!example] Maximum Likelihood using [[Standard normal distribution#Probability density function|Probability Density Function]]

# (Naive) Bayes Classifiers
# Logistic Regression
# Information Theory

---
References: