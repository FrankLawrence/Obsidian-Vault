---
Tags: Math
Created: 2024-10-05 18:35:12
---
(Links:: [[Equational Programming]])
# Combinatory Logic
- We assume countably infinitely many variables $x,y,z,...$
- We have three constants $S$ $K$ $I$
- We have the binary operator for application
- We write application in infix notation
- Application is associative to the left

$$\begin{align}
I\,x & =_{CL} x\\
K\,x\,y & =_{CL}x \\
S\,x\,y\,z & =_{CL} (x\,y)\,(y\,z) \\
\end{align}$$

> [!example] Rewrite Sequences
> $$S\,K\,K\, x\to_{CL}(K\,x)(K\,x)\to_{CL}x$$
> $$(S\,I\,I)\,(S\,I\,I)\to_{CL}(I(S\,I\,I))\,(I(S\,I\,I))\to_{CL}(S\,I\,I)\,(I(S\,I\,I))\to_{CL}(S\,I\,I)\,(S\,I\,I)$$
# The word problem / validity problem
## Validity Problem:
- Assume an equational specification $$((S,\Sigma),E)$$
- Assume two closed terms $s$ and $t$
- Do we have $E\vdash s=t$?
	- **The validity problem is undecidable**
	- An example of an undecidable equational specification is combinatory logic
- We also have the uniform validity problem:
	- Do we have $E\vdash s=t$ for not necessarily closed terms?
	- The uniform validity problem is also **undecidable**

- Given an equation $s=t$ 
	- use equations $l=r$ from left to right to rewrite $s$ to its normal form $s'$
	- use equations $l=r$ from left to right to rewrite $t$ to its normal form $t'$
- if $s'$ and $t'$ are the same term, then $s=t$ is derivable, otherwise it is not 
# Rewriting
You can replace formulas with axioms with equals, but use the axioms **only from left to right**! We then often use the notation $\to$ instead of $=$, both in the equations, then called rules, and in the derivations.

> [!example] Specification for natural numbers
> Do we have $2+(1+3)=(1+1)\times 3$: $$E\vdash add(succ(succ(0)),add(succ(0),succ(succ(succ(0)))))=mul(add(succ(0),succ(0)),succ(succ(succ(0))))$$
> Yes, because both terms can be rewritten to the same term using the axioms from left to right. We also have $$E\vdash succ(add(succ(0),succ(0)))=succ(succ(succ(0)))$$
> $$E\vdash succ(mul(succ(0),succ(0)))=succ(succ(0))$$
> obtained by rewriting the left term to the right (constructor) term

## Termination or Strong Normalization
Similar as for $\beta$-reduction: **A Term is terminating if it is not the beginning of an infinite rewrite sequence**. A rewrite system is terminating if all terms are terminating.

> [!example]
> - With rule $a\to f(a)$ the term $a$ is not terminating because we have an infinite reduction $a\to f(a)\to f(f(a))\to\dots$
> - With rules $a\to a$, $a\to b$ the term $a$ is not terminating because we have an infinite reduction $a\to a\to a\dots$ even though also $a\to b$
> - The oriented equations for natural numbers are terminating

## Confluence
**A term $s$ is confluent if we have**:
- if **$s\to^{*} t$** (doing zero, one or more steps), **and**
- **$s\to^{*} t'$** (doing zero, one or more steps)

**then there is a term $u$**, called a common reduct, **such that** $$t\to^{*} u\text{ and } t'\to^{*} u$$
A rewriting system is confluent if all terms are confluent.

> [!example]
> With rules $a\to b$, $a\to c$ the term $a$ is not confluent because we have $a\to b$ and $a\to c$ and $b$ and $c$ cannot be joined.
> The oriented equations for natural numbers are confluent.

Termination gives that every term has at least one normal form. Confluence gives that every term has at most one normal form. **So in a terminating and confluent system, every term has a unique normal form**. 

---
References: