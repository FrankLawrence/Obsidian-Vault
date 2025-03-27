---
Tags: 
Created: 2025-02-25 03:35:03
---
(Links:: [[Machine Learning]])
# Learning with probability
- **frequentist learning**: given data guess the model that generated some data
	- There is no probability, since repeating test should give the same results
- **Maximum likelihood principle** (MLP): pick model (or the parameters ${\color{#58F}\theta}$) with highest probability of seeing data we saw $${\color{#0C8}\hat\theta}=\underset{{\color{#58F}\theta}}{\text{arg max}}\, p(\,X\,\vert\, {\color{#58F}\theta}\,)$$
	- The function $L({\color{#58F}\theta})=p(\,X\,\vert\, {\color{#58F}\theta}\,)$ is called the *likelihood*
	- We often maximize the logarithm of the likelihood instead

> [!example] Coin Flip
> $$p(\,\text{Heads}\, \vert\, \text{Straight})=\frac{1}{2} \qquad p(\,\text{Heads}\, \vert\, \text{Bent}\,)=\frac{4}{5}$$
> $$p(\,\text{Tails}\, \vert\, \text{Straight})=\frac{1}{2} \qquad p(\,\text{Tails}\, \vert\, \text{Bent}\,)=\frac{1}{5}$$
> $$\underset{{\color{#F05}\text{Model}}\,\in \,\text{Model Space}}{\text{arg max}}p(\,\text{Data}\,\vert \,{\color{#F05}\text{Model}}\,)$$
> The model space in this case are the two types of coins.
> > [!question]- $$\underset{\text{Coin}\,\in\, \{\text{Bent,Straight}\}}{\text{arg max}}p(\,{\color{orange}H}{\color{#58F}T}{\color{orange}H}{\color{orange}H}{\color{orange}H}{\color{#58F}T}{\color{orange}H}{\color{orange}H}{\color{#58F}T}{\color{orange}H}{\color{#58F}T}{\color{orange}H}\,\vert\, \text{Coin}\,)$$
> > $$p(\,\text{D}\,\vert\,{\color{#0C8}\text{Bent}}\,)={\color{orange}\frac{4}{5}}{\color{orange}{\color{#58F}\frac{1}{5}}\frac{4}{5}}{\color{orange}\frac{4}{5}}{\color{orange}\frac{4}{5}}{\color{#58F}\frac{1}{5}}{\color{orange}\frac{4}{5}}{\color{orange}\frac{4}{5}}{\color{#58F}\frac{1}{5}}{\color{orange}\frac{4}{5}}{\color{#58F}\frac{1}{5}}{\color{orange}\frac{4}{5}}\approx 0.000268$$
> > $$p(\,\text{D}\,\vert\,{\color{#F05}\text{Straight}}\,)={\color{orange}\frac{1}{2}}{\color{orange}{\color{#28F}\frac{1}{2}}\frac{1}{2}}{\color{orange}\frac{1}{2}}{\color{orange}\frac{1}{2}}{\color{#28F}\frac{1}{2}}{\color{orange}\frac{1}{2}}{\color{orange}\frac{1}{2}}{\color{#28F}\frac{1}{2}}{\color{orange}\frac{1}{2}}{\color{#28F}\frac{1}{2}}{\color{orange}\frac{1}{2}}\approx 0.000244$$

^2ab53c

> [!example]- Maximum Likelihood using [[Standard normal distribution#Probability density function|Probability Density Function]]
> We want to choose the parameters $\mu$ and $\sigma$ such that the resulting [[Standard normal distribution#The standard normal distribution|Normal Distribution]] matches the data as closely as possible:
> - an off-centre mean would result in most data points getting a low probability density
> - a variance that is too large would prioritize outliers more than the cluster in the middle
> 
> Assume $X$ is a list of single numbers. We try to find $\theta$ such that is maximizes the log probability density of this data.
> $$\begin{align}
> \underset{\theta}{\text{arg max}}\; \ln p(\,X\,\vert\,\theta\,) &= \underset{\theta}{\text{arg max}}\; \ln \prod_{x\in X}p(\,x\,\vert\,\theta\,) \\
> &= \underset{\theta}{\text{arg max}}\; \sum\limits_{x}\,\ln p(\,x\,\vert\,\theta\,) \\
> &= \underset{{\color{orange}\mu},{\color{#58F}\sigma}}{\text{arg max}}\; \sum\limits_{x}\,\ln \frac{1}{\sqrt{2\pi{\color{#58F}\sigma}^{2}}}\exp \left[- \frac{1}{2{\color{#58F}\sigma}^{2}}(x-{\color{orange}\mu})^{2}\right] \qquad \Big(p(\,x\,\vert\,{\color{#58F}\theta}\,)=N(\,x\,\vert\,{\color{orange}\mu},{\color{#58F}\sigma}\,)\;\text{with }{\color{#58F}\theta}=({\color{orange}\mu},{\color{#58F}\sigma})\Big)\\
> &= \underset{{\color{orange}\mu},{\color{#58F}\sigma}}{\text{arg max}}\; \sum\limits_{x}\,\ln \frac{1}{\sqrt{2\pi{\color{#58F}\sigma}^{2}}}- \frac{1}{2{\color{#58F}\sigma}^{2}}(x-{\color{orange}\mu})^{2}
> \end{align}$$
> The resulting [[Linear models and search#Searching for a good model|loss surface]] is shown below; We can solve it analytically or using [[Linear models and search#Gradient Descent|Gradient Descent]]
> ![[Maximum Likelihood Normal Distribution.svg|500]] 
> - We can choose to only solve for $\mu$
> $$\begin{align}
> \underset{{\color{orange}\mu}}{\text{arg max}}\; \sum\limits_{x}\,\ln \frac{1}{\sqrt{2\pi{\color{#888}\sigma}^{2}}}- \frac{1}{2{\color{#888}\sigma}^{2}}(x-{\color{orange}\mu})^{2} &= \underset{{\color{orange}\mu}}{\text{arg max}}\; \sum\limits_{x}\, - \frac{1}{2{\color{#888}\sigma}^{2}}(x-{\color{orange}\mu})^{2} \quad (\text{first term doesn't contain } {\color{orange}\mu}) \\
> &= \underset{{\color{orange}\mu}}{\text{arg max}}\;-\frac{1}{2{\color{#888}\sigma}^{2}}\; \sum\limits_{x}\,(x-{\color{orange}\mu})^{2} \\
> &= \underset{{\color{orange}\mu}}{\text{arg max}}\;-\sum\limits_{x}\,(x-{\color{orange}\mu})^{2} \quad (\text{positive constant factor won't affect the maximum})\\
> &= \underset{{\color{orange}\mu}}{\text{arg min}}\;\sum\limits_{x}\,(x-{\color{orange}\mu})^{2} \\
> \end{align}$$
> The solution for the mean is simply the [[Mean Squared Error Loss]] (and as described in [[Data pre-processing]] is also the arithmetic mean of the data)
## Bayesian Learning
- contrary to frequentist model uses *subjectivist probability* (we are free to assign each potential model a probability)
- With this we don't simply get which model is more likely, but a distribution over all available models (called the **posterior distribution**)
	- From [[Bayes' Theorem]]: 
	  $$\overbrace{p(\,{\color{#58F}\theta}\,\vert\,X\,)} ^{{\color{#58F}\text{posterior distribution}}} = \frac{ \overbrace{p(\,X\,\vert\,{\color{#58F}\theta}\,)}^{\text{data distribution}} \quad \overbrace{p({\color{#58F}\theta})}^{{\color{#58F}\text{prior distribution}}}} {\underbrace{p(X)}_{\text{model evidence}}}$$
	- **prior distribution**: belief about true model before seeing data; it allows us to *encode our assumptions about the problem*

> [!example]- Coin Flip
> - prior distribution: assuming a friend picks either coin randomly we use $0.5$ for $p({\color{#58F}\theta})$
> - model evidence $p(D)$: how likely are we to see this data at all? 
>   $$\begin{align}
>   p(D) &= p(D,{\color{#F05}\text{Straight}})+p(D,{\color{#0C8}\text{Bent}}) \\
>   &= p(D\,\vert\,{\color{#F05}\text{Straight}})p({\color{#F05}\text{Straight}})+p(D\,\vert\,{\color{#0C8}\text{Bent}})p({\color{#0C8}\text{Bent}})
>   \end{align}$$
> - Now we can calculate the probability of a certain model (coin) being used given some data $D = {\color{orange}H}{\color{#58F}T}{\color{orange}H}{\color{orange}H}{\color{orange}H}{\color{#58F}T}{\color{orange}H}{\color{orange}H}{\color{#58F}T}{\color{orange}H}{\color{#58F}T}{\color{orange}H}$:
> 
> $$\begin{align*}
> p({\color{#F05}\text{Straight}}\,\vert\,D) &= \frac{p(D\,\vert\,{\color{#F05}\text{Straight}})p({\color{#F05}\text{Straight}})}{p(D\,\vert\,{\color{#F05}\text{Straight}})p({\color{#F05}\text{Straight}})+p(D\,\vert\,{\color{#0C8}\text{Bent}})p({\color{#0C8}\text{Bent}})} \\
> &= \frac{p(D\,\vert\,{\color{#F05}\text{Straight}})0.5}{p(D\,\vert\,{\color{#F05}\text{Straight}})0.5+p(D\,\vert\,{\color{#0C8}\text{Bent}})0.5} \\
> &= \frac{p(D\,\vert\,{\color{#F05}\text{Straight}})}{p(D\,\vert\,{\color{#F05}\text{Straight}})+p(D\,\vert\,{\color{#0C8}\text{Bent}})} \\
> p({\color{#0C8}\text{Bent}}\,\vert\,D) &= \frac{p(D\,\vert\,{\color{#0C8}\text{Bent}})}{p(D\,\vert\,{\color{#F05}\text{Straight}})+p(D\,\vert\,{\color{#0C8}\text{Bent}})} \\
> \end{align*}$$
> We had computed the values [[#^2ab53c|previously ]] and can now insert those values:
> $$\begin{align*}\\
> p({\color{#F05}\text{Straight}}\,\vert\,D) &= 0.48 \\
> p({\color{#0C8}\text{Bent}}\,\vert\,D) &= 0.52 \\
> \end{align*}$$

> [!attention] As the models grow more complex, it gets more difficult to approximate the *posterior*

# (Naive) Bayes Classifiers
- **probabilistic classifiers**: give a *probability over all classes* for an instance (can be used for ranking)
	- *generative classifier*: learn probability of the data ($X$) given a class ($Y$)
		- Bayes Classifier: Learn a single distribution $P(\,X\,\vert\,Y\,)$. Reasonable approach for low-dimensional data
		- Naive Bayes Classifier: Assume conditionally independent features. Simple, cheap and effective for high-dimensional data
	- *discriminative classifier*: learn a function for $p(\,Y\,\vert\,X\,)$ with $X$ as input and class probabilities as output ([[#Logistic Regression]])
## Bayes Classifier
<!-- In a binary classification, if we compute the $p(\,X\,\vert\,Y\,)$ (probability of an instance given a class) and $p(Y)$ (the prior probability of the class) for both classes, we get the class probabilities. -->
- With MVNs (multivariate normal distributions), given a dataset with $N$ features we can find vector $\mu$ and matrix $\Sigma$ that give us the best fitting MVN (from MLP)

The **Bayes classifier algorithm** works as follows:
- Choose class of probability distributions $M$ (ex. MVNs) and use maximum likelihood estimates to
	- fit params $\mu_{p}, \Sigma_{p}$ to all *positive* points: $p(\,x\,\vert\,\text{pos}\,)=N(\,x\,\vert\,\mu_{p},\Sigma_{p})$
	- fit params $\mu_{n}, \Sigma_{n}$ to all *negative* points: $p(\,x\,\vert\,\text{neg}\,)=N(\,x\,\vert\,\mu_{n},\Sigma_{n})$
- Estimate $P(Y)$ from the class frequencies in the training data, or use domain-specific information
- Compute class probabilities/probability densities $p(\,\text{pos}\,\vert\,x\,)$ or $p(\,\text{neg}\,\vert\,x\,)$

## Naive Bayes Classifier
- Needed for models with many features
- Assumes that all features are independent, **conditional on the class**

$$p(X_{1},X_{2}\,\vert\,Y)=p(X_{1}\,\vert\,Y)p(X_{2}\,\vert\,Y)$$
- The conditional distribution over the class variable $Y$ can be expressed as 
  $$\begin{align}p(Y\,\vert\,X_{1},...,X_{n}) &\propto p(X_{1},...,X_{n}\,\vert\,Y)p(Y) \\
  &=p(X_{1}\,\vert\,Y)\times \cdots \times p(X_{n}\,\vert\,Y)p(Y) \\
  &= p(Y)\prod_{i=1}^{n}p(X_{i}\,\vert\,Y)
  \end{align}$$

> [!example] Email Classification
> | features | ${\color{#F05}\text{spam}}$ | ${\color{#F05}\text{spam}}$ | ${\color{#0C8}\text{ham}}$ | ${\color{#0C8}\text{ham}}$ | ${\color{#0C8}\text{ham}}$ | ${\color{#0C8}\text{ham}}$ | ${\color{#0C8}\text{ham}}$ | ${\color{#F05}\text{spam}}$ | ${\color{#F05}\text{spam}}$ | ${\color{#F05}\text{spam}}$ | ${\color{#0C8}\text{ham}}$ |
> | -------- | ---- | ---- | --- | --- | --- | --- | --- | ---- | ---- | ---- | --- |
> | $X_{2}$: "meeting" | T    | F    | T   | T   | T   | T   | T   | F    | F    | F    | F   |
> | $X_{1}$: "pill"    | T    | T    | T   | T   | F   | F   | F   | F    | T    | F    | F   |
> 
> $$p(X_{1}=T\,\vert\,\text{${\color{#0C8}\text{ham}}$})=2/6$$
> $$p(X_{1}=F\,\vert\,\text{${\color{#0C8}\text{ham}}$})=4/6$$
> $$p(X_{1}=T\,\vert\,\text{${\color{#F05}\text{spam}}$})=3/5$$
> $$p(X_{1}=F\,\vert\,\text{${\color{#F05}\text{spam}}$})=2/5$$
> > [!question]- Which class has a higher probability for an email with both "pill" and "meeting"?
> > $$p({\color{#0C8}\text{ham}}\,\vert\, X_{1}=T,X_{2}=T)\propto p(X_{1}=T,X_{2}=T\,\vert\,{\color{#0C8}\text{ham}})p({\color{#0C8}\text{ham}})$$
> > $$\begin{align}
> > &= p(X_{1}=T\,\vert\,{\color{#0C8}\text{ham}})p(X_{2}=T\,\vert\,{\color{#0C8}\text{ham}})p({\color{#0C8}\text{ham}}) \\
> > &= \frac{2}{6} \times \frac{5}{6} \times \frac{6}{11}=\frac{5}{33}\approx 0.15
> > \end{align}$$
> > $$p({\color{#F05}\text{spam}}\,\vert\, X_{1}=T,X_{2}=T)\propto p(X_{1}=T,X_{2}=T\,\vert\,{\color{#F05}\text{spam}})p({\color{#F05}\text{spam}})$$
> > $$\begin{align}
> > &= p(X_{1}=T\,\vert\,{\color{#F05}\text{spam}})p(X_{2}=T\,\vert\,{\color{#F05}\text{spam}})p({\color{#F05}\text{spam}}) \\
> > &= \frac{3}{5} \times \frac{1}{5} \times \frac{5}{11}=\frac{3}{55}\approx 0.055
> > \end{align}$$

- Issue: if for some feature a value does not occur, the probability for a class will be 0
- *Laplace smoothing*: for each possible value, add one instance where all features have that value
	- when calculating the posterior we can also simply add 1 to the numerator, and $v$ (the different values $X_{1}$ can take) to the denominator
	  $$p(X_{1}=T\,\vert\,Y={\color{#F05}\text{spam}})=\frac{\text{freq. of T in }{\color{#F05}\text{spam}}\text{ data}}{\text{total \# of }{\color{#F05}\text{spam}}\text{ instances}}\qquad \text{unsmoothed}$$
	  $$p(X_{1}=T\,\vert\,Y={\color{#F05}\text{spam}})=\frac{\text{freq. of T in }{\color{#F05}\text{spam}}+1}{\text{total \# of }{\color{#F05}\text{spam}}\text{ instances}+v}\qquad \text{smoothed}$$
	- for a smaller impact of pseudo-observations use $0<\lambda<1$
	  $$p(X_{1}=T\,\vert\,Y={\color{#F05}\text{spam}})=\frac{\text{freq. of T in }{\color{#F05}\text{spam}}+{\color{#58F}\lambda}}{\text{total \# of }{\color{#F05}\text{spam}}\text{ instances}+{\color{#58F}\lambda} v}\qquad {\color{#58F}\lambda}-\text{smoothed}$$

> [!summary]-
> - Bayesian vs frequentist learning. Use what works, mix-and-match
> - *Discriminative* classification: learn $p(Y\,\vert\,X)$ directly
> - *Generative* classification: learn $p(X\,\vert\,Y)$ and $p(Y)$, apply Bayes
> 	- Naive Bayes: Assumes independent features (conditional on the class)
> - Laplace smoothing: add pseudo-observations to avoid zero probabilities

# Logistic Regression
- learns to map features directly to class probabilities without Bayes' rule
- linear classifier with specific loss function
- for binary classification, we determine if $w^{\top}x+b$ is positive or negative -> use regression model
	- instead assign points the probability of being positive (from 0 to 1) and use a **logistic sigmoid** function (to only get values between 0-1) 

![|500](https://mlvu.github.io/probability/31.ProbabilisticModels1.key-stage-0050anim1.svg)
- **log**(**arithmic**) **loss**: 
	- $x$: Some data point
	- $q_{x}$: our classifier $q_{x}(C)=p(C\,\vert\,x)$ $$q_{x}(\text{Pos})=0.1\quad q_{x}(\text{Neg})=0.9$$
	- split data into positives $X_{P}$ and negatives $X_{N}$
	- Find the classifier $q$ that maximizes the probability of the true class (use maximum likelihood objective)
- assume instances are independent -> probability of all class labels is product of all individual class labels $$p(D)=\prod_{x,C\in D} q_{x}(C)$$

- log loss function:
$$\begin{align}
\underset{q}{\text{arg max}} \prod_{C,x}q_{x}(C) &= \underset{q}{\text{arg max }} \log \prod_{C,x}q_{x}(C) \\
&= \underset{q}{\text{arg min }} - \log \prod_{C,x}q_{x}(C) \\
&= \underset{q}{\text{arg min }} \sum\limits_{C,x} -\log q_{x}(C) \\
&= \underset{q}{\text{arg min }} - \sum\limits_{x\in X_{P}} \log q_{x}(P) - \sum\limits_{x\in X_{N}} \log q_{x}(N)\\
\end{align}$$
- logarithmic loss tries to maximize the sum of their probabilities (arrows pushing on curve) ![|500](https://mlvu.github.io/probability/31.ProbabilisticModels1.key-stage-0055.svg)
- To minimize we will use gradient descent which requires computing the gradient: 
  $$\begin{align}
  \frac{\partial \text{loss}({\color{orange}w}, {\color{#58F}b})}{\partial {\color{orange}w_{i}}} &= \frac{\partial \left( - \sum_{x\in X_{P}} \log q_{x}(P) - \sum_{x\in X_{N}}\log q_{x}(N)\right)}{\partial {\color{orange}w_{i}}} \\
  &= \sum\limits_{x\in X_{P}} \bbox[5px,border:2px solid #F08]{- \frac{\partial \log q_{x}(P)}{\partial {\color{orange}w_{i}}}} + \sum\limits_{x\in X_{N}}- \frac{\partial \log q_{x}(N)}{\partial {\color{orange}w_{i}}}
  \end{align}$$

$${\color{#F08}y}={\color{orange}w}^{\top}x+{\color{#58F}b} \quad\to\quad \frac{\partial {\color{#F08}y}}{\partial {\color{orange}w_{i}}}=x_{i} $$
$$\begin{align}
-\frac{\partial \log q_{x}(P)}{\partial {\color{orange}w_{i}}} &= - \frac{\partial \log \sigma({\color{#F08}y})}{\partial {\color{orange}w_{i}}} \\
&= - \frac{\partial \log \sigma({\color{#F08}y})}{\partial \sigma({\color{#F08}y})} &&\times \frac{\partial \sigma({\color{#F08}y})}{\partial {\color{#F08}y}} &&\times \frac{\partial {\color{#F08}y}}{\partial {\color{orange}w_{i}}} \\
&= -\frac{1}{\cancel{\sigma({\color{#F08}y})}} &&\times \cancel{\sigma({\color{#F08}y})}(1-\sigma({\color{#F08}y})) &&\times x_{i} \\
&= -(1-\sigma({\color{#F08}y}))x_{i} \\ 
&= -q_{x}(N)x_{i}
\end{align}$$
# Information Theory
- we can encode an outcome of decision tree with *codes*
	- the trees are called **prefix-free trees** and the resulting codes **prefix-free codes** -> no codeword is the prefix of another (just like with [[Data Structures and Algorithms for CS Lecture 13#Huffman codes]])
	- smaller codes have a higher probability and vice versa: $$p(x)=2^{-L(x)}$$ where $L(x)$: Length of code for $x$
	- the *negative logarithm* of a probability is the **code-lengths of the outcomes under the corresponding codes**: $$L(x)=-\log_{2}p(x)$$
	- we can equate codes with probability distributions: every code gives us a distribution and every distribution gives us a code
## Entropy
- we can see if the corresponding codes are the best by computing the **expected number of bits we will have to use per outcome**
- **Entropy**: code-length of each outcome multiplied with probability and summed $$H(p)=\sum\limits_{x\in X}p(x)L(x)$$
- entropy of a distribution is the expected codelength of an element sampled from that distribution
- **Uncertainty**: Indicates if words are uniformly long (and thus their probability mass) -> more uniform the higher the entropy
- cross-entropy: expected code-length if we use distribution $q$ (our model), but the data comes from distribution $p$ (source of our data) $$\begin{align}H(p,q) &= \mathbb{E}_{p}L^{q}(x) \\ &= - \sum\limits_{x\in X}p(x)\log q(x)\end{align}$$
- The cross entropy is a good way to **quantify the distance between two distributions** (because it’s minimal when the two are the same).

---
References: