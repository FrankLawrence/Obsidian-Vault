---
Tags: Math
Created: 2023-11-09 02:16:26
---
(Links:: [[Statistical Methods]])
# *Simple* law of total probability
**Simple law of total probability** using addition rule with disjoint events ($A\cap B$ and $\bar A \cap B$): $$\begin{align}P(B)&=P(A\cap B) + P(\bar A \cap B)\\ &= P(B|A)\times P(A)+P(B|\bar A)\times P(\bar A)\end{align}$$
If we combine [[Conditional probability]] with the multiplication rule, then we get [[Bayes' Theorem]]

# Partition
Events $A_{1},...,A_{m}$ are called **partition** if
- they are pairwise disjoint: $A_{j} \cap A_{j}=\emptyset$, if $i\neq j$
- the union is entire sample space: $A_{1} \cup A_{2} \cup ... \cup A_{m}=\Omega$
# Law of total probability

Let $A_{1},...,A_{m}$ be a partition, then also $B \cap A_{1},...,B \cap A_{m}$ disjoint. Then 
$$\begin{align}
P(B)&=P(B \cap \Omega) = P(B\cap (A_{1}\cup A_{2}\cup ... \cup A_{m}))\\
&= P((B \cap A_{1})\cup(B\cup A_{2})\cup ... \cup (B \cap A_{m}))\\
&= \sum\limits_{i=1}^{m}P(B\cap A_{j})\quad (\text{General addition rule for disjoint events})\\
&= \sum\limits_{i=1}^{m}P(B|A_{i})\times P(A_{j})\quad (\text{multipilcation rule})
\end{align}$$

We can derive [[Bayes' Theorem]]: Let $A_{1},...,A_{m}$ be partition, then $$P(A_{r}|B)=\frac{P(A_{r}\cap B)}{P(B)}=\frac{P(B|A_{r})\times P(A_{r})}{\sum_{i=1}^{m}P(B|A_{i})\times P(A_{i})},\quad r=1,...,m$$


___
References: [[Statistical Methods Lecture3.pdf]]