---
Tags: 
Created: 2022-11-17 09:02:34
---
(Links:: [[Logic and Sets]])
- A **partial order** on $V$ is a relation of type $V \times V$ that is reflexive ($xRx$) and transitive ($xRy\land yTz\to xTz$) and anti-symmetric ($(x\neq y\land xRy)\to \lnot yRx$, $xRy\land yRx\to x=y$)
  Ex: The relation $\leq$ is a partial order on $\Bbb N$
  Ex: The relation $\subseteq$ is a partial order on $\mathcal P(V)$
- The **power set** $\mathcal P(V)$
  Ex: $V:=\{1,2\}\Rightarrow \mathcal P(V)=\{\emptyset,\{1\},\{2\},\{1,2\}\}$
- $x,y\in V$ are **comparable** if $x\,R\,y\text{ or }y\,R\,x$
  $R$ is a **linear** (or **total**) **order** if all $x,y\in V$ are comparable
- A **strict partial order** $S$ corresponding to $R$ is defined by $$x\,S\,y \Longleftrightarrow x\, R\, y \text{ and } x\neq y$$ -> **irreflexive**

---
References: