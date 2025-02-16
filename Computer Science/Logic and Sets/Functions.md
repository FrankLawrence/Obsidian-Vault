---
Tags: Math
Created: 2022-12-16 12:52:24
---
(Links:: [[Logic and Sets]])
- A function *$f:A\to B$* is a binary relation of type $A\times A$ such that every $x\in A$ relates to at most one $y\in B$
  *Notation:* $f(x)=y \quad\Longleftrightarrow\quad \langle x, y \rangle \in F$
- *domain* A
- *codomain* B
- *domain of definition* $D_f := \{x\in A: f(x)=y \text{ for }y \in B\}$
- *range* $R_f := \{y\in B: y=f(x) \text{ for }x \in A\}$
- if $D_f = A$ then f is *total*
- if $R_f = B$ then f is *surjective*
## Injections and surjections
- $f:A \to B$ is **injective** if 
  $x_1 \neq x_2 \to f(x_1) \neq f(x_2)$
  (only one way to get a value through the function f)
- $f:A \to B$ is **surjective** if 
  $R_f = B$
  (all values in the codomain are reachable)
Example: 
$f:R\to\{y:y\geq 0\}$ with $f(x) = |3x-1|$
- not injective
- surjective
$g:\Bbb Z\to\Bbb N$ with $g(x) = |3x-1|$
- injective
- not surjective
## Composition of functions
The composition of $g\circ f$ of $f:A\to B$ and $g:B\to C$ satisfies $$f(x) = y \land g(y) = z \quad \Rightarrow \quad (g\circ f)(x) = g(f(x)) = z$$
**Note:** $D_{g\circ f} \subseteq D_f$ and it is possible that $D_{g\circ f} \neq D_f$
$R_{g\circ f} \subseteq R_g$ and it is possible that $R_{g\circ f} \neq R_g$
Example:
$f: R\to R$ and $g:R\to R$
$f(x) = 1-x^2 \quad D_f=R \quad R_f=\{y:y\leq 1\}$
$g(x) = \sqrt{x} \quad D_f=\{x:x\geq 0\} \quad R_f=\{y:y\geq 0\}$
$(g\circ f)(x)=g(f(x)) = \sqrt{1-x^2}$
$D_{g\circ f}=\{x:-1\leq x \leq 1$ **$\neq D_f$**
$R_{g\circ f}=\{y:0\leq y \leq 1$ **$\neq R_f$**
## Inverse functions
- The inverse of $f$ is a function if and only if $f$ in injective
  injective: *one-one*
  non-injective: *many-one*
- The inverse of a total injection $f$ is total if and only if $f$ is surjective
  $f: A \to B$ is *bijective* if
	- $f$ is total
	- $f$ is injective
	- $f$ is surjective
	  Bijections have a bijective inverse
## Cardinality
- The cardinality $|A|$ of $A$ is an abstract notion of the size of a set $A$
  $|A|\leq |B|$ <-> there exists a total injection $f:A\to B$
  $|A|=|B|$ <-> there exists a bijection $f:A\to B$
  ## Countable and uncountable sets
A set $A$ is called
- **countbly infinite** if there exists a bijection $f:N\to A$
- **countable** if $A$ is finite or countably infinite
- **uncountable** if $A$ is not countable
Example:
- $N\times N$ is countably infinite
  $\{\langle 0,0 \rangle ,\langle 0,1 \rangle ,\langle 1,1 \rangle ,\langle 1,0 \rangle ,\langle 2,0 \rangle ,\langle 2,1 \rangle ,...\}$
- The set $(0,1):=\{x:0<x<1\}$ cannot be enumerated


---
References: