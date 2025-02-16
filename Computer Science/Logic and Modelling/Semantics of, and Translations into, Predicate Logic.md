---
Tags: Math
Created: 2024-05-07 12:42:06
---
(Links:: [[Logic and Modelling]])
# [[Equivalence relations|Logic Equivalence ]]
> [!definition] Logical equivalence
> Formulas $\phi$ and $\psi$ are **logically equivalent**, denoted by $$\phi = \psi$$ if for all models $\mathcal{M}$ and environments $\mathscr{l}$: $$\mathcal{M}\vDash_{\mathscr{l}}\;\phi \Longleftrightarrow \mathcal{M}\vDash_{\mathscr{l}}\;\psi$$
> That is, $\phi$ and $\psi$ are true in precisely the same models when interpreted with the same environments.

> [!info] Theorem
> $$\phi \equiv \psi \quad \Longleftrightarrow \quad \phi \vDash \psi \;\textrm{ and }\; \psi \vDash \phi$$
> > [!info]- Proof
> > $$\begin{align}
> > \phi & \equiv \psi \\
> > 	& \Longleftrightarrow \textrm{for all } \mathcal{M} \textrm{ and } \mathscr{l}: \\  
> > 	& \qquad \;(\mathcal{M} \vDash_{\mathscr{l}} \,\phi\Rightarrow \mathcal{M} \vDash_{\mathscr{l}}\,\psi) \textrm{ and } (\mathcal{M} \vDash_{\mathscr{l}} \,\psi\Rightarrow \mathcal{M} \vDash_{\mathscr{l}}\,\phi) \\
> > 	& \Longleftrightarrow \textrm{for all } \mathcal{M} \textrm{ and } \mathscr{l}: (\mathcal{M} \vDash_{\mathscr{l}} \,\phi\Rightarrow \mathcal{M} \vDash_{\mathscr{l}}\,\psi) \\  
> > 	& \qquad \,\textrm{ and for all } \mathcal{M} \textrm{ and } \mathscr{l}: (\mathcal{M} \vDash_{\mathscr{l}} \,\psi\Rightarrow \mathcal{M} \vDash_{\mathscr{l}}\,\phi) \\
> > 	& \Longleftrightarrow \phi \vDash \psi \;\textrm{ and }\; \psi \vDash \phi
> > \end{align}$$

> [!example]- Proving Logical Equivalence
> $$\neg \forall x P(x)\equiv \exists x\neg P(x)$$
> For all models $\mathcal{M}$ with domain $A$ and environments $\mathscr{l}$ we find:
> $$\begin{align}
> \mathcal{M} & \vDash_{\mathscr{l}} \;\neg \forall xP(x) \\
> 	& \Longleftrightarrow \text{not: } \mathcal{M} \vDash_{\mathscr{l}} \forall x P(x) \\
> 	& \Longleftrightarrow \text{not for all } a\in A: \mathcal{M} \vDash_{\mathscr{l}[x\mapsto a]} P(x) \\
> 	& \Longleftrightarrow \text{there exists } a \in A \text{ such that not: } \mathcal{M} \vDash_{\mathscr{l}[x\mapsto a]} P(x) \\
> 	& \Longleftrightarrow \text{there exists } a \in A \text{ such that: } \mathcal{M} \nvDash_{\mathscr{l}[x\mapsto a]} P(x) \\
> 	& \Longleftrightarrow \text{there exists } a \in A \text{ such that not: } \mathcal{M} \vDash_{\mathscr{l}[x\mapsto a]} \neg P(x) \\
> 	& \Longleftrightarrow \mathcal{M} \vDash_{\mathscr{l}} \exists x \,\neg P(x)
> \end{align}$$
> Hence we can conclude: $\neg \forall xP(x) \equiv \exists x\,\neg P(x)$.

# Satisfiability, Validity, and Consistency
> [!definition] Satisfiability, validity of formulas
> Let $\phi$ be a formula, and $\Gamma$ be a set of formulas.
> - $\phi$ is **satisfiable** <==> there is *some* model $\mathcal{M}$ and *some* environment $\mathscr{l}$ such that $\mathcal{M}\vDash_{\mathscr{l}} \;\phi$.
> - $\phi$ is **valid** <==> $\mathcal{M}\vDash_{\mathscr{l}} \;\phi$ holds for *all* models $\mathcal{M}$ and *all* environments $\mathscr{l}$ in which $\phi$ can be checked.
> - $\Gamma$ is **consistent/satisfiable** <==> there is *some* model $\mathcal{M}$ and *some* environment $\mathscr{l}$ such that $$\mathcal{M}\vDash_{\mathscr{l}}\;\psi \quad \text{ for all } \quad\psi \in \Gamma$$

For all formulas $\phi$, $\psi$ the following statements are equivalent:
- $\phi\equiv\psi$ 
- $\phi\leftrightarrow \psi$ is valid

> [!info]- Proof
> For all models $\mathcal{M}$ and environments $\mathscr{l}$ it holds:
> $$\begin{align}
> \mathcal{M} & \vDash_{\mathscr{l}} \;\phi \leftrightarrow \psi \\
> 	& \Longleftrightarrow \mathcal{M} \vDash_{\mathscr{l}} (\phi \to \psi)\land (\psi \to \phi) \\
> 	& \Longleftrightarrow \mathcal{M} \vDash_{\mathscr{l}} \phi \to \psi \text{ and } \mathcal{M} \vDash_{\mathscr{l}} \psi \to \phi \\
> 	& \Longleftrightarrow \quad\;\;\; \text{if } \mathcal{M} \vDash_{\mathscr{l}} \phi, \text{ then }\mathcal{M} \vDash_{\mathscr{l}} \psi, \\
> 	& \qquad\;\, \text{and if } \mathcal{M} \vDash_{\mathscr{l}} \psi, \text{ then } \mathcal{M} \vDash_{\mathscr{l}} \phi \\
> 	& \Longleftrightarrow (\mathcal{M} \vDash_{\mathscr{l}} \phi \Longleftrightarrow \mathcal{M}\vDash_{\mathscr{l}}\psi)
> \end{align}$$
> From this we conclude: $\phi\leftrightarrow \psi$ is valid if and only if $\phi\equiv \psi$

> [!example]- $\mathcal{F}=\{\text{alma}_{/0}\},\mathcal{P}=\{\text{loves}_{/2}\}$
> "None of Alma's lovers' lovers loves her" translated to the formula $\phi$:
> $$\forall x\forall y((\text{loves}(x,\text{alma })\land \text{ loves}(y,x))\to \neg \text{loves} (y,\text{alma}))$$
> > [!example]- Satisfiable Formula
> > Consider a model $\mathcal{M}$ with the domain $A_{1}=\{a,g,f,w\}$ and: $$\text{alma}^{\mathcal{M}_{1}}=a\qquad\qquad \text{loves}^{\mathcal{M}_{1}}=\{\langle g,a \rangle, \langle f,a\rangle, \langle w,a\rangle\}$$
> > In this model there are not any lovers of lovers.
> > Hence: $\mathcal{M}_{1}\vDash \phi$
> 
> > [!example]- Not valid Formula
> > Consider a model $\mathcal{M}_{2}$ with domain $A_{2}=\{a,g,f,w\},\text{alma}^{\mathcal{M}_{1}}=a$, and: $$\text{loves}^{\mathcal{M}_{2}}=\{\langle a,g\rangle, \langle a,f\rangle, \langle a,w\rangle,\langle a,a\rangle, \langle g,a\rangle, \langle f,a\rangle,\langle w,a\rangle\}$$
> > In this model:
> > - Alma herself is a lover of a lover of herself
> > - Alma loves herself
> >
> > Hence: $\mathcal{M}_{2}\nvDash \phi$
> 
> > [!example]- Inconsistent Set of Formulas
> > The set $\Gamma=\{\phi, \text{loves}(\text{alma,alma})\}$ is **inconsistent**: Suppose that $\mathcal{M}$ is a model with $\mathcal{M}\vDash \text{loves}(\text{alma,alma})$. Then in $\mathcal{M}$:
> > - Alma is a lover of Alma
> > - Alma is a lover of a lover of Alma
> > - Alma is a lover's lover of Alma who loves Alma
> > 
> > Consequently $\mathcal{M}\nvDash \phi$
> > Hence there is no model satisfying both formulas in the set.

# Translation into Predicate Logic and the Interplay of Quantifiers


# Interplay between Quantifiers and Connectives
> [!definition] Proposition
> For all variables $\mathbf{x}$ and formulas $\phi$ it holds:
> 1. $\neg \forall \mathbf{x}\phi\equiv \exists \mathbf{x}\neg \phi$
> 2. $\neg \exists \mathbf{x}\phi\equiv \forall \mathbf{x}\neg \phi$
> 3. $\forall \mathbf{x}\phi\equiv \neg \exists \mathbf{x}\neg \phi$
> 4. $\exists \mathbf{x}\phi\equiv \neg \forall \mathbf{x}\neg \phi$

> [!definition] Proposition
> For all variables $\mathbf{x}$ and formulas $\phi$ it holds:
> 1. $\forall\mathbf{x}(\phi\land \psi)\equiv\forall \mathbf{x}\phi \land\forall\mathbf{x}\psi$
> 2. $\exists \mathbf{x}(\phi\lor\psi)\equiv\exists \mathbf{x}\phi\lor\exists\mathbf{x}\psi$
> 
> For all variables $\mathbf{x}$ and formulas $\phi$ these formulas are **valid**:
> 1. $\forall \mathbf{x}(\phi\to\psi)\to(\forall\mathbf{x}\phi\to\forall\mathbf{x}\psi)$
> 1. $(\exists\mathbf{x}\phi\to\exists\mathbf{x}\psi)\to\exists \mathbf{x}(\phi\to\psi)$

> [!definition] Proposition
> For all variables $\mathbf{x},\mathbf{y}$ and formulas $\phi$ it holds:
> 1. $\forall \mathbf{x}\forall\mathbf{y}\phi\equiv\forall\mathbf{y}\forall\mathbf{x}\phi$
> 2. $\exists \mathbf{x}\exists\mathbf{y}\phi\equiv\exists\mathbf{y}\exists\mathbf{x}\phi$
> 3. $\forall \mathbf{x}\phi\equiv\phi\quad\text{if } \mathbf{x} \text{ not free in } \phi$
> 3. $\exists \mathbf{x}\phi\equiv\phi\quad\text{if } \mathbf{x} \text{ not free in } \phi$

> [!definition] Proposition
> For all variables $\mathbf{x}$ and formulas $\phi$ it holds:
> 1. $\exists \mathbf{x}\phi\equiv\exists \mathbf{z}\phi[\mathbf{z}/\mathbf{x}]\quad\text{if } \mathbf{z} \text{ not free in }\phi$
> 1. $\forall \mathbf{x}\phi\equiv\forall \mathbf{z}\phi[\mathbf{z}/\mathbf{x}]\quad\text{if } \mathbf{z} \text{ not free in }\phi$

Recall that $$\phi[t/\mathbf{x}]$$ is the result of replacing all free occurrences of $\mathbf{x}$ in $\phi$ by $t$ if **no capture of free variables happens**, otherwise *undefined*.


---
References:

