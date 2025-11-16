If $P(A)>0$, then: $$P(B|A)=\frac{P(B \cap A)}{P(A)}$$
If $A$ has occurred, $B$ only happens if outcome is in both $A$ and $B$. Hence, in $A\cap B$. In addition, the sample space has been reduced to $A$.

> [!caution]
> In general, $P(B|A)\neq P(A|B)$

> [!example] 
> We have two sets the *health* of someones *teeth* and the *age group*:
> - $\text{Teeth}=\{\text{healthy},\text{unhealthy}, \text{fake}\}$
> - $\text{Age}=\{\text{young},\text{teen},\text{old}\}$
> 
> $$\begin{array}{ccc}
>   &   &      &   T  &     \\
>   &   &   h  &   u  &  f  \\
>   & y & 5/18 & 3/18 & 1/18\\
> A & t & 1/18 & 1/18 & 2/18\\
>   & o & 1/18 & 1/18 & 3/18
> \end{array}$$
> If we want to know the probability of someone having fake teeth given that they're young we can compute: $$p(f|y)=\frac{p(f,y)}{p(y)}=\frac{1/18}{\sum_{x}p(x,y)}=\frac{1/18}{5/18+3/18+1/18}=\frac{1/18}{1/2}=\frac{1}{9}$$