---
Tags: 
Created: 2023-02-28 13:04:03
---
(Links:: [[Computer Organization]])
# 9.4
$$
\begin{equation*}
\begin{array}{B3}
	  && 0101 & \\
	& + & 0110 & \\ \hline
	  && 1011 & >1010\\
	  && 0110 & \\ \hline
	  & 1 & 0001 & \\
\end{array}
\end{equation*}
$$
$$
\begin{equation*}
\begin{array}{B3}
	  && 0011 & \\
	& + & 0100 & \\ \hline
	  && 0111 & <1010\\
\end{array}
\end{equation*}
$$
# 9.10
## A
$$
\begin{equation*}
\begin{array}{B3}
	& \quad01 & 0111 \\
	\times & \quad11 & 0110 \\ \hline
	& \quad01 & 0111 \\
	\times & \;\;\;-1 & +2 -2\; & \Leftarrow  \\ \hline
	\textcolor{blue}{1111}& \textcolor{blue}{1}101 & 0010 \\
	\textcolor{blue}{000}0& 1011& 10\quad \\
	\textcolor{blue}{11}10 & 1001& \\ \hline
	1111 & 0001 & 1010 \\
\end{array}
\end{equation*}
$$
## B
$$
\begin{equation*}
\begin{array}{B3}
	& \quad11 & 0011 \\
	\times & \quad10 & 1100 \\ \hline
	& \quad11 & 0011 \\
	\times & \;\;\;-1 & -1 \;\;0\; & \Leftarrow  \\ \hline
	
	\textcolor{blue}{0000}& \textcolor{blue}{00}00 & 0000 \\
	\textcolor{blue}{0000}& 0011& 01\quad \\
	\textcolor{blue}{00}00 & 1101& \\ \hline
	0001 & 0000 & 0100 \\
\end{array}
\end{equation*}
$$
## C
$$
\begin{equation*}
\begin{array}{B3}
	& \quad 00 & 1111 \\
	\times & \quad00 & 1111 \\ \hline
	& \quad00 & 1111 \\
	\times & \;\;\;+1 & 0 -1 & \Leftarrow  \\ \hline
	
	\textcolor{blue}{1111}& \textcolor{blue}{11}11 & 0001 \\
	\textcolor{blue}{0000}& 0000& 00\quad \\
	\textcolor{blue}{00}00 & 1111& \\ \hline
	0000 & 1110 & 0001 \\
\end{array}
\end{equation*}
$$
# 9.16
$$L=1.7\log_216-1.7=1.7\times 4-1.7=6.8-1.7=5.1$$
$$L=1.7\log_232-1.7=1.7\times 5-1.7=8.5-1.7=6.8$$
# 9.19
$$
\begin{equation*}
\begin{array}{B3}
	& \quad 1 & 0101 \\
	\times & \quad 0 & 0101 \\ \hline
	
	\textcolor{blue}{0} &\textcolor{blue}{000}1 & 0101 \\
	\textcolor{blue}{0} & \textcolor{blue}{00}00& 000\;\, \\
	\textcolor{blue}{0} & \textcolor{blue}{0}101& 01\quad \\ 
	\textcolor{blue}{0} & 0000 & 0\quad\;\, \\
	0 & 0000 & \\ \hline
	0 & 0110 & 1001 \\
\end{array}
\end{equation*}
$$
# 9.22
$A = 0 100001 111111110 = 1,99609375 \times 2^2$
$B = 0 011111 001010101 = 1,166015625 \times 2^0$
$diff = 100001-011111=000010$ 
-> Shift mantissa of number with smaller mantissa (B) by two decimal points to the right and add the two mantissas
$$1.111111110+1.000010101=11.000010001=1.100001000$$
$$A+B=0|100010|100001000$$
# 9.25
# 9.28
# 9.31

---
References: