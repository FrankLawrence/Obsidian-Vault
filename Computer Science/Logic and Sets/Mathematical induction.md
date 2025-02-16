---
Tags: Math
Created: 2022-12-16 13:47:46
---
(Links:: [[Logic and Sets]])
- Mathematical induction is a strategy of proving claims of the form $$S_n \text{ is true for every integer } n\geq a$$
- **Strategy:**
	- **base case:** prove $S_a$ is true
	- **inductive step:** prove $S_m \to S_{m+1}$ is true for any $m\geq a$
Example: *Claim* for all $n\in N, \; \sum_{k=0}^n 2^k=2^{n+1}-1$
*base case:*
- $\sum_{k=0}^0 2^k=2^0=1$
- $2^{0+1}-1=2^1-1=2-1=1$
  -> true for $n=0$
*induction step:* Let $m\geq 0$. Assume $\sum_{k=0}^m 2^k=2^{m+1}-1$ *IH*
$\sum_{k=0}^{m+1} 2^k=\sum_{k=0}^{m+1} 2^k+2^{m+1}=2^{m+1}-1+2^{m+1}$
$2^{(m+1)+1}-1=2\cdot 2^{m+1}-1=2^{m+1}-1+2^{m-1}$
Hence *IH* implies $\sum_{k=0}^{m+1}2^k=2^{(m+1)+1}-1$
## Well-ordering principle
- Every non-empty subset $A$ of $Z$ that has a lower bound has a smallest element (in the usual linear order on $Z$)
- In $Z$, well-ordering implies mathematial induction
- In $Z$, mathematical induction implies well-ordering
## Sequences
- A total function $f:N\to A$ defines the sequence: $$(f(n))_{n=0}^{\infty}=(f(0),f(1),f(2),...$$
  Example: factorials
  $(t_n)_{n=0}^{\infty}$ defined by $t_0:=1,t_{n+1}:=(n+1)\cdot t_n$
  Sequence $(1,1,2\cdot 1,3\cdot 2\cdot 1,...)$ so $t_n=n!$
## Transitive closure
$x \; R^n \; y \quad \Leftrightarrow$ there is a path of exactly n steps from x to y in the directed graph representation of R
- the transitive closure of a binary relation R in a set $A$ is the relation
  $$R^*:=\{\langle x,y\rangle :x\;R^n\;y\text{ for some }n\geq 1\}$$

---
References: