| $x_i$ | $y_i$ | Carry-in $c_i$ | Sum $s_i$ | Carry-out $c_{i+1}$ |
| ----- | ----- | -------------- | --------- | ------------------- |
| 0     | 0     | 0              | 0         | 0                   |
| 0     | 0     | 1              | 1         | 0                   |
| 0     | 1     | 0              | 1         | 0                   |
| 0     | 1     | 1              | 0         | 1                   |
| 1     | 0     | 0              | 1         | 0                   |
| 1     | 0     | 1              | 0         | 1                   |
| 1     | 1     | 0              | 0         | 1                   |
| 1     | 1     | 1              | 1         | 1                   |

$s_i = \bar x_i \bar y_i c_i+\bar x_i y_i\bar c_i+x_i\bar y_i\bar c_i + x_iy_ic_i = x_i \oplus y_i \oplus c_i$
$c_{i+1}=y_ic_i+x_ic_i+x_iy_i$

Example:
![[SCR-20230208-x5u.png|500]]