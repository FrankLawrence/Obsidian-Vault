---
Tags: 
Created: 2024-05-27 21:12:34
---
(Links:: [[Logic and Modelling]])
# Metatheorems of Predicate Logic
> [!info] $\vdash$ and $\vDash$ for arbitrary sets $\Gamma$ of formulas
> - $\Gamma \vdash \psi$: there is a natural deduction derivation of $\psi$ that uses only premises in $\Gamma$ 
> - $\Gamma \vDash \psi$: for all $\mathcal{M}$ and $\ell$: if $\mathcal{M}\vDash_{\ell}\phi$ for every $\phi \in \Gamma$ , then $\mathcal{M}\vDash\psi$
## Soundness and Completeness Theorem
The syntactic version $\vdash$ and the semantic version $\vDash$ of logical entailment **coincide**: $$\Gamma\vdash \phi\quad\Longleftrightarrow\quad\Gamma\vDash\phi$$ holds for all formulas $\phi$, and every set $\Gamma$ of formulas.
Two directions: $$\begin{align}
& \Longrightarrow\;:\text{ soundness (correctness)} \\
& \Longleftarrow\;:\text{ completeness}
\end{align}$$
In a symbolic abbreviation: $$\vdash \quad = \quad\vDash$$
### Soundness Theorem
> [!definition] Theorem (Soundness of $\vdash$ with respect to $\vDash$)
> For all formulas $\phi$, and every set $\Gamma$ of formulas: $$\Gamma\vdash\phi\quad\Longrightarrow\quad\Gamma\vDash\phi$$

Reformulation and explanation: If there is a natural deduction derivation of $\phi$ from $\Gamma$, then there is no model $\mathcal{M}$ in which all formulas of $\Gamma$ are true, but $\phi$ is false. The derivation rules are **correct** in this sense: It is *not possible* to derive a *false* conclusion $\phi$ from *true* premises $\Gamma$. Truth in a model is preserved under making deductions.

Proof of the theorem: by induction on derivation lengths.
### Completeness Theorem
> [!definition] Theorem (Completeness of $\vdash$ with respect to $\vDash$)
> For all formulas $\phi$, and every set $\Gamma$ of formulas: $$\Gamma\vDash\phi\quad\Longrightarrow\quad\Gamma\vdash\phi$$

Reformulation and explanation: The derivation rules are **strong enough** to derive **all valid semantic entailment statements**.
Thus **more** derivation rules are **not necessary**, and in this sense these rules are **complete**.

Proof of the theorem: nontrivial
## Consistency and Syntactic Consistency
We [[Semantics of, and Translations into, Predicate Logic#Satisfiability, Validity, and Consistency|already know]] a **semantic** notion of consistency: A set $\Gamma$ of formulas is consistent if there is a model $\mathcal{M}$ and an environment $\ell$ such that $\mathcal{M}\vDash_{\ell}\phi$ for all $\phi\in\Gamma$. There is also a **syntactic** variant: 
A set $\Gamma$ of formulas is syntactically consistent if: $$\Gamma\nvdash\bot$$ (That is, there is no derivation of $\bot$ from $\Gamma$.)

> [!definition] Theorem (Consistency)
> For every set $\Gamma$ of formulas it holds: $$\Gamma \text{ is consistent }\quad\Longleftrightarrow\quad\Gamma \text{ is syntactically consistent}$$

# Definability and Undefinability Results
# Definability and Undefinability Results for Model Cardinality
# Definability and Undefinability Results for Reachability

---
References: