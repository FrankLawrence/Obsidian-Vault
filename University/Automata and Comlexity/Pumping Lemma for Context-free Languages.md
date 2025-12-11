---
Tags: 
Created: 2025-03-20 13:37:48
---
(Links:: [[Automata and Complexity]])
> [!info] THEOREM
> Let $L$ be a context-free language. There exists $m> 0$ such that for every word $w\in L$ with $|w| \geq m$: $$w=uvxyz$$ with $|vxy| \leq m$ and $|vy|\geq 1$, and $uv^{i}xy^{i}z\in L$ for every $i\geq 0$.

We take the word: $$a^{m}b^{m-1}c^{m-2}$$
Since $|vxy| \leq m, vy = a^{j}b^{k}$ or $vy = b^{k}c^{j}$ for some $j,k \geq 0$
Since $|vy| \geq 1$ we have $j + k \geq 1$
Since $|vxy| \leq m$, we have:
- If $|u| < m$, then $uv^{0}xy^{0}z = a^{m-j}b^{m-1-k}c^{m-2}$, but since $j + k \geq 1$ we either have there are number $a's \leq b's$ or number $b's \leq c's$
- If $|u| \geq m$, then $uv^{0}xy^{0}z = a^{m}b^{m-1-k}c^{m-2-j}$, but since $j + k \geq 1$ we have that number $b's \leq c's$

Thus L is not context-free

-0.5: as $m >= 0$, we cannot use $...c^{m-2}$, as this could be -1 c's.

In the second case, you cannot use $i=0$ here, as $j=0$ would mean that we only lose c's, and $m-1 > m-1-k$ still holds if $k>0$ (with $j=0$). Formally:
If $vy$ contains only c's, you must pump with multiplicity > 1. 
Values greater <= 1 do not work since deleting c's always gives a word in L again.



---
References: