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
[D] Consider an instruction set in which instruction encoding is such that register addresses for different instructions are not always in the same bit locations. What effect would that have on the execution steps of the instructions? What would you do to maintain a five-step execution sequence in this case? Assume the same hardware structure as in Figure 5.8.
# 5.12
[E] The execution of computational instructions follows the pattern given in Figure 5.11 for the Add instruction, in which no processing actions are performed in step 4. Consider a program that has the instruction statistics given in Problem 5.11. Estimate the increase in instruction execution rate if this step is eliminated, assuming that all execution steps are completed in one clock cycle.
# 5.15
[E] We have seen how all RISC-style instructions can be executed using the steps in Figure 5.4 on the multi-stage hardware of Figure 5.8. Autoincrement and Autodecrement addressing modes are not included in RISC-style instruction sets. Explain why the instruction
$$Load \quad R3, (R5)+ $$
cannot be executed on the hardware in Figure 5.8.
# 5.18
[M] Give the sequence of steps needed to fetch and execute the two special instructions
MoveControl Ri, IPS
and
MoveControl IPS, Ri
used in Example 5.6.
# 5.21
As stated in the book, there are other steps during the instruction execution, which do not need to access the memory. During these steps, the Counter_enable signal should be set to 1. In the case where memory is accessed, it should be set to 1 when MFC is 1. This is why both signals are needed for Counter_enable to work.
# 5.24
[M] A microprogrammed processor has the following parameters. Generating the starting address of the microroutine of an instruction takes 2.1 ns, and reading a microinstruction from the control store takes 1.5 ns. Performing an operation in the ALU requires a maximum of 2.2 ns, and access to the cache memory requires 1.7 ns. Assume that all instructions and data are in the cache.
(a) Determine the minimum time needed for each of the steps in Figure 5.26.
(b) Ignoring all other delays, what is the minimum clock cycle that can be used for this processor?

---
References: