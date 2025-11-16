---
Tags: 
Created: 2025-03-17 12:09:33
---
(Links:: [[Automata and Complexity]])
> [!summary]
> The following statements are equivalent
> - The language $L$ is **regular**
> - There is a **DFA** $M$ with $L(M)=L$
> - There is a **NFA** $M$ with $L(M)=L$
> - There is a **right linear grammar** $G$ with $L(G)=L$
> - There is a **left linear grammar** $G$ with $L(G)=L$
> - There is a **regular expression** $r$ with $L(r)=L$

- **Elementary Properties of Regular Languages**: If $L_{1},L_{2},L$ are regular languages, then also $$L_{1}\cup L_{2}\qquad L_{1}\cap L_{2}\qquad L_{1}L_{2}\qquad \bar L\qquad L_{1}\setminus L_{2}\qquad L^{*}\qquad L^{R}$$
# Decidable Properties of Regular Languages
- roughly speaking, a property is **decidable** if there is an **algorithm/program** that can tell whether the property holds
- If we say that a property of regular languages is decidable, we implicitly assume that the language is given as (or can be depicted as) a DFA
- **Decidability of Emptiness**: It is decidable whether a regular language $L$ is **empty** 
	- $L$ is empty if the DFA $M$ with $L(M)=L$ has a path from start to final state
- **Decidability of Membership**: It is decidable if a word $u$ is **member** of a regular language $L$
	- conversion to DFA might require many states -> generate states of DFA when reading $u$
- **Decidability of Subsets**: It is decidable for regular languages $L_{1}$ and $L_{2}$ if **$L_{1}\subseteq L_{2}$**
- **Decidability of Equivalence**: It is decidable if two regular languages $L_{1}$ and $L_{2}$ are **equal**
# Word Matching
> [!info] Algorithm: Does a word match a regular expression
> 1. Transform the regular expression $\Sigma^{*}\cdot r$ into an NFA
> 2. Compute on-the-fly path of $u$ in the corresponding DFA
> 3. Terminate as soon as a final state is reached

- Worst-case time complexity: $O(|r|\cdot |u|)$

---
References: