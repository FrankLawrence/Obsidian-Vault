---
Tags: 
Created: 2025-03-01 13:27:54
---
(Links:: [[Automata and Complexity]])

- A grammar is in **Chomsky normal form** if all rules have the form $$A\to BC \quad \text{or} \quad A\to a$$
- Note that a grammar in Chomsky normal form contains
	- no [[Context-free Grammars#Removal of Lambda Productions|lambda-production]] rules, and 
	- no [[Context-free Grammars#Removal of Unit Productions|unit production]] rules 

> [!info] Theorem
> For every context-free language $L$ there is a grammar $G$ in *Chomsky normal form* with $L(G)=L\setminus \{\lambda\}$

### Construction
Let $G$ be a context-free grammar without $\lambda$- and unit-productions and $L(G)=L\setminus \{\lambda\}$
1. Introduce variables $C_{a}$ and rules $$C_{a}\to a$$ for every $a\in T$
2. Replace every rules $A\to x_{1}\cdots x_{n}$ where $x_{i}\in V\cup T$ with $n\geq 2$ by $$A\to B_{1}\cdots B_{n}\qquad \text{where} \qquad B_{i}=\begin{cases}x_{i}, & \text{if }x_{i}\in V\\ C_{x_{i}}, &\text{if }x_{i}\in T\end{cases}$$
3. Replace every $A\to B_{1}\cdots B_{n}$ with $n\geq 3$ by $$A\to B_{1}\cdots B_{n-2}X\qquad X\to B_{n-1}B_{n}$$ where $X$ is a fresh variable
4. Repeat the last step until all rules are in Chomsky normal form

> [!example]-
> Transform the following context-free grammar
> $$\begin{align}
> S &\to aAbB \\
> A &\to SbBa\;\vert\; a \\
> B &\to AbBb \;\vert\; aba
> \end{align}$$
> into Chomsky normal form. Note that the grammar has no $\lambda$- and unit-productions.
> 1. We introduce fresh variables $C_{a}$ and $C_{b}$ and rules $$C_{a}\to a \qquad C_{b}\to b$$
> 2. We replace the terminal symbols in all rules that are not in Chomsky normal form by $C_{a}$ and $C_{b}$:
> $$\begin{align}
> S &\to C_{a}AC_{b}B \\
> A &\to SC_{b}BC_{a}\;\vert\; a \\
> B &\to AC_{b}BC_{b} \;\vert\; C_{a}C_{b}C_{a}
> \end{align}$$
> 1. We split rules whose right-hand sides have length > 2.
> $$\begin{align}
> S &\to C_{a}X_{1} \qquad X_{1} \to AX_{2} \qquad\;\, X_{2} \to C_{b}B \\
> A &\to SX_{3} \qquad\;\; X_{3} \to C_{b}X_{4} \qquad X_{4} \to BC_{a} \\
> A &\to a \\
> B &\to AX_{5} \qquad\;\; X_{5}\to C_{b}X_{6} \qquad X_{6}\to BC_{b} \\
> B &\to C_{a}X_{7} \qquad\, X_{7}\to C_{b}C_{a}
> \end{align}$$

---
References: