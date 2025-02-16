---
Tags: Math
Created: 2024-09-16 03:26:28
---
(Links:: [[Equational Programming]])
# Weak and strong Normalization
> [!definition] Termination
> $M$ is **terminating** or **strongly normalizing (SN)** if all $\beta$-reduction sequences starting in $M$ are finite.
> 
> $M$ is **weakly normalizing (WN)** if *there exists* a $\beta$-reduction sequence starting in $M$ that ends in a $\beta$-normal form or: $M$ has a normal form.
> 
> Termination and weak normalization are *undecidable* 
> $M$ is a normal form implies $M$ has a normal form but not vice versa

> [!example]
> $$(\lambda x.x\,x)(\lambda f.\lambda y.(f\,y))\;\text{ is SN}$$ 
> $$(\lambda x.x\,\Omega)\;(\lambda y.z)\;\text{ is WN but not SN}$$
> $$(\lambda x.x\Omega)\;(\lambda y.y)\;\text{ is not WN}$$
> $$(\lambda x.x\,x\,x)\;(\lambda x.x\,x\,x\,x)\;\text{ is not WN}$$
> $$x \text{ is SN and is in normal form}$$
> $$(\lambda x.x)\,y\;\text{ is WN but not in normal form}$$
> $$(\lambda x.y)\,\Omega\;\text{ is WN but not terminating}$$
> $$\Omega \text{ is not WN (and hence not SN)}$$

- if $M$ is a NF (ex. $M=a$) then $M$ is $SN$
- if $M$ is SN (ex. $M=I\;a$) then $M$ is $WN$
- if $M$ is WN (ex. $M=(\lambda x.y)\Omega$) then $M$ is a term

## Reduction graph of a $\lambda$-term
- terms are the vertices and (all) the reduction steps are the edges
- a reduction graph may be finite and cycle-free, eg. $I\;x$
- a reduction graph may be finite with cycles, eg. $\Omega$
- a reduction graph may be infinite, eg. $(\lambda x.x\,x\,x)\;(\lambda x.x\,x\,x)$
- a reduction graph is not necessarily simple, eg. $I\;(I\;I)$
# Reduction Strategies
- **Outermost Redex**: A redex occurrence is outermost if it is not contained in another redex; all outermost redexes of a term are parallel (no redex is contained in another redex)
- **Innermost Redex**: A redex occurrence is innermost if it does not contain another redex; all innermost redexes of a term are parallel
- A set of parallel redexes has a unique leftmost redex which is the leftmost one in the term tree

> [!info] Redex properties
> # Outermost
> Every term not in NF has an outermost redex, and it has a unique leftmost-outermost redex
> # Innermost
> Every term not in NF has an innermost redex, and it has a unique leftmost-innermost redex
> # Lazy redex
> Every term not in WHNF has a lazy redex. Lazy redex is unique.
## Call by value / leftmost-innermost
With call by value you reduce in every step the leftmost-innermost redex. This has the effect that 
- terms are **not** normalizing: $$(\lambda x.y)\;{\color{red}\Omega}\to_{\beta}(\lambda x.y)\;{\color{red}\Omega}\to_{\beta}\cdots$$
- redexes are not [copied]: $$(\lambda x.f\,x\,x)({\color{red}((\lambda x.x)\,a)})\to_{\beta}{\color{red}(\lambda x.f\,x\,x)\,a}\to_{\beta}f\,a\,a$$
- it may contract redexes that are not needed: $$(\lambda x.y)\,({\color{red}I\,z})\to_{\beta}(\lambda x.y)\,z\to_{\beta}y$$
## Call by need / leftmost-outermost
We reduce in every step the leftmost-outermost redex. Unlike call by value, it is normalizing. Ex: $${\color{red}(\lambda x.f\,x\,x)\,((\lambda x.x)\,3)}\to_{\beta}f({\color{red}(\lambda x.x)\,3})\,((\lambda x.x)\,3)\to_{\beta}f\,3\,({\color{red}(\lambda x.x)\,3})\to_{\beta}f\,3\,3$$
## Lazy Reduction && WHNF
Haskell is a lazy functional programming language which evaluates only at the very last moment. Programs are terms and evaluate with leftmost-outermost reduction (with sharing) and the result is a weak head normal form.

If we try reducing a term in normal form (like $x$) then the reduced term is itself. A term $\lambda x.M$ will reduce to the form $\lambda x.M'$ where $M\to_{\beta}^{*}M'$. The same can be said for $x\;M$: it reduces to a term of the form $x\;M'$ where $M\to_{\beta}^{*}M'$

With the term $M\;N$, where $M$ and $N$ are $\lambda$-terms, we don't know what it would reduce to.

> [!definition] Weak Head Normal Form
> At least one symbol is stable under reduction.
> - $\lambda x.P$ with $P$ an arbitrary $\lambda$-term
> - $x\,P_{1}...P_{n}$ with $n\geq0$ and $P_{1},...,P_{n}$ arbitrary $\lambda$-terms

# Data types in the lambda calculus
## Booleans
- Definition of **true**: $$\lambda x.\lambda y.x$$
- Definition of **false**: $$\lambda x.\lambda y.y$$
- Definition of **not**: $$\lambda x.x \text{ false true}$$
- Definition of **ite**: $$\lambda b.\lambda x.\lambda y.b\,x\,y$$
- Definition of **and**: $$\lambda x.\lambda y.x\, y\text{ false}$$
- Definition of **or**: $$\lambda x.\lambda y.x\text{ true }y$$
## Encoding natural numbers: Church Numerals
$$\begin{align}
c_{0}&= \lambda s.\lambda z.z \\
c_{{\color{red}1}}&= \lambda s.\lambda z.{\color{red}s}\,z \\
c_{{\color{red}2}}&= \lambda s.\lambda z.{\color{red}s}\,({\color{red}s}\,z) \\
c_{{\color{red}3}}&= \lambda s.\lambda z.{\color{red}s}\,({\color{red}s}\,({\color{red}s}\,z)) \\
c_{{\color{red}4}}&= \lambda s.\lambda z.{\color{red}s}\,({\color{red}s}\,({\color{red}s}\,({\color{red}s}\,z))) \\
&\;\;\vdots \\
c_{n}&= \lambda s.\lambda z.s^{n}(z) \\
\end{align}$$
### Successor
We aim to have: $$S\; c_{n}=_{\beta} \;c_{n+1}$$
$$\begin{align}
c_{3}\;s\;z &= (\lambda s.\lambda z.s\,(s\,(s\,z)))\, s\,z \\ 
&=_{\beta} s\,(s\,(s\,z)) \\ \\
s\,(c_{3}\,s\,z) &=_{\beta}\,s\,(s\,(s\,(s\,z)))\\ \\
\lambda s.\lambda z.s\,(c_{3}\,s\,z) &=_{\beta}\lambda s.\lambda z.\,s\,(s\,(s\,(s\,z)))\\ 
&=_{\beta}c_{4}\\
\end{align}$$
Thus we get: $$(\lambda {\color {red}x}.\lambda s.\lambda z.s({\color{red}x}\,s\,z))\,{\color{red}c_{3}}=_{\beta}\,c_{4}$$
$$S=(\lambda x.\lambda s.\lambda z.s\,(x\,s\,z))$$
$$S=(\lambda x.\lambda s.\lambda z.x\,s\,(s\,z))$$
### iszero
We aim to have $\text{iszero }c_{0}=\text{ true}$
If we compute $c_{0}\,A\,\text{true}$ then we get $$c_{0}\,A\,\text{true}=(\lambda s.\lambda z.z)\,A\,\text{true}=_{\beta}\text{true}$$
Computing $c_{1}\,A\,\text{true}$ we get $$c_{1}\,A\,\text{true}=(\lambda s.\lambda z.s\,z)\,A\,\text{true}=_{\beta}A\,\text{true}$$
With this in mind, we can choose $A$ such that it takes true as the argument and returns false: $A=\lambda y.\text{false}$
Inserting it into the previous term we have $$c_{1}\,(\lambda y.\text{false})\,\text{true}=_{\beta}\,(\lambda y.\text{false})\,\text{true}=_{\beta}\text{false}$$
Thus we get: $$(\lambda x.x\,(\lambda y.\text{false})\,\text{true})\,c_{0}=_{\beta}\text{true}$$
$$(\lambda x.x\,(\lambda y.\text{false})\,\text{true})\,c_{1}=_{\beta}\text{false}$$
$$\text{iszero}:=\lambda n.n\,(\lambda y.\text{false})\,\text{true}$$
### Other Operations on natural numbers
- **Addition**: $$\text{plus}:=\lambda m.\lambda n.\lambda s.\lambda z.m\,s\,(n\,s\,z)$$
- **Multiplication**: $$\text{mul}:=\lambda m.\lambda n.\lambda s.\lambda z.m\,(n\,s)\,z$$
- **Exponentiation**: $$\text{exp}:=\lambda m.\lambda n.n\,m$$
### Definability
A function $f:\Bbb{N}\to\Bbb{N}$ is **definable in the $\lambda$-calculus by a term $F$** if $$F[n]=_{\beta}[f(n)]$$ for every $n\in \Bbb{N}$. All functions $f:\Bbb{N}\to\Bbb{N}$ that are computable are also $\lambda$-definable. Take as an example the Church Numerals: $F\,c_{n}=_{\beta}\,c_{f(n)}$
## Pairs and Lists
> [!definition] Pairing Operator
> $$\pi :=\lambda l.\lambda r.\lambda z.z\,l\,r$$
> Example: $$\pi \,P\,Q=_{\beta}\lambda z.z\,P\,Q$$
> # Projections
> $$\pi_{1}:=\lambda u.u\,(\lambda l.\lambda r.l)=\lambda u.u\,\text{true}$$
> $$\pi_{2}:=\lambda u.u\,(\lambda l.\lambda r.r)=\lambda u.u\,\text{false}$$
> then:
> $$\pi_{1}\,(\pi\,P\,Q)=_{\beta}P$$
> $$\pi_{2}\,(\pi\,P\,Q)=_{\beta}Q$$

A list is obtained by repeatedly forming a pair.
> [!definition] Encoding of lists
> $$\text{nil}:= \lambda xy.y$$
> $$\text{cons}:=\lambda h.\lambda t.\lambda z.z\,h\,t=\pi$$
> $$\text{head}:=\lambda l.l\,(\lambda ht.h)=\pi_{1}$$
> $$\text{tail}:=\lambda l.l(\lambda ht.t)=\pi_{2}$$
> $$\text{head (cons H T)}=_{\beta}H$$
> $$\text{tail (cons H T)}=_{\beta}T$$

> [!example]
> - $\text{cons } {\color{orange}c_{1}}(\text{cons } {\color{green}c_{2}}(\text{cons } {\color{blue}c_{3}}{\color{red}\text{ nil}}))=_{\beta}$ $$\lambda z.z\,{\color{orange}(\lambda s.\lambda z.s\,z)}\,(\lambda z.z\,{\color{green}(\lambda s.\lambda z.s\,(s\,z))}\,(\lambda z.z\,{\color{blue}(\lambda s.\lambda z.s\,(s\,(s\,z)))}\,{\color{red}(\lambda x.\lambda y.y)}))$$

> [!example] Calculating the head
> $\text{cons } H\,T = ((\lambda h.\lambda t.\lambda z.z\,h\,t)\,H)\,T=_{\beta}(\lambda t.\lambda z.z\,H\,t)\,T=_{\beta}\lambda z.z\,H\,T$
> $$\begin{align}
> \text{head } (\text{cons }H\,T) &=_{\beta} \\
> \text{head } (\lambda z.z\,H\,T) &=_{\beta} \\
> (\lambda l.l\,(\lambda h.\lambda t.h))\, (\lambda z.z\,H\,T) &=_{\beta} \\
> (\lambda z.z\,H\,T)\,(\lambda h.\lambda t.h) &=_{\beta} \\
> (\lambda h.\lambda t.h)\,H\,T &=_{\beta} \\
> (\lambda t.H)\,T &=_{\beta} \\
> H &=_{\beta} \\
> \end{align}$$

> [!definition] isempty
> $$\text{isempty}:=\lambda l.l\,(\lambda x.\lambda y.\lambda z.\text{false})\,\text{true}$$

---
References: