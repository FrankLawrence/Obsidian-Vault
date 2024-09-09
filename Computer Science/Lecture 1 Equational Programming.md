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
# More formal approach to substitution
# Beta-equality and beta-reduction
# Relation with Haskell


---
References: