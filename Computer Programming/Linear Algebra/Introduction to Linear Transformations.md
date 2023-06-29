---
Tags: 
Created: 2023-04-10 23:36:45
---
(Links:: [[Linear Algebra]])
- Matrix multiplication "acts" on a vector by transforming it
- Solving $Ax=b$ amounts to **finding all vectors $x$ in $\Bbb R^n$ that are transformed into the vector $b$ in $\Bbb R^m$** under the "action" of multiplication by $A$
- The **transformation/function/mapping** $T$ from $\Bbb R^n$ to $\Bbb R^m$ is a rule that assigns to each vector $x$ in $\Bbb R^n$ a vector $T(x)$ in $\Bbb R^m$. (Notation: $T:\Bbb R^{n}\to \Bbb R^{m}$)
	- The vector $T(x)$ is called the **image**
	- The set of all images $T(x)$ is called the **range** of $T$

# Linear Transformations
> [!definition] Linear Transformation
> A transformation (or mapping) $T$ is **linear** if
> 1. $T(u+v)=T(u)+T(v)$ for all $u$,$v$ in the domain of $T$;
> 2. $T(cu)=cT(u)$ for all scalars $c$ and all $u$ in the domain of $T$.

- Linear transformations *preserve the operations of vector addition and scalar multiplication*

---
References: