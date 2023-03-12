---
Tags: 
Created: 2023-02-15 19:01:19
---
(Links:: [[Computer Organization]])
# A.1
| $x$ | $y$ | $XOR$ | $\overline {XOR}$ |
| --- | --- | ----- | ----------------- |
| 0   | 0   | 0     | 1                 |
| 0   | 1   | 1     | 0                 |
| 1   | 0   | 1     | 0                 |
| 1   | 1   | 0     | 1                 |
$\bar x\times \bar y + x \times y$
# A.4
$$f=x_1x_2 \bar x_3+x_1x_2x_3+x_1 \bar x_2 \bar x_3 x_4+x_2 \bar x_3 \bar x_4 + x_1x_2 \bar x_4$$
$$d=x_1\bar x_2x_3x_4+x_1\bar x_2\bar x_3 \bar x_4+\bar x_1\bar x_3x_4$$
$$f=x_1x_2+x_1\bar x_3+x_2\bar x_3$$
# A.7
$f =\bar b_3 + \bar b_2 \bar b_1$
# A.10
| $x$ | $y$ | $z$ | $\lnot (x\land y)$ | $\lnot (y\land z)$ | $\lnot(\lnot(x\land y)\land z)$ | $\lnot ( x \land\lnot(y\land z))$ |
| --- | --- | --- | ------------------ | ------------------ | ------------------------------- | --------------------------------- |
| 0   | 0   | 0   | 1                  | 1                  | 1                               | 1                                 |
| 0   | 0   | 1   | 1                  | 1                  | 0                               | 1                                 |
| 0   | 1   | 0   | 1                  | 1                  | 1                               | 1                                 |
| 0   | 1   | 1   | 1                  | 0                  | 0                               | 1                                 |
| 1   | 0   | 0   | 1                  | 1                  | 1                               | 0                                 |
| 1   | 0   | 1   | 1                  | 1                  | 0                               | 0                                 |
| 1   | 1   | 0   | 0                  | 1                  | 1                               | 0                                 |
| 1   | 1   | 1   | 0                  | 0                  | 1                               | 1                                 |
==> $\overline{(\overline{(x\times y)}\times z)} \not \equiv \overline {( x \times\overline{(y\times z)})}$
# A.13
$$
\begin{align}
f & = (x_1 + x_3)(\bar x_2+\bar x_4) \\
& = \overline {(\overline {x_1 + x_3})} \overline {(\overline {\bar x_2+\bar x_4})} \\
& = (\overline {\bar x_1 \times \bar x_3}) \times \overline {( x_2 \times x_4)} \\
& = \left(\overline {(\overline {x_1\times x_1}) \times (\overline {x_3\times x_3})}\right) \times \overline {(x_2 \times x_4)} \\
& = \overline {\left(\overline {\left(\overline {(\overline {x_1\times x_1}) \times (\overline {x_3\times x_3})}\right) \times \overline {(x_2 \times x_4)}}\right)} \\
& = \overline {\left(\overline {\left(\overline {(\overline {x_1\times x_1}) \times (\overline {x_3\times x_3})}\right) \times \overline {(x_2 \times x_4)}}\right)\times \left(\overline {\left(\overline {(\overline {x_1\times x_1}) \times (\overline {x_3\times x_3})}\right) \times \overline {(x_2 \times x_4)}}\right)} \\
\end{align}
$$
# A.22
| $Clk$ | $D$ | $Q(t+1)$ |
| ----- | --- | -------- |
| 0     | 0   | 0        |
| 0     | 1   | 1        |
| 1     | x   | $Q(t)$   |
The circuit no longer saves the state when the clock value is 0, but when the value is 1. This means the circuit technically works the same just on inverted clock pulses.
$a = \bar x_1 \bar x_2 \bar x_3$
$b = \bar x_1 \bar x_2 x_3$
$c = \bar x_1 x_2 \bar x_3$
$d = \bar x_1 x_2 x_3$
$e = x_1 \bar x_2 \bar x_3$
$f = x_1 \bar x_2 x_3$
$g = x_1 x_2 \bar x_3$
$h = x_1 x_2 x_3$
# A.31
$$Y_2 = \bar y_2$$
$$Y_1 = y_1y_2 + \bar y_1 \bar y_2$$

---
References: