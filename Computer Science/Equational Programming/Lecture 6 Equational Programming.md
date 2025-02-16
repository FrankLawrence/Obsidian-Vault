---
Tags: Math
Created: 2024-09-22 20:28:30
---
(Links:: [[Equational Programming]])
# Russell's paradox
In set theory we have $$\text{define }R:=\{X|X\notin X\}$$
$$\text{then }R\in R\Leftrightarrow R\notin R$$
In lambda calculus we have $$\text{define }R:=\lambda x.\text{not}\,(x\,x)$$
$$\text{then }R\,R=_{\beta}\text{not}\,(R\,R)$$
# Haskell is strongly typed
In untyped $\lambda$-calculus there is no restriction on forming applications for example $\lambda x.x\,x$ is a term with self-application. In Haskell for example $\text{head true}$ is not accepted. Similarly, in simple typed $\lambda$-calculus, if $$u\; : \text{natlist} \to \text{nat}\; \text{ and }\;v :\text{bool}$$ then we cannot apply $u$ to $v$. We also **cannot apply $x$ to $x$** (important for later). We can only apply a 'function' $F$ to an 'argument' $P$ if $$F:A\to B\quad\text{ and }\quad P:A$$
## Terminology
- Type declaration for a variable: $x:A$
- Environment (or context): set of declarations for different variables $$\Gamma = \{x_{1}:A_{1},...,x_{n}:A_{n}\}\quad\text{also written}\quad x_{1}:A_{1},...,x_{n}:A_{n}$$
- Typing statement: $$\Gamma \vdash M:A$$
  In environment $\Gamma$ the term $M$ has type $A$, then we say that $M$ is typable, and that $M$ is an inhabitant of $A$
# Typing System to derive typing statements
> [!definition] Variable Rule
> $$\Gamma, x:A\vdash x:A$$

> [!definition] Abstraction Rule
> $$\frac{\Gamma,x:A\vdash M:B}{\Gamma \vdash(\lambda x.M):A\to B}$$

> [!definition] Application Rule
> $$\frac{\Gamma\vdash F:A\to B\qquad\Gamma\vdash M:A}{\Gamma \vdash(F\,M): B}$$

> [!example] K
> $$\cfrac{x:A,y:B\vdash x:A}{\cfrac{x:A\vdash \lambda y.x:B\to A}{\vdash \lambda x.\lambda y.x:A\to B\to A}}$$
> K is typable in the empty environment with type $A\to B\to A$
# Termination
- **Every typable term is terminating**
- There are terminating terms that are not typable: $\lambda x.x\,x$
- It is decidable whether a term is typable
- It is not decidable whether a term is terminating


---
References: