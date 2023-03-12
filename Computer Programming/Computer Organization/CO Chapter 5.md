---
Tags: 
Created: 2023-03-12 20:13:18
---
(Links:: [[Computer Organization]])
# 5.3
- If the same field locations can be used, then there doesn't need to be more circuitry to accept and decode different formats. This leads to a more simple and faster hardware circuitry.
# 5.6
$$
\begin{align}
XGY&=\bar x_3y_3+\overline{(x_3+y_3)}\times \biggr(x_2\bar y_2+\overline{(x_2\oplus y_2)}\times \Bigr(x_1\bar y_1+\overline{(x_1\oplus x_2)}x_0\bar y_0\Bigr)\biggr)
\\
&+(x_3 y_3)\times \biggr(\bar x_2 y_2+\overline{(x_2\oplus y_2)}\times \Bigr(\bar x_1 y_1+\overline{(x_1\oplus x_2)}\bar x_0 y_0\Bigr)\biggr)
\end{align}$$
$$XEY = \overline{(x_3\oplus y_3)}\times\overline{(x_2\oplus y_2)}\times\overline{(x_1\oplus y_1)}\times\overline{(x_0\oplus y_0)}$$
$$XLY = \overline{XGY+XEY}$$

# 5.9
# 5.12
# 5.15
# 5.18
# 5.21

# 5.24


---
References: