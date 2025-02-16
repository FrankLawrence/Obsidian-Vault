---
Tags: Math
Created: 2024-09-12 14:54:46
---
(Links:: [[Equational Programming]])
# Beta Reduction
> [!example]-
> $$I\; a \to _{\beta}a$$
> $$(I\; a)\;b \to_{\beta}a\; b$$
> $$b\;(I\; a) \to_{\beta}b\; a$$
> $$\lambda y.I\; a \to_{\beta}\lambda y.a$$

In general we do *not* have: $$M[x:=N][y:=P]=M[y:=P][x:=N]$$

> [!definition] Beta-redex
> A $\beta$-redex is an instance of the left-hand side of the $\beta$-reduction rule, that is: sub-term of the form $(\lambda x.M)\,N$
> ```mermaid
> flowchart TD
> 	subgraph beta-redex
> 		a[@] --> b
> 	end
> 	b[λx] --> c[M]
> 	a --> d[N]
> ```
> A term can contain zero, one, or more redexes
> > [!example]
> > 1. $x((\lambda y.y)u)$ contains one $\beta$-redex
> > 2. $x(\lambda y.y)u$ contains no $\beta$-redexes
> > 3. $\Omega$ contains one $\beta$-redex
> > 4. $(I\;a)\;(I\;a)\;(I\;a)$ contains three $\beta$-redexes
>  
>  - A term containing *one or more* $\beta$-redexes can be $\beta$-reduced
>  - A term containing *two or more* $\beta$-redexes can be $\beta$-reduced in different ways

> [!example] Beta-reduction with creation of redexes
> $$(\lambda x.x)(\lambda y.y\,z)\,a\to _{\beta}{\color{red}(\lambda y.y\,z)\,a}\to_{\beta}a\,z$$
> $$(\lambda x.\lambda y.f\,x\,y)\,a\,b\to _{\beta}{\color{red}(\lambda y.f\,a\,y)\,b}\to_{\beta}f\,a\,b$$
> $$(\lambda x.x\,a)(\lambda y.y\,b)\to _{\beta}{\color{red}(\lambda y.y\,b)\,a}\to_{\beta}a\,b$$

## Beta-reduction: Lemma
If $$M\to _{\beta}^{*}M'$$, then $$\lambda x.M\to_{\beta}^{*}\lambda x.M'$$, and also $$M\; N\to_{\beta}^{*}M'\; N$$, and also $$N\; M\to_{\beta}^{*}N\; M'$$
This can be shown by induction on the length of $$M\to_{\beta}^{*}M'$$
If $M\to_{\beta}M'$, then $FV(M)\subseteq FV(M')$

## Beta-normal form
> [!definition]
> - A normal form is a result of a computation
> - A $\lambda$-term is a $\beta$-normal form if it does not contain a $\beta$-redex so it cannot do a $\beta$-reduction step
> - $M$ is a $\beta$-normal form iff
> 	- $M=\lambda x.M_0$ with $M_0$ a $\beta$-normal form, or
> 	- $M=x\,M_{1}...M_{n}$ with $n\geq0$  and $M_{1},...,M_{n}$ $\beta$-normal forms

> [!example]- Which terms are in $\beta$-normal form?
> > [!question]- $u(\lambda x.x\,x)\,v$
> > Yes
> 
> > [!question]- $\lambda x.(\lambda y.y)\,v$
> > No, $(\lambda y.y)v$
> 
> > [!question]- $(\lambda x.x)(\lambda y.y)\,x$
> > No, $(\lambda x.x)(\lambda y.y)$
> 
> > [!question]- $\lambda x.x((\lambda y.y)x)$
> > No, $(\lambda y.y)\,x$
> 
> > [!question]- $\lambda x.x(\lambda y.y)$
> > Yes
## Beta-equality
Besides $\beta$-reduction we also consider $\beta$-equality $M=_{\beta}N$. Either start from the $\beta$-equality axiom, and use reflexivity, symmetry, transitivity, compatibility with term formation, *or* start from the $\beta$-reduction relation, and take its reflexiv-transitiv-symmetric closure, that is: take as many $\beta$-reduction steps as you want, in either direction.
This gives the same! $$=_{\beta}\;\Large{=}\;\leftrightarrow_{\beta}^{*}$$
Note: $\to _{\beta}\;\large{\subset}\;\to_{\beta}^{*}\;\large{\subset}\;=_{\beta}$

> [!question] Give $\lambda$-terms $M$ and $N$ such that $M$ and $N$ are $\beta$-normal forms, but $M[x:=N]$ is not a $\beta$-normal form.

> [!question] Give $\lambda$-terms $M$ and $N$ such that $M$ and $N$ both have a $\beta$-normal form, but $M[x:=N]$ does not have a $\beta$-normal form.
# Fixed point combinators
> [!definition] Fixed point
> $x\in A$ is a fixed point of $f\;:\;A\to B$ if $f(x)=x$

> [!example] 0 and 1 are fixed points of $f\;:\Bbb{R}\to \Bbb{R}$ with $x\mapsto x^{2}$
- In $\lambda$-calculus $M$ is a fixed point of $F$ if $F\; M=_{\beta}M$
- Every term $M$ is a fixed point of $I$
- **Every term $M$ has a fixed point!**

Take a term $F$ and consider $W=\lambda x.F(x\,x)$. We have: $$W\;W=(\lambda x.F(x\,x))W\to_{\beta}F\;(W\;W)=_{\beta}W\;W$$

> [!definition] Fixed point combinator
> $Y$ is a fixed point combinator if $F\;(Y\;F)=_{\beta}Y\;F$ for every $\lambda$-term $F$. This means we can use $Y$ to construct a fixed point for a given term $F$.

- **Curry's fixed point combinator**: $$Y=\lambda f.(\lambda x.f\;(x\;x))\;(\lambda x.f\;(x\;x))$$
- **Turing's fixed point combinator**: $$T=(\lambda x.\lambda y.y\;(x\;x\;y))\;(\lambda x.\lambda y.y\;(x\;x\;y))$$

> [!example]- Curry's fpc
> for an arbitrary $F$ we have: 
> $$\begin{align}
> Y\;F\;\;&=\quad (\lambda {\color{red}f}.(\lambda x.f\,(x\,x))\,(\lambda x.f\,(x\,x))) F \\
> &\to_{\beta}\; (\lambda {\color{red}x}.F\,(x\,x))\,(\lambda x.F\,(x\,x)) \\
> &\to_{\beta}\; F\,{\color{green}((\lambda x.F\,(x\,x))\,(\lambda x.F\,(x\,x)))} \\
> &\leftarrow \;\;\, F((\lambda {\color{red}f}.(\lambda x.f\,(x\,x))\,(\lambda x.f\,(x\,x)))F) \\
> &=\;\;\; F\,(Y\,F)
> \end{align}$$
> and also: $$F\,((\lambda x.F\,(x\,x))\,(\lambda x.F\,(x\,x))) \to_{\beta}F\,(F\,((\lambda x.F\,(x\,x))\,(\lambda x.F\,(x\,x))))$$
> and: $$Y\;F=_{\beta}F\;(Y\;F)=_{\beta}F\;(F\;(Y\;F))=_{\beta}\cdots$$

- we can use a fixed point operator to solve an equation $x=f(x)$

> [!example] Hindley
> > [!question] Define $X$ such that $X\; y=_{\beta}X$ (a garbage disposer)
> > $$\begin{align}
> > X\;y&=_{\beta}X \\
> > \leftarrow X &=_{\beta}\lambda y.X \\
> > \leftarrow X&=_{\beta}(\lambda x.\lambda y.x)\,X \\
> > \leftarrow X&=_{\beta}Y\,(\lambda x.\lambda y.x) \\
> > \to X&= Y\,(\lambda x.\lambda y.x)
> > \end{align}$$

---
References: