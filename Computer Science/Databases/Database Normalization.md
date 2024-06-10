---
Tags: 
Created: 2024-05-31 01:03:56
---
(Links:: [[Databases]])
# Functional Dependencies
If the functional dependency is minimal, then we call it a **determinant**. A determinant is like partial key, it uniquely determines some attributes, but not all in general.

> [!definition] 
> A **functional dependency** has the form: $$A_{1},...,A_{n}\to B_{1},...,B_{m}$$
> This functional dependency **holds** (is **satisfied**) for a table $R$, if for all rows $t,u$ of $R$ we have: $$t.A_{1}=u.A_{1}\text{ and }...\text{ and }t.A_{n}=u.A_{n}$$ $$\Rightarrow t.B_{1}=u.B_{1}\text{ and }...\text{ and }t.B_{m}=u.B_{m}$$

In other words, the functional dependency holds if rows of table R agree on all attributes 𝐵1,…,𝐵𝑚 whenever they agree on all attributes 𝐴1,…,𝐴𝑛.
Functional dependencies are **a generalization of keys**: $$\{A_{1},...,A_{n}\}$$
{𝐴1,…,𝐴𝑛} is a key of relation 𝑅(𝐴1,…,𝐴𝑛,𝐵1,…,𝐵𝑚).  
The functional dependency 𝐴1,…,𝐴𝑛→𝐵1,…,𝐵𝑚 holds.
## Cover
> [!definition] Cover
> The **cover** $X^{+}$ of a set of attributes $X$ with respect to a set of functional dependencies $F$ is the set of all attributes $B$ that are uniquely determined by $X$. In a formula, the cover of $X$ is $$X^{+}=\{\;B\;\mid\; F\;\text{ implies }\;X\to B\;\}$$

> [!tip] Computing Covers
> Let $X$ be a set of attributes and $X_{1}\to Y_{1},...,X_{n}\to Y_{n}$ be a set of functional dependencies.
> 1. We start with $Z=X$.
> 2. If there is a functional dependency $X_{i}\to Y_{i}$ such that $Z$ contains $X_{i}$, then add $Y_{i}$ to $Z$.
>    In other words: if $X_{i}\subseteq Y$, then assign $Z=Z\cup Y_{i}$.
> 3. Repeat step (2) until there is nothing to add to $Z$ anymore.
> 
> When the algorithm stops, $Z$ is the cover of $X:X^{+}=Z$.

> [!info] THEOREM
> A functional dependency $X\to Y$ is implied by a set of functional dependencies $F\Longleftrightarrow Y\subseteq X^{+}$.
## Minimal Keys
Let $R$ be a relation with attributes $\Gamma$, and let $F$ be a set of functional dependencies.
> [!info] THEOREM
> A set of attributes $X\subseteq \Gamma$ is a **key** of $R\Longleftrightarrow X^{+}=\Gamma$.

> [!definition]
> A key $X$ is **minimal** if every attribute $A\in X$ is **vital**, that is, $(X-\{A\})^{+}\neq \Gamma$.

> [!tip] Finding some minimal key
> Consider a relation $R$ with attributes $\Gamma$ and a set of functional dependencies $F$.
> 1. Start with $X=\Gamma$
> 2. If there is an attribute $A\in X$ such that $A\in (X-\{A\})^{+}$, then remove $A$ from $X$.
> 3. Repeat step (2) until no more attributes of $X$ can be removed.
> 
> When the algorithm stops, $X$ is a minimal key of $R$.

> [!tip] Algorithm for computing all minimal keys
> Create a set from the attributes.
> Start with the shortest attribute and create its cover. Concat attributes in C (Candidates) that are not in the cover. Remove attributes which are in *Results*
## Determinants
> [!definition] 
> $\{A_{1},...,A_{n}\}$ is a **determinant** for $\{B_{1},...,B_{m}\}$ if and only if the following conditions hold:
> 1. the FD $\{A_{1},...,A_{n}\}\to\{B_{1},...,B_{m}\}$ holds, and
> 2. the **left-hand side is minimal**, that is, whenever any $A_{i}$ is removed, then $A_{1},...,A_{i-1},A_{i+1},A_{n}\to B_{1},...,B_{m}$ does not hold, and
> 3. the FD is **not trivial**, that is, $\{B_{1},...,B_{m}\}\not \subseteq\{A_{1},...,A_{n}\}$.

> [!example] find all determinants of {A,B,D}
> Find keys which contain A,B,D in its cover.
## Canonical Dependencies
> [!tip] Algorithm for computing canonical set of FDs
> 1. Make right-hand sides singular
> 2. Minimize left-hand sides (like when computing covers, remove each $A\in a$ which can be found)
> 3. Remove implied FDs
# Normal Forms
## First Normal Form
> [!definition] 
> The first normal form requires that all table entries are atomic.
## Boyce-Codd Normal Form
> [!definition]
> A relation $R$ is in **Boyce-Codd Normal Form** if all its FDs are implied by its key constraints.
> That is, for every FD $A_{1},...,A_{n}\to B_{1},...,B_{m}$ of $R$ we have:
> 1. $\{B_{1},...,B_{m}\}\subseteq \{A_{1},...,A_{n}\}$ (the functional dependency is trivial), or
> 2. $\{A_{1},...,A_{n}\}$ contains a key of $R$.
## Third Normal Form
> [!definition] 
> A **key attribute** is an attribute that appears in a minimal key.

Assume that FDs with multiple arguments on the right-hand side have been expanded, that is, every FD has a single attribute on the right-hand side.
Then a relation $R$ is in **third normal form** if every FD $A_{1},...,A_{n}\to B$ satisfies at least one of the conditions: 

If a relation is in BCNF, it is automatically in 3NF
# Transformation to normal forms
## Transformation to BCNF
Input: relation $R$ and a set of FDs for $R$.
1. **Compute a canonical set of FDs $F$**
2. **Maximize the right-hand sides of the FDs**
3. **Start with the schema $S=\{R\}$**
4. **Split off violating FDs one by one**:
	1. For every $R_{i}\in S$ and $X\to Y\in F$: if
		1. $X\subseteq R_{i}$ (that is, $X$ is contained in $R_{i}$), and
		2. $R_{i}\not\subseteq X^{+}$ (that is, $X$ is not a key of $R_{i}$), and
		3. $Y\cap R_{i}\neq \emptyset$ (that is, $Y$ overlaps with $R_{i}$),
	2. then, let $Z=Y\cap R_{i}$, and 
		1. remove $Z$ from $R_{i}$, and
		2. add a new relation with attributes $XZ$ to $S$; the key of this relation is $X$.
## Transformation to 3nF

---
References: