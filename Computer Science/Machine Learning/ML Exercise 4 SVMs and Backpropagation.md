---
title: Week 4 answers
---
# Back-propagation
# Normalization

> [!question]- Why is it useful to have an orthonormal basis? 
> Matrices representing an orthonormal basis are *orthogonal*. This means that $C^{−1} = C^{\top}$. This means that we can easily transform from and to the basis without hav- ing to compute an expensive matrix inverse.

> [!question]- 

$$\begin{align}
p({\color{#0C8}\text{Ham}}\mid e) &= p({\color{#0C8}\text{Ham}} \mid X_{p}=1, X_{m}=0) \\
&\propto p(X_{p} = 1 \mid {\color{#0C8}\text{Ham}})p(X_{m} = 0\mid {\color{#0C8}\text{Ham}}) p({\color{#0C8}\text{Ham}}) \\
&= \frac{1}{4} \frac{2}{4} \frac{1}{2} \\
&= \frac{2}{32} \\
p({\color{#F08}\text{Spam}}\mid e) &= p({\color{#F08}\text{Spam}} \mid X_{p}=1, X_{m}=0) \\
&\propto p(X_{p} = 1 \mid {\color{#F08}\text{Spam}})p(X_{m} = 0\mid {\color{#F08}\text{Spam}}) p({\color{#F08}\text{Spam}}) \\
&= \frac{3}{4} \frac{3}{4} \frac{1}{2} \\
&= \frac{9}{32} \\ \\
p({\color{#0C8}\text{Ham}} \mid X_{p}=1, X_{m}=0) &= \frac{p(X_{p},X_{m}\mid {\color{#0C8}\text{Ham}})p({\color{#0C8}\text{Ham}})}{p(X_{p},X_{m})} \\
&= \frac{p(X_{p},X_{m}\mid {\color{#0C8}\text{Ham}})p({\color{#0C8}\text{Ham}})}{\sum_{Y\in \{{\color{#F08}\text{Spam}}, {\color{#0C8}\text{Ham}}\}} p(X_{p},X_{m} \mid Y)p(Y)} \\ 
&= \frac{\frac{2}{32}}{\frac{2}{32}+ \frac{9}{32}} = \frac{\frac{2}{32}}{\frac{11}{32}} = \frac{2}{\cancel{32}} \frac{\cancel{32}}{11} = \frac{2}{11} \\ \\
p({\color{#F08}\text{Spam}} \mid X_{p}=1, X_{m}=0) &= \frac{p(X_{p},X_{m}\mid {\color{#F08}\text{Spam}})p({\color{#F08}\text{Spam}})}{p(X_{p},X_{m})} \\
&= \frac{p(X_{p},X_{m}\mid {\color{#F08}\text{Spam}})p({\color{#F08}\text{Spam}})}{\sum_{Y\in \{{\color{#F08}\text{Spam}}, {\color{#0C8}\text{Ham}}\}} p(X_{p},X_{m} \mid Y)p(Y)} \\ 
&= \frac{\frac{9}{32}}{\frac{2}{32}+ \frac{9}{32}} = \frac{\frac{9}{32}}{\frac{11}{32}} = \frac{9}{\cancel{32}} \frac{\cancel{32}}{11} = \frac{9}{11}
\end{align}$$