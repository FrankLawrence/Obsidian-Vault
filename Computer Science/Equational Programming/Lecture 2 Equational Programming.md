---
Tags: 
Created: 2024-09-06 16:05:43
---
(Links:: [[Equational Programming]])
# Lambda Calculus in a nutshell
$\lambda$-terms:
- variable $x$
- constant $c$
- abstraction $(\lambda x.M)$
- application $(F\; M)$

We consider $\lambda$-terms modulo $\alpha$-conversion
$\beta$-axiom: $$(\lambda x.M)N=_{\beta}M[x:=N]$$
$\beta$-reduction rule: $$(\lambda x.M)N\to _{\beta}M[x:=N]$$

# Intuitive approach to substitution 
- **Two terms are $\alpha$-convertible if they are the same up to renaming of bound variables**
- We identify $\alpha$-equivalent terms, so we work modulo $\alpha$ 
- If we work modulo renaming of bound variables, that is, we identify terms that are the same up to a renaming of bound variables (we work with $\alpha$-equivalence classes) then we rename the bound variables such that they differ from free variables

> [!definition] Informal recursive substitution definition
> $x\,[x:=N]=N$
> $a\,[x:=N]=a$ with $a\neq x$ a variable or a constant
> $(PQ)[x:=N]=(P[x:=N])(Q[x:=N])$
> $(\lambda x.P)[x:=N]=\lambda x.P$
> $(\lambda x.P)[x:=N]=\lambda y.(P[x:=N])$

> [!example]
> $$(\lambda x.x)[x:=c]=\lambda x.x$$
> $$(\lambda x.y)[y:=c]=\lambda x.c$$
> $$(\lambda x.y)[y:=x]=\lambda z.x$$
> $$(\lambda y.x(\lambda w.vwx))[x:=uv]=\lambda y.uv(\lambda vw(uv))$$
> $$(\lambda y.x(\lambda x.x))[x:=\lambda y.xy]=\lambda y.(\lambda y.xy)(\lambda x.x)$$
> $$((\lambda x.x)x(\lambda y.xu))[x:=y]=(\lambda x.x)y(\lambda y'.yu)$$
# More formal approach to substitution
Substiution in a variable or constant:
$$x [x:= N]=N$$
$$a[x:=N]=a$$ with $a\neq x$ a variable, or a constant
Substitution in an application:
$$(PQ)[x:=N]=(P[x:=N])(Q[x:=N])$$
Substitution in an abstraction:
$$(\lambda x.P)[x:=N]=\lambda x.P$$
$$(\lambda y.P)[x:=N]=\lambda y.(P[x:=N])$$ if $x\neq y$ and $y\notin FV(N)$
$$(\lambda y.P)[x:=N]=\lambda z.(P[y:=z][x:=N]) \text{ if } x\neq y \text{ and }y\notin FV(N)\cup FV(P) \text{ and } y\in FV(N)$$
where $z$ is the first fresh variable

> [!definition] Alpha
> $a=_{\alpha}a$ for $a$ a variable or a constant
> if $M=_{\alpha}M'$ and $N=_{\alpha} N'$, then $M\; N=_{\alpha} M'\; N'$
> if $M[x:=z]=_{\alpha} N[y:=z]$ and $z$ is fresh for $M$ and for $N$, then $\lambda x.M=_{\alpha}\lambda y.N$
# Beta-equality and beta-reduction
> [!definition] Beta Equality
> # $\beta$-axiom: 
> $$(\lambda x.M)N=_{\beta}M[x:=N]$$
> $M[x:=N]$ is the result of substituting $N$ for the free occurrences of $x$ in $M$
> # Reflexivity, symmetry, transitivity
> $$\begin{align}&M=_{\beta}M \\
> &\text{if } M=_{\beta}N \text{ then } N=_{\beta}M \\
> &\text{if } M=_{\beta}N \text{ and } N=_{\beta}P \text{ then } M=_{\beta}P\end{align}$$
> # Compatibility with term formation
> $$\begin{align}
> & \text{if } M=_{\beta}N \text{ then } L\, M =_{\beta}L\, N\\
> & \text{if } M=_{\beta}N \text{ then } M\, R =_{\beta}N\, R\\
> & \text{if } M=_{\beta}N \text{ then } \lambda x.M=_{\beta}\lambda x.N\\
> \end{align}$$

> [!example] Beta Equality
> 1. $$(\lambda x.x)y=_{\beta} x[x:= y]=y$$
> 2. $$(\lambda x.xx)y=_{\beta} (xx)[x:= y]=yy$$
> 3. $$(\lambda x.xz)y=_{\beta} (xz)[x:= y]=yz$$
> 4. $$(\lambda x.z)y=_{\beta} (z)[x:= y]=z$$
> 5. $$\Omega=(\lambda x.xx)(\lambda x.xx)=_{\beta}\Omega$$
> 6. $$K=\lambda x.\lambda y.x$$
> 7. $$K I\Omega=_{\beta}KI\Omega \text{ and also }KI\Omega =_{\beta}(\lambda y.I)\Omega =_{\beta}I$$

> [!definition] Beta Reduction
> $$(\lambda x.M)N\to_{\beta}M[x:=N]$$
> Compatible with term formation:
> $$\begin{align}
> &\text{if } M\to_{\beta} N \text{ then } L\, M\to_{\beta}L\, N\\
> &\text{if } M\to_{\beta} N \text{ then } M\, R\to_{\beta}N\, R\\
> &\text{if } M\to_{\beta} N \text{ then } \lambda x.M\to_{\beta}\lambda x.N
> \end{align}$$

## Combinators can be safely substituted
A term that is closed is sometimes called a combinator. Examples of some combinators:
$$K=\lambda x.\lambda y.x \text{ and }I=\lambda x.x$$
we have:
$$K\,I=(\lambda x.\lambda y.x)I\to_{\beta}\lambda y.I$$
$$Ky=(\lambda x.\lambda y.x)y\to_{\alpha}(\lambda x.\lambda y'.x)y\to_{\beta}\lambda y'.y$$
# Relation with Haskell
- **Application by juxtaposition**
  $F\,M\,N$ and `head [1,2]`
- **Partial application**
  `sum=foldr(+) 0`
- **Anonymous function**
  $\lambda x.x$ and `\x -> x`
- **Only arguments of the right type**
  so far: untyped $\lambda$-calculus, later simply typed $\lambda$-calculus

---
References: