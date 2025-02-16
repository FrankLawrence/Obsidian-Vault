---
Tags: lecture Math
Created: 2022-11-14 17:03:53
---
(Links:: [[Logic and Sets]])
# Disjunctive and conjunctive normal forms
> [!important]+ Theorem
> Propositional logic is *functionally complete*:
> Each truth table can be represented by a propositional formula

> [!important]+ Disjuntive normal form
> - A *literal* is a propositional varialbe or the negation of a propositional variable: $p,q,\lnot q, \lnot r,s,...$
> - A **disjunctive normal form** is a disjunction $\psi_1 \lor \dots \psi_n$ where the formulas $\psi_i$ are conjuntions of *literals*
> - conjuntions and disjunctions are sometimes optional

> [!important]+ Adequate systems
> A system *$C$* of connectives is **adequate** if every truth table can be expressed as a formula with connectives *$C$*
> -> $\{\lnot, \land, \lor\}$ is an *adequate* system of connectives
> -> $\{\lnot, \land\}$ are *adequate* since $\lor$ can be expressed with only $\{\lnot, \land\}$

## Sheffer stroke
- The **Sheffer stroke** $|$ forms an adequate system by itself
$$\phi \mid \psi \equiv \lnot (\phi \land \psi)$$
- **NAND**-operation
- $\phi \mid \phi \equiv \lnot \phi$
$$\phi \lor \psi \equiv \lnot(\lnot\phi\land\lnot\psi)\equiv\lnot\phi\mid\lnot\psi\equiv(\phi\mid\phi)\mid(\psi\mid\psi)$$
- -> $\{\mid\}$ is an adequate system
# Conjunctive normal form
A *conjunctive normal form (CNF)* is analogous to a DNF, but with roles of $\land$ and $\lor$ reversed.
> [!definition]+
> - A **clause** is a *disjuction of literals*
> - A **conjunctive normal form** is a conjunction $\phi_1 \land \dots \phi_n$ where the formulas $\phi_i$ are clauses

## Transformation method to CNF using algorithm *CNF*
The algorithm *CNF* tranforms formulas $\phi$ built from $\lnot,\land,\lor,\to$ to a CNF in 3 steps:
1. *IMPL-FREE*: Eliminate all occurrences of $\to$
2. *NNF*: Bring all occurrences of $\lnot$ inside, until they are all directly in front of a porpositional variable
3. *DISTR*: Use ditributivity to re-arrange $\lor$ and $\land$
   $(\phi\land\psi)\lor\chi \equiv(\phi\lor\chi)\land(\psi\lor\chi)$
   
## Tautology for CNFs
> [!important]+ Criterion
> A CNF $\psi_1\land\psi_2\land\dots\psi_n$ is a tautology *if and only if* each clause $\psi_i$ contains literals $p$ and $\lnot p$ for some $p$ (All conjuncts are tautologies)

# Satisfiability
**Satisfiability problem**: Given a propositional formula $\phi$, try to find a valuation that applied to $\phi$ yields *T*.
## DPLL procedure
We check for satisfiability of a CNF $\phi$
- eliminate "unit" clauses:
	- for each *unit clause* $p$ of $\phi$, replace all $p$'s in $\phi$ by $\top$
	- for each *unit clause* $\lnot p$ of $\phi$, replace all $p$'s in $\phi$ by $\bot$
- eliminate "pure" propositional variabales:
	- if a $p$ occurs only *positively* in $\phi$, replace all $p$'s in $\phi$ by $\top$
	- if a $p$ occurs only *negatively* in $\phi$, replace all $p$'s in $\phi$ by $\bot$
- if $\phi$ becomes $\top$, then it is *satisfiable*
- if $\phi$ becomes $\bot$, then it is *unsatisfiable*
- else choose a $p$ in $\phi$:
	- replace all $p$'s in $\phi$ by $\top$, and apply the DPLL procedure
	- if the outcome is "unsatisfiable", then repalce all $p$'s in $\phi$ by $\bot$, and *again* apply the DPLL procedure

What is P=NP?

---
References: