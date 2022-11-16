---
Tags: 
Created: 2022-11-08 13:49:08
---
(Links:: [[Logic and Sets]])

- Ignore the rows in the truth-table where the values are false (left-hand side always evaluates to false)
- When the left-hand-side is true but the right-hand-side is false, the entailment doesn't hold
- The consequence is always true, or the atiscident is false
```ad-important
title: Deduction Theorem
collapse: none
For all formulas $\phi_1 ,\dots ,\phi_n \; \text{and} \; \psi:$
$$\phi_1 ,\dots ,\phi_n \vDash \psi \Longleftrightarrow \phi_1,\dots,\phi_{n-1} \vDash \phi_n \to \psi$$
```

---
References: