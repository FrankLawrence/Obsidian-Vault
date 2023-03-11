---
Tags: 
Created: 2023-02-08 16:47:35
---
(Links:: [[Computer Organization]])
# Addition and subtraction of signed numbers
- each stage of the addition process must accommodate a carry-in bit
- carry-in $c_i$ is the same as the carry-out from stage (i-1)
- $s_i$ implemented with 3-input XOR gate
- carry-out function $c_{i+1}$ implemented with AND-OR circuit
- [[Logic specification for a stage of binary addition]]
- [[Logic for a single stage.png]]
- [[An n-bit ripple-carry adder.png]]
	- carries propagate through the cascade -> *ripple-carry adder*
## Addition/Subtraction Logic Unit
- detect [[Basic Structure of Computers#Overflow in Integer Arithmetic|overflow]] (operands are the same, sign of result is different) with expression: 
  $$\begin{align}\texttt{Overflow} & = x_{n-1}y_{n-1}\bar s_{n-1} + \bar x_{n-1}\bar y_{n-1}s_{n-1} \\ & = c_n \oplus c_{n-1} \end{align}$$
- [[Binary addition-subtraction logic circuit.png]]
	- when Add/Sub control line is set to 1:
		- $Y$ number is 1's-complemented by XOR gates
		- $c_0$ is set to complete 2's complementation of $Y$
# Design of Fast Adders
- signal delay through a network of logic gates depends on the integrated circuit electronic technology used in fabricating the network and on the number of gates in the paths from inputs to outputs
- delay through any combinatioanl circuit is the longest signal propagation path through the circuit

> [!question] How do we reduce delay in adders?
> -> use fastest possible electronic technology
> ->> Use logic gate network carry-lookahead network
## Carry-Lookahead Addition
$$\begin{align}
c_{i+1} & =x_iy_i+x_ic_i+y_ic_i \\
& = x_iy_i + (x_i+y_i)c_i \\
& = G_i + P_ic_i
\end{align}$$
$$\texttt{where} \quad G_i = x_iy_i \qquad \texttt{and} \qquad P_i =x_i+y_i$$
- $G_i$ and $P_i$: *generate* and *propagate* functions for stage *i*
# Multiplication of Signed Numbers
- ![[Sign extension of negative multiplicand.png|500]]
- for a negative multiplier -> form 2's-complement of both the multiplier and the multiplicand and proceed as in the case of a positive mulitplier
- **left most digit in multiplicand is repeated** (sign extension)
## Booth Algorithm
- number of required operations can be reduced by recoding the multiplicator as the difference of two numbers
	- $0011110 (30) = 0100000 (32) - 0000010 (2)$
	  ![[Normal and Booth multiplication schemes.png|500]]
<table style="width:100%;text-align:center">
	<caption>Booth multiplier recoding table</caption>
	<tr>
		<th colspan="2" style="text-align:center">Multiplier</th>
		<th rowspan="2"style="text-align:center">Version of multipicand selected by bit i</th>
	</tr>
	<tr>
		<th style="text-align:center">Bit i</th>
		<th style="text-align:center">Bit i-1</th>
	</tr>
	<tr>
		<td>0</td>
		<td>0</td>
		<td>0xM</td>
	</tr>
	<tr>
		<td>0</td>
		<td>1</td>
		<td>+1xM</td>
	</tr>
	<tr>
		<td>1</td>
		<td>0</td>
		<td>-1xM</td>
	</tr>
	<tr>
		<td>1</td>
		<td>1</td>
		<td>0xM</td>
	</tr>
</table>
# Fast Multiplication
## Bit-Pair Recoding of Multipliers
- use of at most one summand for each pair of bits in the multiplier
- form pairs of bits -> two bits at a time from the right are rewritten to use at most one version of the multiplicand to be added
<table style="width:100%; text-align:center">
	<caption>Table of mulitplicand selection decisions</caption>
	<tr>
		<th colspan="2" style="text-align:center">Multiplier bit-pair</th>
		<th rowspan="2"style="text-align:center">Multiplier bit on the right <br> i-1</th>
		<th rowspan="2"style="text-align:center">Multiplicand selected at position i</th>
	</tr>
	<tr>
		<th style="text-align:center">i+1</th>
		<th style="text-align:center">i</th>
	</tr>
	<tr>
		<td>0</td>
		<td>0</td>
		<td>0</td>
		<td>0xM</td>
	</tr>
	<tr>
		<td>0</td>
		<td>0</td>
		<td>1</td>
		<td>+1xM</td>
	</tr>
	<tr>
		<td>0</td>
		<td>1</td>
		<td>0</td>
		<td>+1xM</td>
	</tr>
	<tr>
		<td>0</td>
		<td>1</td>
		<td>1</td>
		<td>+2xM</td>
	</tr>
	<tr>
		<td>1</td>
		<td>0</td>
		<td>0</td>
		<td>-2xM</td>
	</tr>
	<tr>
		<td>1</td>
		<td>0</td>
		<td>1</td>
		<td>-1xM</td>
	</tr>
	<tr>
		<td>1</td>
		<td>1</td>
		<td>0</td>
		<td>-1xM</td>
	</tr>
	<tr>
		<td>1</td>
		<td>1</td>
		<td>1</td>
		<td>0xM</td>
	</tr>
</table>
## Carry-Save Addition of Summands
- [[Carry-save array.png]]
- speed up multiplication process by saving the carries and introducing them into the next row at the correct weighted positions
- result is described as the sum of 2 binary numbers:
	- S is the sum obtained by adding the digits without any carry propagation (OR operaition on the numbers)
	  $S_i = a_i \oplus b_i \oplus c_i$
	  Ex: $1100 + 0101 = 1001$ 
	- C is composed of carries from the previous individual sums
	  $C_{i+1}=(a_ib_i) + (b_ic_i) + (c_ia_i)$
	  Ex: $011 + 101 = 010$
## Summand Addition Tree using 3-2 Reducers
- group summands in threes and perform carry-save addition on each of these groups in parallel to generate a set of $S$ and $C$ vectors in one full-adder delay
	- Ex: Add 6 numbers (A-F) by doing carry-save addition on the first three and the last three, then doing carry-save addition on the two sums $S$ and carry $C$
	  ![[Schematic representation of the carry-save addition operations.png|400]]
	- The final regular addition operation on $S_4$ and $C_4$ can be done with a carry-lookahead adder
	- reduction in delay is significant with more summands
## Summand Addition Tree using 4-2 Reducers
- regular structured tree in the case in which the number of summands to be reduced is a power of 2
  Ex: 32 summands requires four levels for 4-2 reducers and eight levels for 3-2 reducers
- use of sum bit $s$, a carry bit $c$ and second carry bit $c_{out}$
- each 4-2 reducer sends $c_{out}$ laterally to the 4-2 in next high-weighted bit position on same reduction level -> fifth input $c_{in}$

> [!summary]
> - three outputs $s$, $c$, and $c_{out}$ represent the arithmetic sum of the five inputs: $$w+x+y+z+c_{in}=s+2(c+c_{out})$$
> - Output $s$ is the usual sum variable: $XOR$ function of the five input variables
> - lateral carry $c_{out}$ independent of $c_{in}$; function of four input variables $w$, $x$, $y$ and $z$
- [[4-2 reducer block.png]]
- [[4-2 reducer truth table]] -> logic gate network can be derived from the table
# Floating-Point Numbers and Operations
-  ![[Basic Structure of Computers#Floating-Point numbers]]
- Normalized value: implicit 1 to the left of the binary point
- Special values
	- $E' = 0$ and $M = 0$ -> 0
	- $E' = 255$ and $M = 0$ -> $\infty$
	- $E' = 0$ and $M \neq 0$ -> *denormal* numbers ($\pm0.M\times 2^{-126}$) -> smaller than smallest number (*gradual underflow*)
	- $E' = 255$ and $M \neq 0$ -> *Not a Number* (NaN)
## Arithmetic Operations on Floating-Point Numbers
> [!note]+ Add/Subtract Rule
> 1. Choose the number with the smaller exponent and shift it's mantissa right a number of steps equal to the difference in exponents.
> 2. Set the exponent of the result equal to the larger exponent.
> 3. Perform addition/subtraction on the mantissas and determine the sign of the result
> 4. Normalize the resulting value, if necessary

> [!note]+ Multiply Rule
> 1. Add the exponents and subtract 127 to maintain the excess-127 representation
> 2. Multiply the mantissas and determine the sign of the result
> 3. Normalize the resulting value, if necessary

> [!note]+ Divide Rule
> 1. Subtract the exponents and add 127 to maintain the excess-127 representation
> 2. Divide the mantissas and determine the sign of the result
> 3. Normalize the resulting value, if necessary
## Guard Bits and Truncation
- *von Neumann rounding*
	- all 0s are dropped
	- if any of the bits to be removed are 1, the least significant bit of retained bits is set to 1
	- Ex: 6-bit to 3-bit truncation
		- all 6-bit fractions with $b_{-4}b_{-5}b_{-6}$ are truncated to $0.b_{-1}b_{-2}1$
		- error ranges between $-1$ and $+1$ in LSB position of retained bits -> *unbiased* because symmetrical about 0
		  -> with more operations positive errors offset negative errors -> complex computation more accurate
- *rounding*
	- 1 is added to LSB (Last siginificant bit) position of bits retained if MSB position of bits removed is 1
	- Ex: $0.b_{-1}b_{-2}b_{-3}1...$
		- -> rounded to $0.b_{-1}b_{-2}b_{-3} + 0.001$
	- Ex: $0.b_{-1}b_{-2}b_{-3}0...$
		- -> rounded to $0.b_{-1}b_{-2}b_{-3}$
	- with case where removed bits are $10...0$ -> biased to round up -> retained bits rounded to nearest even number
		- $0.b_{-1}b_{-2}0100...$ -> $0.b_{-1}b_{-2}0$
		- $0.b_{-1}b_{-2}1100...$ -> $0.b_{-1}b_{-2}1 + 0.001$
	- **truncation technique**:
	  > "round to the nearest number or nearest even number in case of a tie"
	- error range approximately $-\frac 12$ to $+\frac 12$ in LSB position of retained bits
## Implementing Floating-Point Operations
- ![[Floating-point addition-subtraction unit.png]]
# Decimal-to-Binary Conversion
- convert integer part: divide by 2 and take the remainder -> least significant bit of the integer part 
	- repeats up to and including the step in which the quotient becomes 0
- convert fraction part: multiply by 2 part left to the decimal point if bit $b_{-1}$ -> repeat

---
References: