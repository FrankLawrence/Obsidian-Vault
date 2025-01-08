---
Tags: 
Created: 2024-10-04 02:57:01
---
(Links:: [[Equational Programming]])
# Algebraic Specifications
We assume two finite sets:
- **Sort names**, also called data sorts, or sorts, denoted by $s$, and
- **Function symbols**, denoted by $f$

Every function symbol $f$ has a type $f : s_{1}\times\cdots\times s_{n}\to s$
A **signature** consists of two finite sets:
- $S$ of sort names, and
- $\Sigma$ of function symbols, together with a typing: $$f : s_{1}\times\cdots\times s_{n}\to s$$

A function symbol $c:\to s$ (that is: $n$ is 0) is called a **constant**.
> [!example]- Stacks
> $S$ is $\{\text{data, stack}\}$
> $\Sigma$ consists of: 
> $$\begin{align}
> d_{1},...,d_{n}\quad &:\quad \to \text{data} \\
> \text{empty}\quad &:\quad \to\text{stack}\\
> \text{push}\quad &:\quad \text{data} \times \text{stack}\to \text{stack}\\
> \text{pop}\quad &:\quad \text{stack}\to\text{stack}\\
> \text{top}\quad &:\quad \text{stack}\to\text{data}\\
> \text{error}\quad &:\quad \to\text{data}\\
> \end{align}$$

> [!definition]
> Assume a signature ($S$,$\Sigma$).
> Also, assume an $S$-sorted set $X=(X_{s})_{s\in S}$ of **variables** such that
> - $X_{s}\cap X_{s'}$ is empty if $s\not\equiv s'$
> - the variables in $X_{s}$ are of sort $s$; notation also $x\;:\;s$ for $x\in X_{s}$.
> 
> *Terms*:
> - a variable $x\;:\;s$ is a term of sort $s$;
> - if $t_{1}\;:\;s_{1},...,t_{n}\;:\;s_{n}$ are terms, and $f\;:\;s_{1}\times\cdots\times s_{n}\to s$ is a function symbol from $\Sigma$, then $f(t_{1},...,t_{n})$is a term of sort $s$A
> 
> We write $c$ instead of c() in case $c\;:\to s$. The set of terms is denoted by **$\text{Ter}_{\Sigma}(X)$**. A term is said to be **closed** if it does not contain variables; Otherwise it is said to be **open**. There are no bound variables.
> > [!example]
> > Open terms: `add(succ(x),0)` and `mul(0,add(y,z))`
> > Closed terms: `succ(add(0,succ(0)))`

An **equation** is a pair of terms of the same sort, written as $$t=u$$ An equational specification is a signature with a finite set of equations. Equational specifications of data types are relatively simple, and executable as a term rewriting system.

> [!example] Equations for Naturals
> $$\begin{align}
> add(x,0)\quad &=\quad x\\
> add(x,succ(y))\quad &=\quad succ(add(x,y))\\
> mul(x,0)\quad &=\quad 0\\
> mul(x,succ(y))\quad &=\quad add(mul(x,y),x)
> \end{align}$$

> [!example] Equational Specification: Natural Numbers
> $S$: $nat$
> $\Sigma$: consisting of four function symbols:
> $$\begin{align}
> 0\quad &:\quad \to \text{nat}\\
> \text{succ}\quad &:\quad\text{nat}\to\text{nat}\\
> \text{add}\quad &:\quad\text{nat}\times\text{nat}\to\text{nat}\\
> \text{mul}\quad &:\quad\text{nat}\times\text{nat}\to\text{nat}
> \end{align}$$
> $E$: consisting of four equations:
> $$\begin{align}
> add(x,0)\quad &=\quad x\tag{1}\\
> add(x,succ(y))\quad &=\quad succ(add(x,y))\tag{2}\\
> mul(x,0)\quad &=\quad 0\tag{3}\\
> mul(x,succ(y))\quad &=\quad add(mul(x,y),x)\tag{4}
> \end{align}$$

> [!example] Equational Specification: Stack
> $S$: $data, stack$
> $\Sigma$: function symbols:
> $$\begin{align}
> d_{1},\dots,d_{n}\quad &:\quad  \to \text{data}\\
> \text{empty}\quad &:\quad \to\text{stack}\\
> \text{push}\quad &:\quad \text{data}\times\text{stack}\to\text{stack}\\
> \text{pop}\quad &:\quad \text{stack}\to\text{stack}\\
> \text{top}\quad &:\quad \text{stack}\to\text{data}\\
> \text{error}\quad &:\quad \to\text{data}
> \end{align}$$
> $E$:equations:
> $$\begin{align}
> pop(empty)\quad &=\quad \text{empty}\tag{1}\\
> top(empty)\quad &=\quad \text{error}\tag{2}\\
> pop(push(x,s))\quad &=\quad s\tag{3}\\
> top(push(x,s))\quad &=\quad x\tag{4}\\
> \end{align}$$
# Derivability
The signature tells us which terms can be built. The equations tell us which terms are 'equal' to each other. The intuition is in line with the formal definition.

## Equational specifications and derivability
An **equational specification** is a pair $((S,\Sigma), E)$ consisting of 
1) a **signature $(S,\Sigma)$** with $S$ a finite set of sorts, and $\Sigma$ a finite set of function symbols
2) a (finite) **set $E$ of equations** over $(S,\Sigma)$, that is: between terms in $\text{Ter}_{\Sigma}(X)$

$E\vdash t=u$ denotes that $t=u$ is derivable from $E$.

## Intuition
$$\begin{align}
&succ(add(succ(0),succ(0))) = \\
&\quad \text{we use \eqref{2} with substitution } x\mapsto succ(0), y\mapsto 0\\
&succ(succ(add(succ(0),0))) = \\
&\quad \text{we use \eqref{1} with substitution } x\mapsto succ(0)\\
&succ(succ(succ(0)))
\end{align}$$

We use equations from the specification 'anywhere in a term'. We use equations from the specification that are 'instantiated'. Instantiations are obtained using **substitutions**. 

We build **terms** from variables and the given signature (ex. `push(n,empty)` and `add(x,succ(y))`). We use **substitutions** to instantiate variables. Ex:
- $\text{map } n \text{ to }d_{1}$ gives $\text{push}(d_{1},\text{empty})$
- $\text{map }x \text{ to } succ(z)$ and $y \text{ to } 0$ gives $add(succ(z),succ(0))$

The mapping from variables to terms is extended to a mapping from terms to terms. 

> [!definition] Substitution
> Let $(S,\Sigma)$ be a signature, $X$ an $S$-sorted set of variables. A **substitution** is a function $$\theta\;:\;X\to\text{Ter}_{\Sigma}(X)$$ where $\theta(x)$ is a term of the sort of the variable $X$. Such a $\theta$ is inductively extended to a function $\bar\theta\;:\; \text{Ter}_{\Sigma}(X)\to\text{Ter}_{\Sigma}(X)$:
> $$\begin{align}
> \bar\theta(x)\quad &\equiv\quad\theta(x)\\
> \bar\theta(f(t_{1},\dots,t_{n}))\quad&\equiv\quad f(\bar\theta(t_{1}),\dots,\bar\theta(t_{n}))\\
> \end{align}$$

> [!example] Substitution
> With $\theta$ defined by $x\mapsto succ(z)$ and $y\mapsto0$, we have:
> $$\begin{align}
> &\bar\theta(\text{add}(x,\text{succ}(y))) &= \\
> &\text{add}(\bar\theta(x),\bar\theta(\text{succ}(y)))&=\\
> &\text{add}(\theta(x),\text{succ}(\bar\theta(y)))&=\\
> &\text{add}(\text{succ}(z),\text{succ}(\theta(y)))&=\\
> &\text{add}(\text{succ}(z),\text{succ}(0))
> \end{align}$$
## More formal notion of derivation

> [!tip] For $\beta$-reduction we considered the definition of $\to_{\beta}$
> Ex: if $M\to_{\beta}N$ then $L\;M\to_{\beta}L\;N$

- **Axioms**:
	- $l=r$ if $l=r$ is an equation in $E$
- **Reflexivity, symmetry, transitivity**:
	- $t=t$
	- if $t=u$ then $u=t$
	- if $t_{1}=t_{2}$ and $t_{2}=t_{3}$ then $t_{1}=t_{3}$
- **Congruence and substitution**:
	- if $t_{i}=u_{i}$ for $i=1,...,n$ then $f(t_{1},...,t_{n})=f(u_{1},...,u_{n})$ 
	- if $t=u$ then $\bar\theta(t)=\bar\theta(u)$ for a substitution $\theta$

> [!example] Formal Derivation: Stacks
> $E\vdash pop(push(x,pop(push(x,empty)))) = empty$
> $$\begin{align}
> & 1\quad pop(push(x,s))=s &\text{(3)} \\
> & 2\quad pop(push(x,empty))=empty &subst,1 \\
> & 3\quad pop(push(x,pop(push(x,empty))))=pop(push(x,empty)) &subst,1 \\
> & 4\quad pop(push(x,pop(push(x,empty))))=empty &trans,3,2 \\
> \end{align}$$

> [!example] Give a derivation from the axioms given below:
> **$$even(add(succ(0)),succ(0))=F$$**
> 
> $$\begin{align}
> add(x,0)&=x \tag{N1}\\
> add(x,succ(y))&=succ(add(x,y)) \tag{N2}\\ \\
> even(0) &= T \tag{E1}\\
> even(succ(x)) &= odd(x) \tag{E2}\\
> odd(0) &= F \tag{E3}\\
> odd(succ(x)) &= even(x) \tag{E4}\\
> \end{align}$$
> Which on of the six axioms is not used in the derivation?
> Is it possible to derive **$add(x,y)=add(y,x)$** from (N1) and (N2)? It seems it is not possible, but how to prove this?

> [!question] When do we have $E\vdash s=t$? 
> If there is a derivation in $E$ with conclusion $s=t$

> [!question] How do we show that there is a derivation?
> Usually by giving one

> [!question] Can we show that there is no derivation, so $E\not \vdash s=t$?
> Yes, sometimes, more later (but the argument 'I cannot fine one' is not valid)

## Matching
> [!definition] Matching
> A left-hand side $l$ of an equation $l=r$ **matches** a term $t$ if there is a substitution $\sigma$ such that $\bar \sigma (l)=t$
>  It computes given $l$ and $s$ such a substitution $\sigma$, or answers 'fail'.

> [!example] 
> $add({\color{red}x},0)$ matches $add({\color{red}add(succ(z),0)},0)$ with $x\mapsto add(succ(z),0)$
> $equal(x,x)$ does not match $equal(1,0)$
> $add(x,0)$ does not match $add(x,y)$

> [!question] Can we decide whether an axiom can be used on a term?
> Yes: A term has finitely many subterms. If our specification has finitely many axioms, we can compute for every axiom $l=r$ whether $l$ or $r$ matches our subterm.


---
References:
