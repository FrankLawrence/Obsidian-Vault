---
Tags: 
Created: 2022-12-20 16:44:41
---
(Links:: [[Logic and Sets]])
- Explain how an OBDD for $O_1 ? O_2$ can be constructed from $O_1$ and $O_2$
  This is basically done in the same way as the construction of an OBDD for $O_1 \lor O_2$. The only difference is if $O_1$ or $O_2$ is a leaf.
  Suppose $O_1$ is a leaf. If it is a 0 leaf return ... If it is a 1 leaf return ...
  Suppose $O_2$ is a leaf. If it is a 0 leaf return ... If it is a 1 leaf return ...
  Now suppose $O_1$ and $O_2$ are both not single leaves
  Case 1: associated to the same variable $x$
  Case 2: associated to different variables $x$ and $y$
  If $x$ is before $y$ then the corresponding branches are $O_1' \;operator\; O_2$ and $O_1'' \;operator\; O_2$
- Which of the two formulas entails the other?
    The ... formula entails the ... formula.  If $\phi$ hold on a model, it means there is an interpretation of the free variable $y$ in this model for which $\phi$ holds.
- Prove by mathematical induction that the statement holds true for all ... numbers $n$
  base case: $t_0 = \_$
  Induction step: Let $m \geq \, \_\;$ . Assume (Claim) that $...$ (IH)
  Hence (IH) implies $t_{m+1}=\; ...$
- Determine whether the relation $R$ is reflexive, symmetric and/or transitive
  ...
  It follow that $\langle a,b \rangle\; R\; \langle a,b \rangle$ holds, so $R$ is reflexive
- Determine whether or not it is injective and/or surjective
  The function $f(x)$ is injective, since $f(x) = f(z)$ -> $x=z$.
  It is surjective since for every ... number $y$, $f(x)=y$ will be true for $x = y ...$

---
References: