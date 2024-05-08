---
Tags: 
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

# Satisfiability, Validity, and Consistency
# Translation into Predicate Logic and the Interplay of Quantifiers
# Interplay between Quantifiers and Connectives


---
References:

