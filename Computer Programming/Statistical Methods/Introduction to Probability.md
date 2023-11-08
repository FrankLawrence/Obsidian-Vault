---
Tags: 
Created: 2023-11-08 16:58:57
---
(Links:: [[Statistical Methods]])
# Basic concepts of Probability
**Probability experiment**: production of (random) outcome.
Example: Die roll, coin toss.
**Sample space $\Omega$**: Set of all possible outcomes.
Example (for dies): $\Omega=\{1,2,3,4,5,6\}$
**Event $A,B,...$**: Collection of outcomes.
Example: $A=\{\text{even number is thrown}\} = \{2,4,6\}$
**Probability measure**: function $P()$ assigning values between 0 and 1 to events.
Example: $P(A)=P(\{2,4,6\})=\frac{1}{2}$

**Interpretation** of probabilities:
- $P(A)=0$: occurrence of $A$ is impossible
  Example: $P(\emptyset)=0$ ($\emptyset$ = empty event: nothing happens)
- $P(A)=1$: occurrence of $A$ is certain. 
  Example: $P(\Omega)=1$
- Event $A$ is **unlikely** when $P(A)$ is small

Three ways to determine **probability** $P(A)$ of event $A$:
1. Estimate with **relative frequency**: $$P(A)=\frac{\text{number of times } A \text{ occurred}}{\text{number of times the procedure was repeated}}$$
2. **Classical (theoretical) approach**: Make probability model (outcome space, probability measure, etc.), compute $P(A)$ (using properties of $P$)
   Examples: rolling dice, card games, etc.
3. **Subjective approach**: (Not of importance for this course) Estimate $P(A)$, based on intuition and/or experience.

Suppose a procedure is repeated (independently). The relative frequency probability of an event $A$ tend towards the true value of $P(A)$ with more repetitions.

> [!example]- Classical (theoretical) approach
> Throw a fair (unbiased) coin 3x
> Probability of 1x Heads?
> Sample space $\Omega$ has $2\times2\times2=8$ outcomes (write "H", "T"): $$\Omega=\{HHH,HHT,HTH,HTT,THH,THT,TTH,TTT\}$$
> Interesting event $A=\{1\text{ Heads}\} \Rightarrow A=\{HTT,THT,TTH\}$
> Here: outcomes *equally likely*, hence: $$P(A)=\frac{\text{number of ways } A \text{ can occur}}{\text{total number of different simple events}}=\frac{3}{8}$$
> **Determining $P(A)$ if all outcomes are equally likely**: $$P(A)=\frac{\text{number of ways } A \text{ can occur}}{\text{total number of different simple events}}$$

**General probability measure for finite/countable sample space**: 
- Sample space $\Omega$ is finite/countable
- Each outcome $\omega \in \Omega$ has a probability:
	- $P(\omega)\geq 0$ and
	- $\sum\limits_{\omega\in\Omega}P(\omega)=1$
- Any event $A$: probability defined by $$P(A)=\sum\limits_{\omega : \omega\in A}P(\omega)$$

## Recap
Three approaches to determine probability:
- Estimating with relative frequency
- Classical (theoretical) approach
- Subjective probability

General recipe to find $P(A)$ (in discrete case):
- Find sample space $\Omega$
- Determine probabilities $P(\omega)$ for all $\omega$ in $\Omega$
- Determine: outcomes forming $A$ and compute $P(A)=\sum\limits_{\omega : \omega\in A}P(\omega)$

# Addition rule

> [!example]
> Draw a card from each of two card decks, totally at random. What is the probability of drawing *at least one* spade?
> 
> Sample space $\Omega=\{SS,SH,HS,HH\}$; outcomes equally likely.
> Then $\{\text{a spade}\}=\{SS,SH,HS\}$, hence $$P(\text{a spade})=P(\{SS,SH,HS\})=3\times \frac{1}{4}=\frac{3}{4}$$

**Addition rule**:
$$P(A \cup B)=P(A)+P(B)-P(A\cap B)$$

> [!question]- $P(\text{"Tails twice or Heads in first throw"})$?
> $$\Omega = \{HHH,HHT, HTH, HTT, THH, THT, TTH, TTT\}$$
> $A=\{\text{Tails twice}\}=\{HTT,THT,TTH\}$, so **$$P(A)=\frac{3}{8}$$**
> $A=\{\text{Heads in first throw}\}=\{HTT,HHT,HTH,HHH\}$, so **$$P(B)=\frac{4}{8}=\frac{1}{2}$$**
> $A\cap B=\{\text{Heads in first throw}\}=\{HTT\}$, so **$$P(A\cap B)=\frac{1}{8}$$**
> $$\begin{align}\Rightarrow P(\text{Tails twice or Heads in first throw})&=P(A\cup B)\\&=P(A)+P(B)-P(A\cap B)\\ &=\frac{3}{8}+\frac{1}{2}-\frac{1}{8}\\&=\frac{3}{4}\end{align}$$

$A$ and $B$ are **disjoint** if they exclude each other, i.e. $A\cap B=\emptyset$. In this specific case we can express the addition rule as $$P(A\cup B)=P(A)+P(B)$$

> [!question]- Roll a fair die once. What is the probability of "even number *or* 3"?
> $$\Omega=\{1,2,3,4,5,6\}$$
> $A=\{\text{even number}\}=\{2,4,6\}$, so $$P(A)=\frac{3}{6}=\frac{1}{2}$$
> $B=\{3\}$, so $$P(B)=\frac{1}{6}$$
> Furthermore, $A\cap B=\emptyset$, so $A$ and $B$ are disjoint. Hence, $$\begin{align}P(A\cup B)&=P(A)+P(B)\\&=\frac{1}{2}+\frac{1}{6}\\&=\frac{2}{3}\end{align}$$

**General addition rule for disjoint events**:
Let $A_{1}, ..., A_{m}$ be disjoint, i.e. $A_{i} \cap A_{j}=\emptyset$ for $i\neq j$. Then: $$P(A_{1}\;\cup\;\cdots\; \cup\; A_{m})=\sum\limits_{i=1}^{m}P(A_{i})$$

**TODO: Example**
$\bar A$ (or $A^{c}$) is the complement of $A$.

**Complement rule**: $$P(\bar A)=1-P(A)$$

---
References: