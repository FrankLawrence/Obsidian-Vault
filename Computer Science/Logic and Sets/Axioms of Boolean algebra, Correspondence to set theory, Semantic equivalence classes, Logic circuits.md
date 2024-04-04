---
Tags: lecture
Created: 2022-11-28 16:12:03
---
(Links:: [[Logic and Sets]])
# Axioms
The axioms of Boolean algebra are:
- **Sound** for propositional logic.
  Its axioms equate only semantically equivalent formulas
- **Complete** for propositional logic.
  All sound equations between formulas can be derived
- **Irredundant**
  No axiom can be derived from the other axioms
## Correspondence with algebra of sets

| *propositional logic* | *algebra of sets* |
| --------------------- | ----------------- |
| $\bot$                | $\emptyset$       |
| $\top$                | $\bigcup$         |
| $\lor$                | $\cup$            |
| $\land$               | $\cap$            |
| $\lnot$               | $'$               |

Propositional logic coincides with set theory with a universe of a single element.
# Equivalence relation
A binary relation *R* is an **equivalence relation** if, for all elements *x*,*y*,*z* in its domain, *R* satisfies the following three properties:
- Reflexive
- Symmetric
- Transitive
-> $\equiv$ is an equivalence relation

Within an equivalence class, all formulas are semantically equivalent / all elements are related by *R*.
Elements in *different* equivalence classes are not related by *R*
Each equivalence class contains *infinitely* many formulas.
- the number of equivalence classes is proportional to the amount of variables: 
	- for `n` variables there are $2^n$ valuations
	- Each valuation has 2 possible outcomes (*T* or *F*)
	- for `n` variables there are $2^{2^n}$ permutations / equivalence classes
# Logic circuits
**Binary Addition of two bits**
- the left bit equals to $x \land y$ that results when adding bits $x$ and $y$
- the right bit equals $x \oplus y$ that results when adding bits $x$ and $y$
- $x_0 + y_0 = s_1s_0$
	- $s_0 = x_0 \oplus y_0$
	- $c_1 = x_0 \land y_0 =s_1$

**Binary Addition of two pairs of bits**
- $x_1x_0 + y_1y_0 = s_2s_1s_0$
	- $s_0 = x_0 \oplus y_0$
	- $c_1 = x_0 \land y_0$
	- $s_1=x_1\oplus y_1 \oplus c_1$
	- $c_2 = (x_1\land y_1) \lor (c_1 \land (x_1\oplus y_1))$
	- $s_2 = c_2$

**Addition of two bit strings**
- $c_0 = 0$
- for $i= 0,\dots,b-1$:
  $s_i = x_i\oplus y_i \oplus c_i$
  $c_{i+1}=(x_i\land y_i) \lor (c_i\land(x_i\oplus y_i))$
- Finally, $s_b=c_b$

---
References: