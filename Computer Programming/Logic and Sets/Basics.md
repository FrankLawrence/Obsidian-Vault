---
Tags: 
Created: 2022-11-01 13:55:19
---
(Links:: [[Logic and Sets]])
```ad-important
collapse: none
title:**Definition**
A **declarative sentence** (or **proposition**) is a statement that is *true* or *false*
```
# Syntax of Propositional Formulas
```ad-important
collapse: none
title:**Propositional logic**
 	A sentence in propositional logic consists of *variables p,q,v,…* and a few simple *connectives* like *and, or, not, implies* and so forth
```

**Connectives**
- $\lnot$ for $not$
- $\land$ for $and$
- $\lor$ for $or$
- $\to$ for $If\dots then\dots$

```ad-important
title:Definition (Propositional Formula)
collapse: none
The set of *propositional formulas* is inductively defined as follows:
1. A propositional variable is a propositional formula. We call such a formula an *atomic formula* or simply *atom*.
2. If $\phi$ and $\psi$ are propositional formulas, then so are ($\lnot \phi$), ($\phi \land \psi$), ($\phi \lor \psi$), ($\phi \oplus \psi$), ($\phi \to \psi$) and ($\phi \leftrightarrow \psi$)
```
## Priority schema
1. $\lnot$
2. $\land \; \lor$
3. $\to \; \leftrightarrow$
## Semantic equivalence
```ad-important
title: Definiton
collapse: none
Formulas $\phi$ and $\psi$ are *semantically equivalent*, notation $\phi \equiv \psi$, if they have identical columsn in their truth tables
```


---
References: