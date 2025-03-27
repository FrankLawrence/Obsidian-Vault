---
Tags: 
Created: 2025-03-27 18:18:01
---
(Links:: [[Automata and Complexity]])
# Minimal DFA
- Create $\{Q\setminus F, F\}$
- Split sets into two, those that
	- have a transition to another set
	- don't have a transition to that set (with the same symbol)
# NFA to DFA
After reading $w$, the DFA should be in a state $\{X\}$ that contains all states $q$ that the NFA can be in after reading $w$
# Strict right-linear Grammar
For rule $$A\to u[B]$$ where $u=aw$ split into $$A\to aX \quad \text{ and } \quad X \to w[B]$$ ($[B]$ can be empty)
# Strictly right linear grammar to NFA
- For $S\to wX$ we have $q\xrightarrow{w}X$
- For $S\to w$ we have $q\xrightarrow{w}F$
# Remove useless variables
- Non-productive variables:
	- Create $P = \emptyset$
	- Add $A$ if for $A\to y$ all variables in $y$ are in $P$
-> remove rules that have non-productive variables
- Non-reachable variables: $B$ where $S \cancel{\Rightarrow^{*}} xBy$
-> remove rules that have non-rechable variables

# LL(1)
- $First(w)$: expand the RHS of rules
- $Follow(w)$: look for the variable in the RHS
	- if a $\lambda$ comes after, check $Follow(LHS)$
- Parsing table:
	- For all rules construct $First(RHS)$ and $Follow(LHS)$ if it contained $\lambda$

---
References: