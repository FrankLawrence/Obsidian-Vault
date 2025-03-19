---
Tags: 
Created: 2025-03-17 11:06:09
---
(Links:: [[Automata and Complexity]])
> [!info] Regular Expression
> We define the regular expressions over an alphabet $\Sigma$:
> - $\emptyset$ is a regular expression
> - $\lambda$ is a regular expression
> - $a$ is a regular expression for every $a \in \Sigma$
> - $r_{1}+r_{2}$ is a regular expression for all regular expr. $r_{1}$ and $r_{2}$
> - $r_{1}\cdot r_{2}$ is a regular expression for all regular expr. $r_{1}$ and $r_{2}$
> - $r^{*}$ is a regular expression for all regular expressions $r$

- Every regular expression $r$ defines a language $L(r)$
- A language $L$ is regular $\Longleftrightarrow$ there is a regular expression $r$ with $L(r)=L$

> [!info]+ Construction NFA from regular expression
> We construct $M$ by induction (recursion) on $r$:
> ![[Pasted image 20250317113743.png|500]]

> [!info]+ Construction regular expression from NFA
> 1. Transform $M$ such that there is precisely one initial and final state
> 2. Remove all double arrows (q1 can be equal to q2): 
> ```mermaid
> flowchart LR
> q1 -->|r1| q2
> q1 -->|r2| q2
> ```
> ```mermaid
> flowchart LR
> q1 -->|r1+r2| q2
> ```
> 3. Pick one state $q$ that is neither a starting nor final state. We remove $q$ as follows:
> ```mermaid
> flowchart LR
> q1 -->|r1| q -->|r| q -->|r2| q2
> ```
> ```mermaid
> flowchart LR
> q1 -->|r1·r*·r2| q2
> ```
> *Repeat* step 2 and 3 until there is nothing to be done.
> 4. If $F\neq \{q_{0}\}$, then the transition graph is finally of the form
>    If an arrow $r_{i}$ with $1\leq i \leq 4$ does not exist, let $r_{i}=\emptyset$. Then the regular expression is: $$L(r_{1}^{*}\cdot r_{2}\cdot (r_{4}+r_{3}\cdot r_{1}^{*}\cdot r_{2})^{*})=L(M)$$

---
References: