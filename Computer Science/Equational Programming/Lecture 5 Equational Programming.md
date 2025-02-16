---
Tags: Math
Created: 2024-09-22 20:03:53
---
(Links:: [[Equational Programming]])
# Recursive Functions in lambda calculus
> [!example] 
> ```haskell
> factorial n = if (n=0)
> 	then 1
> 	else (n * factorial (n-1))
> ```
> ```haskell
> sum [] = 0
> sum (h:t) = h + (sum t)
> ```
> ```haskell
> length [] = 0
> length (h:t) = (length t) + 1
> ```
# Length in lambda-calculus
- Out definition: length = $\lambda l.$ if $l$ is empty then zero, else length of tail of $l$ plus 1
- Lists are represented as $\text{nil}:=\lambda xy.y$ and $\text{cons}:=\lambda ht.\lambda z.z\,h\,t$
- Conditionals are represented as $\text{ite}:=\lambda b.\lambda x.\lambda y.b\,x\,y$ (or simply $b\,x\,y$ with $b$ the test and $x$ and $y$ the branches)
- Checking if $l$ is empty is done with $\text{isempty}:=\lambda l.l(\lambda xyz.\text{false})\,\text{true}$
- Church numerals with $0$ are represented as $c_{0}=\lambda s.\lambda z.z$
- The tail of $l$ is represented as $\text{tail}:=\lambda l.l\,(\lambda h.\lambda t.t)$
- Plus 1 is represented as the successor $S=\lambda x\lambda s.\lambda z.s\,(x\,s\,z)$

Combining all of these we get: $$\text{length}:=\lambda l.(\text{isempty}\,l)c_{0}(S(\text{length}(\text{tail } l)))$$ which still contains length. If we define $$M:= \lambda a.\lambda l.(\text{isempty}\,l)c_{0}(S(a\,(\text{tail } l)))$$ we get $$M\text{ length}=_{\beta}\text{length}$$
We look for a fixed point of $M$ by using the fixed point combinator $Y$: $$\text{length}:=Y\,M$$

> [!example] Nil
> $$\begin{align}
> \text{length nil} &=_{\beta} Y\,M\,\text{nil} \\
> &= (\lambda{\color{green}f}.(\lambda x.{\color{green}f}(x\,x)))\,{\color{green}M}\,\text{nil} \\
> &= (\lambda {\color{red}x}.M({\color{red}x\,x}))\,{\color{red}(\lambda x.M\,(x\,x))}\,\text{nil} \\
> &= M\,{\color{orange}((\lambda x.M\,(x\,x))\,(\lambda x.M\,(x\,x)))}\,{\color{lightblue}\text{nil}} \\
> &= (\text{isempty}{\color{lightblue}\text{ nil}})\,c_{0}\,(S\,({\color{orange}((\lambda x.M\,(x\,x))\,(\lambda x.M\,(x\,x)))}\,(\text{tail }{\color{lightblue}\text{nil}}))) \\
> &= \text{true } c_{0}\,(...) \\
> &= c_{0}
> \end{align}$$
# Recursive Functions
We try to define $$G\text{ with }G=...G...$$ hence we look for $$G\text{ with }G=(\lambda g...g...)\,G$$ hence we take $$\text{a fixed point of }\lambda g...g...$$ that is, using a fixed point combinator $Y$ we define: $$G=Y\,(\lambda g...g...)$$

---
References: