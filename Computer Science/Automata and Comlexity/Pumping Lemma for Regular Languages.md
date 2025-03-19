---
Tags: 
Created: 2025-03-17 14:49:33
---
(Links:: [[Automata and Complexity]])

> [!info] THEOREM
> $$L=\{a^{n}b^{n} \,|\, n\geq 0\} \text{ is not regular}$$
> > [!info]- Proof
> > For a contradiction, assume that $L$ was regular.
> > Then there exists a DFA $M=\{Q,\{a,b\}, \delta, q_{0}, F\}$ with $L(M)=L$. 
> > Because $Q$ is finite, we have $$q_{0} \overset{a^{k}}{\twoheadrightarrow} q \overset{a^{l}}{\twoheadrightarrow} q$$ for some $l\geq 1$ and $q \in Q$. Then for some $q' \in Q$ $$q_{0} \overset{a^{k}}{\twoheadrightarrow} q \overset{b^{k}}{\twoheadrightarrow} q' \qquad q_{0} \overset{a^{k}}{\twoheadrightarrow} q \overset{a^{l}}{\twoheadrightarrow} q \overset{b^{k}}{\twoheadrightarrow} q'$$
> > Then $q'\in F$ since $a^{k}b^{k}\in L$. However, $q'\notin F$ since $a^{k+l}b^{k}\notin L$
> > Contradiction, thus $L$ is not regular.

Let $L$ be a regular language. There exists $m > 0$ such that every $w \in L$ with $|w|\geq m$ can be written in the form $$w=xyz$$ with $|xy|\leq m$ and $|y|\geq 1$, and $xy^{i}z\in L$ for every $i\geq 0$.
- The **pumping lemma** can be used to prove that a language is **not regular**

> [!example]-
> Assume that $L=\{w\in \{a,b\}^{* \,|\,}w = w^{R}\}$ is regular.
> By the pumping lemma there exists $m> 0$ such that $$a^{m}ba^{m}=xyz$$ with $|xy|\leq m$, $|y|\geq 1$, and $xy^{i}z\in L$ for every $i\geq 0$.
> Since $|xy|\leq m$ and $|y|\geq 1$, it follows that $$x=a^{j}\quad \text{and}\quad y=a^{k}$$ with $j\geq 0$ and $k\geq 1$.
> However $xyyz=a^{m+k}ba^{m}\notin L$. Contradiction!

A contradiction for specific values of $m$, $x$, $y$, or $z$ is not sufficient. Instead we have to prove the following: 
$$\begin{align}&\forall m> 0. \\ &\quad \exists w \in L \text{ with } |w|\geq m. \\ &\qquad \forall x,y,z \text{ with } w=xyz, |xy|\leq m, |y| \geq 1. \\ & \qquad \quad \exists i\geq 0.xy^{i}z \notin L\end{align}$$

For this we play the following game. Given is a language $L$. We want to prove that $L$ is not regular. 
1. Opponent picks $m$
2. We choose a word $w\in L$ with $|w|\geq m$.
3. Opponent picks $x,y,z$ with $w = xyz, |xy| \leq m$ and $|y| \geq 1$
4. If we can find $i \geq 0$ such that $xy^{i}z\notin L$, then **we win**

If we can always win, $L$ does not have the pumping property!

---
References: