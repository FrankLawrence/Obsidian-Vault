Status:
Tags: [[Analysis]]
Links: [[Ortslinien]]
___
# Funktionscharen
## Ortslinien
$f_t (x) = (t-x) \cdot e^x$

$f'_t(x)=-1\cdot e^x+(t-x)\cdot e^x=(-x+t-1)\cdot e^x$

$f''_t(x)=-1\cdot e^x+(-x+t-1)\cdot e^x=(-x+t-2)\cdot e^x$

$0=f'_t(x)=(-x+t-1)\cdot e^x$

$x=t-1$

$f''_t(t-1)=-e^{t-1} < 0$ also $H_t(t-1|f_t(t-1))$ bzw. $H_t(t-1|e^{t-1})$

Ortskurve: $x=t-1$ wird nach t aufgelöst $(t=x+1)$ und in $y=e^{t-1}$ eingesetzt. Man erhält als Ortskurve der Hochpunkt $y= e^{(x+1)-1}=e^x$
## Extremstellen
$f_a(x) = 3 - x \cdot e^{-ax^2}$

$f'_a(x) = -e^{-ax^2}-x\cdot e^{-ax^2}\cdot (-2ax) = e^{-ax^2} \cdot (2ax^2-1)$

$f'_a(x)=0 \Leftrightarrow e{-ax^2}\cdot (2ax^2-1)=0 \Leftrightarrow 2ax^2-1=0$

$x_{1,2}=\pm\sqrt\frac{1}{2a}$
Der Faktor $2ax^2-1$ von $f'_a(x)$ wechselt bei $x_1$ das vorzeichen von "-" nach "+", also wechslet auch $f'_a(x)$ das Vorzeichen von "-" nach "+".
Somit ist $x_1$ Minimustelle. Entsprechend ist $x_2$ Maximumstelle.
## Gemeinsame Punkte
$f_t (x) = (t-x) \cdot e^x$

$f_{t_1}(x)=f_{t_2}(x)$
$t_1 \ne t_2 \Rightarrow (x-1)\cdot e^{-t_1\cdot x}= (x-1)\cdot e^{-t_2\cdot x}$

$0=(x-1) \cdot e^{-t_1\cdot x}-(x-1)\cdot e^{-t_2\cdot x} = (x-1)\cdot (e^{-t_1\cdot x} -e^{-t_2\cdot x} )$

Aus $x-1=0$ folgt $x=1$ udn $P(1|f_t(1))$ bzw. $P(1|0)$
Aus $e^{-t_1\cdot x} -e^{-t_2\cdot x}=0$ folgt $e^{-t_1\cdot x} =e^{-t_2\cdot x}$ bzw. $-t_1\cdot x=-t_2\cdot x$
$\rightarrow x=0$ ; $Q(0|f_t(0))$ bzw. $Q(0|-1)$
## [[Abstands- und Winkelberechnungen|Abstand]]
___
References:

Created: 2022-04-23 01:34