---
Tags: lecture
Created: 2022-12-05 16:37:24
---
(Links:: [[Logic and Sets]])
# Models
- The meaning (truth value) of a formula depends on the underlying model $M$
- $\forall x\phi$ holds in model $M$ if $\phi$ holds for every element x in $A$
	- properties are represented by subsets
	- relations are arrows between elements
	- constants are elements, quantifies range over the set
	- For a formula $\phi$ we denote by $M \vDash \phi$ that $\phi$ holds in $M$
- A tautology is true in all models
- A contradiction is false in all models
- A satisfiable formula is true in some model
# Semantic entailment
$\phi_1, ...,\phi_n \vDash \psi$ in predicate logic:
for all Models $M$:
$$M \vDash \phi_1 \text{ and ... and }M \vDash \phi_n \Rightarrow M \vDash \psi$$
# Semantic equivalence
$\phi_1 \equiv \psi$ in predicate logic:
for all Models $M$:
$$M \vDash \phi \Leftrightarrow M \vDash \psi$$
# $\alpha$-conversion
- $\exists xC(x)\equiv \exists yC(y)$
- $\forall xC(x)\equiv \forall yC(y)$
  $\Rightarrow \exists xC(x) \land \exists xD(x) \equiv \exists xC(x) \land \exists yD(y)$
# Important semantic equivalences
$$\lnot \exists x \,\phi \equiv \forall x  \,\lnot \phi$$
$$\lnot \forall x \,\phi \equiv \exists x  \,\lnot \phi$$
$$\forall x(\phi\land\psi) \equiv \forall x  \,\phi \land \forall x  \,\psi$$
$$\exists x(\phi\lor\psi) \equiv \exists x  \,\phi \lor \exists x  \,\psi$$
$$\exists x \exists y \,\phi \equiv \exists y \exists x \,\phi$$
$$\forall x \forall y \,\phi \equiv \forall y \forall x \,\phi$$

$$\exists x(C(x)\land D(x)) \not\equiv \exists x\,C(x)\land \exists x\,D(x)$$
$$\forall x(C(x)\lor D(x)) \not\equiv \forall x\,C(x)\lor \forall x\,D(x)$$
$$\forall x\exists y\, K(x,y) \not\equiv \exists y \forall x\, K(x,y)$$

---
References: