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
> For every set $\Gamma$ of formulas it holds: $$\Gamma \text{ is consistent }\quad\Longleftrightarrow\quad\Gamma \text{ is syntactically consistent}$$ or, put differently: $$\Gamma \text{ has a model }\quad\Longleftrightarrow\quad\text{ there is no derivation of $\bot$ from } \Gamma$$
> > [!info]- Proof of $\Rightarrow$
> > 1. Let $\mathcal{M}$ and $\ell$ such that $\mathcal{M}\vDash_{\ell}\Gamma$
> > 2. Note that $\mathcal{M}\nvDash_{\ell}\bot$
> > 3. We conclude $\Gamma\nvDash\bot$
> > 4. Then by the soundness theorem: $\Gamma\nvdash\bot$
> 
> > [!info]- Proof of $\Leftarrow$
> > 1. Suppose that $\Gamma\nvdash\bot$
> > 2. Then by the completeness theorem: $\Gamma\nvDash\bot$
> > 3. Hence there are $\mathcal{M}$ and $\ell$ such that $\mathcal{M}\vDash_{\ell}\Gamma$ and $\mathcal{M}\nvDash_{\ell}\bot$
> > 4. Hence $\Gamma$ is consistent

## Compactness Theorem
> [!definition] Theorem (Compactness)
> For every set $\Gamma$ of formulas it holds: $$\Gamma \text{ is consistent}\quad\Longleftrightarrow\quad\text{ every finite subset }\Gamma_{0}\subset\Gamma\text{ is consistent}$$
> > [!info]- Proof of $\Rightarrow$
> > Every model for $\Gamma$ also is a model for all subsets $\Gamma_{0}\subset\Gamma$ 
> 
> > [!info]- Proof of $\Leftarrow$ (by [[Transposition]])
> > 1. Suppose that $\Gamma$ is **not** consistent. Then it follows $\Gamma\vDash\bot$
> > 2. Then by the completeness theorem: $\Gamma\vdash\bot$
> > 3. Hence there is a derivatino $\mathcal{D}$ of $\bot$ from premises in $\Gamma$ 
> > 4. As $\mathcal{D}$ is finite, it can only use finitely many premises $\Gamma_{0}\subset\Gamma$
> > 5. Then $\mathcal{D}$ also shows $\Gamma_{0}\vdash\bot$
> > 6. By the soundness theorem it follows: $\Gamma_{0}\vDash\bot$ 
> > 7. Hence the finite subset $\Gamma_{0}$ of $\Gamma$ is **not** consistent (Contradicts the right side!!)
# Definability and Undefinability Results
## Definability and Undefinability Results for Model Cardinality
Define $\phi_{n}$ for $n\in\Bbb{N}$ with $n\geq 2$: $$\phi_{n}=\exists x_{1}...\exists x_{n}\bigwedge_{1\leq i<j\leq n}x_{i}\neq x_{j}$$
Define $\psi_{n}$ for $n\in \Bbb{N}$ with $n\geq 1$: $$\psi_{n}=\forall x_{1}...\forall x_{n+1}\bigvee_{1\leq i<j\leq n}x_{i}=x_{j}$$

> [!definition] Proposition
> For all models $\mathcal{M}$ and all $n\geq 2$ it holds: 
> 1. $\mathcal{M}\vDash\phi_{n} \quad \Longleftrightarrow \quad A$ has at least $n$ elements (i.e. $\mid A\mid \geq n$)
> 2. $\mathcal{M}\vDash\psi_{n} \quad \Longleftrightarrow \quad A$ has at most $n$ elements (i.e. $\mid A\mid \leq n$)
> 3. $\mathcal{M}\vDash\phi_{n}\land \psi_{n} \quad \Longleftrightarrow \quad A$ has precisely $n$ elements (i.e. $\mid A\mid = n$)
## Definability and Undefinability Results for Reachability

---
References: