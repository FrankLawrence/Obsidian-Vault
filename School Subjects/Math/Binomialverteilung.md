Status:
Tags: [[Stochastik]]
Links: [[Parameter k bestimmen]] - [[Parameter p bestimmen]] - [[Parameter n bestimmen]]
___
# Binomialverteilung

Wenn sich eine Zufallsgröße X als Trefferanzahl bei einer Bernoulli-Kette der Länge n und Trefferwahrscheinlichkeit p beschreiben lässt, dann heißt die zugehörige Wahrscheinlichkeitsverteilung **Binomialverteilung** mit den Parametern n und p. Die Wahrscheinlichkeit für genau k Treffer berechnet man mit der **Formel von Bernoulli**

> [!Formula] 
> $$P(X=k) = \binom {n}{k} \cdot p^k \cdot (1-p)^{n-k}$$

Diese Wahrscheinlichkeit wird auch mit $B_{n;p}(k)$ bezeichnet-
$P(X \leq k)=P(X=0)+P(X=1)+\dots +P(X=k)$ heißt **kumulierte Wahrscheinlichkeit**.
Der **Erwartungswert** von X ist $\mu = n\cdot p$.
Die **Standardabweichung** von X ist $\sigma = \sqrt{n\cdot p \cdot (1-p)}$
___
References:

Created: 2022-04-22 22:20