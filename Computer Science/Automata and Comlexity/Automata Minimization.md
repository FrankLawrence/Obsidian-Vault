---
Tags: 
Created: 2025-03-17 12:56:11
---
(Links:: [[Automata and Complexity]])

# Construction minimal DFA $\hat M$ with $L(M)=L(\hat M)$
1. Remove all unreachable states from $M$. Remove all states $q\in Q$ for which there is no path from $q_{0}$ to $q$
2. Partition $Q$ in indistinguishable states
	- States $q_{1},q_{2}\in Q$ are distinguishable if there exists $w\in \Sigma^{*}$ s.t. $$q_{1}\overset{w}{\twoheadrightarrow}q_{1}' \in F \qquad $q_{2}\overset{w}{\twoheadrightarrow}q_{2}' \notin F$$, or vice versa
	- We construct the partitioning stepwise:
		- Initial partitioning is $\{Q\setminus F,F\}$
		- If there are partitions $R$ and $S$ such that $$\delta(q,a)\in S\qquad \text{and}\qquad \delta(q',a)\notin S$$, for some $a\in \Sigma$ and $q,q' \in R$, then we split $R$ in $$\{q\in R \,|\, \delta(q,a)\in S\}\qquad \{q\in R\,|\, \delta(q,a)\notin S\}$$ We keep splitting until no more split is possible
3. Read off the minimal DFA
   Let $Q_{1},...,Q_{n}$ be the final partition of $Q$. These are the *states* of the minimal DFA $\hat M$. The *transitions* of $\hat M$ are: $$Q_{i}\overset{a}{\to} Q_{j}\quad \Longleftrightarrow \quad q\overset{a}{\to} q' \text{ for some } q\in Q_{i},q'\in Q_{j}$$ The *starting state* is the set that contains $q_{0}$. The *final states* are the subsets of $F$

> [!example]-
> ```mermaid
> flowchart LR
> 	A@{ shape: sm-circ, label: "Small start" }
> 	B@{ shape: circ, label: "q<sub>0</sub>" }
> 	C@{ shape: circ, label: "q<sub>1</sub>" }
> 	D@{ shape: circ, label: "q<sub>2</sub>" }
> 	E@{ shape: circ, label: "q<sub>3</sub>" }
> 	F@{ shape: dbl-circ, label: "q<sub>4</sub>" }
> 	A --> B
> 	B -->|a| C
> 	C -->|a| D
> 	D -->|a| C
> 	D -->|b| F
> 	C -->|b| F
> 	B -->|b| E
> 	E -->|a| B
> 	E -->|b| F
> 	F -->|a| E
> 	F -->|b| F
> ```
> 1. All states are reachable
> 2. Initial partitioning: $\{Q\setminus F, F\}=\{\{q_{0},q_{1},q_{2},q_{3}\},\{q_{4}\}\}$ 
>    
>    Splitting $R=\{q_{0},q_1,q_2,q_3\}$ with $S=\{q_4\}$ and letter $b\in \Sigma$.
>    New partitioning: $\{\{q_{0}\},\{q_{1},q_{2},q_{3}\},\{q_{4}\}\}$ 
>    
>    Splitting $R=\{q_1,q_2,q_3\}$ with $S=\{q_0\}$ and letter $a\in \Sigma$.
>    New partitioning: $\{\{q_{0}\},\{q_{1},q_{2}\},\{q_{3}\},\{q_{4}\}\}$
>    
>    Nothing more to split!

# Lexical Analysis
- [[CoCo Language and Grammar#Lexical analyzers/scanners vs acceptors|Lexical analysis]] converts a sequence of characters into a sequence of tokens.

> [!question] How to get from characters to tokens?
> - Regular expressions $r_{1},...,r_{n}$ express the pattern. **Every regular expression corresponds to a token**
> - Lexical analysis repeatedly searches the longest prefix of the input that is matched by one of the regular expressions. This prefix is transformed into a token

---
References: