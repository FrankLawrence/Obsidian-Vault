---
Tags: 
Created: 2023-07-01 00:25:26
---
(Links:: [[Linear Algebra]])
> [!definition] Vector space
> A **vector space** is a nonempty set $V$ of objects, called *vectors*, on which are defined two operations, called *addition* and *multiplication by scalars* (real numbers), subject to the ten axioms (or rules) listed below. The axioms must hold for all vectors $u$, $v$, and $w$ in $V$ and for all scalars $c$ and $d$.
> 1. The sum of $u$ and $v$, denoted by $u+v$, is in $V$.
> 2. $u+v=v+u$
> 3. $(u+v)+w=u+(v+w)$
> 4. There is a **zero** vector $0$ in $V$ such that $u+0=u$
> 5. For each $u$ in $V$, there is a vector $-u$ in $V$ such that $u+(-u)=0$
> 6. The scalar multiple of $u$ by $c$, denoted by $cu$, is in $V$
> 7. $c(u+v)=cu+cv$
> 8. $(c+d)u=cu+du$
> 9. $c(du)=(cd)u$
> 10. $1u=u$

# Subspaces
> [!definition] Subspace
> A **subspace** of a vector space $V$ is a subset $H$ of $V$ that has three properties:
> 1. The zero vector of $V$ is in $H$.
> 2. $H$ is closed under vector addition. That is, for each $u$ and $v$ in $H$, the sum $u+v$ is in $H$.
> 3. $H$ is closed under multiplication by scalars. That is, for each $u$ in $H$ and each scalar $c$, the vector $cu$ is in $H$.

- A plane in $\Bbb R^3$ *not* through the origin is not a subspace of $\Bbb R^3$, because the plane does not contain the zero vector of $\Bbb R^3$. Similarly, a line in $\Bbb R^2$ *not* through the origin, is *not* a subspace of $\Bbb R^2$
# A Subspace Spanned by a Set
> [!example] Proof that a span $H=\text{Span}\{v_1,v_2\}$ is a subspace of a vector space $V$

> [!info] THEOREM 1
> If $v_1,...,v_p$ are in a vector space $V$, then $\text{Span}\{v_1,v_2\}$ is a subspace of $V$.

> [!example] Proof that $H=\{(a-3b,b-a,a,b):a\text{ and }b \text{ in }\Bbb R\}$ is a subspace of $\Bbb R^4$

---
References: