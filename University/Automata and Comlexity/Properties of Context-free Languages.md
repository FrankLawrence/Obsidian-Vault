---
Tags: 
Created: 2025-03-25 22:49:44
---
(Links:: [[Automata and Complexity]])
# Basic Properties

> [!info] THEOREM
> If $L_{1}$ and $L_{2}$ are context-free, then also $$L_{1} \cup L_{2} \qquad L_{1}L_{2} \qquad L_{1}^{*} \qquad L_{1}^{R}$$
> > [!info] Proof

- For context free languages $L_{1}$ and $L_{2}$ the following are **not** always context-free $$L_{1} \cap L_{2} \qquad L_{1} \setminus L_{2} \qquad \overline L_{1}$$

> [!example] $L_{1}\cap L_{2}$
> The languages $L_{1}$ and $L_{2}$ are context-free:
> $$L_{1} = \{a^{n}b^{n}c^{m} \mid n \geq 0 \land m\geq 0\}$$
> $$L_{2} = \{a^{n}b^{m}c^{m} \mid n \geq 0 \land m\geq 0\}$$
> However $L_{1}\cap L_{2} = \{a^{n}b^{n}c^{n} \mid n\geq 0\}$ is **not** context-free.

> [!example] $\overline{L_{1}}$ and $L_{1}\setminus L_{2}$
> $$L_{1} \cap L_{2} = \overline{\overline{L_{1}} \cup \overline{L_{2}}} \qquad \overline{L_{1}}=\Sigma^{*}\setminus L_{1}$$

> [!info] THEOREM
> If $L_{1}$ is context-free and $L_{2}$ regular, then $L_{1} \cap L_{2}$ is context-free
> If $L_{1}$ is context-free and $L_{2}$ regular, then $L_{1} \setminus L_{2}$ is context-free

## Construction
Let
- $M= (Q, \Sigma, \Gamma, \delta, q_{0}, z, F)$ be an [[Pushdown Automata|NPDA]] accepting $L_{1}$ and 
- $N=(R, \Sigma, \epsilon, r_{0}, G)$ a [[Finite Automata#Deterministic Finite Automata|DFA]] accepting $L_{2}$

We construct an NPDA $\hat M = (\hat Q, \Sigma, \Gamma, \hat \delta, \hat q_{0}, z, \hat F)$ where $$\hat Q = Q \times R \qquad \hat q_{0} = (q_{0},r_{0}) \qquad \hat F = F \times G$$
The transition function $\hat \delta$ is defined by:
- $\hat M: (q,r) \xrightarrow{a[b/v]}(q',r') \text{ if }M:q\xrightarrow{a[b/v]} q' \text{ and }N:r\xrightarrow{a}r'$
- $\hat M: (q,r) \xrightarrow{\lambda[b/v]}(q',r) \text{ if }M:q\xrightarrow{\lambda[b/v]} q'$

Then $L(\hat M) = L(M) \cap L(N)$

> [!question] Why does the construction not work for two NPDA's? (instead of an NPDA and a DFA)

- $L \setminus \{\lambda\}$ is context-free for every context-free language $L$

> [!question] Show that the language $L=\{w\in \{a,b,c\}^{*}\mid n_{a}(w) = n_{b}(w)=n_{c}(w)\}$ is not context-free
> For a contradiction, assume $L$ was context-free. The language $L(a^{*}b^{*}c^{*})$ is regular, thus $$L\cap L(a^{*}b^{*}c^{*})=\{a^{n}b^{n}c^{n}\mid n\geq 0\}$$ would be context-free. However, we know that it is not 
> -> *Contradiction*, thus $L$ is not context-free.

# Decidability
Given context-free grammar $G$ and $H$. Which of the following questions are **decidable**?
1. Given $w\in \Sigma^{*}$, do we have $w\in L(G)$?
2. Is $L(G)$ empty?
3. Does $L(G)=\Sigma^{*}$ hold?
4. Does $L(G)$ contain a palindrome ($w=w^{R}$)?
5. Does $L(G)=L(H)$ hold?
6. Is $L(G)\cap L(H)$ empty?

- Only the first two questions are decidable. Remove all $\lambda$ and unit productions
	- $\{v \mid S \Rightarrow^{*} v, \lvert v \rvert \leq \lvert w \rvert \}$ can be computed in finite time
	- $L(G)$ is empty $\iff$ starting variable is useless

---
References: