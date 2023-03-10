---
Tags: 
Created: 2023-02-07 12:07:06
---
(Links:: [[Computer Organization]])
# Minimization of Logic Expressions
![[Network for the XOR function 1.png|400]]
- simplify logic expressions by performing a series of [[Algebra of Sets|algebraic manipulations]]
- reduce the cost of implementation of a given logic function 
- [[Disjunctive and Conjunctive normal forms, Adequate systems of connectives, Satisfiability#Disjunctive and conjunctive normal forms|sum-of-products]] expression from truth table -> [[Disjunctive and Conjunctive normal forms, Adequate systems of connectives, Satisfiability#Transformation method to CNF using algorithm *CNF*|minimal sum-of-products]] expression
## Karnaugh Maps
- for three-variable function, map is a rectangle composed of eight squares arranged in two rows of four squares each
- each square corresponds to a particular valuation of the input variables
- entries in the squares are the function values for corresponding input valuations
- horizontally and vertically adjacent squares differ in only one variable
- two adjacent squares that contain 1 have an algebraic simplification
- the product term that correpsonds to a group of squares is the product of the input varialbes whose values are constant on these squares
```ad-hint
- If the value of input variable $x_i$ is 0 for all 1s of a group, then $\bar x_i$ is entered in the product, but if $x_i$ has the value 1 for all 1s of the group then $x_i$ is entered in the product
```
- left-end square are adjacent to the right-end squares
- [[Minimization using Karnaugh maps.png]]
```ad-important
collapse: none
larger groups correspond to smaller product terms
```
## Don't-Care Conditions
- representing 0 through 9 with 4 bits only requires 10 distinct valuations
- we do not care what the function values are for the unused input valuations -> *don't cares* denoted by letter 'd'
- interpret don't-cares as 1s whenever they can be used to enlarge a group of 1s
- minimization is enhanced by the inclusion of don't-care entries
- [[Karnaugh map don't-cares.png|Four-variable Karnaugh map illustrating don't cares]]
# Synthesis with NAND and NOR Gates
- 
# Flip-Flops
- *latch*: stores binary information
![[Basic latch implemented with NOR gates.png|500]]
- remembers which of the two inputs S and R was most recently equal to 1
- [[Basic latch timing diagram.png]]
## Gated Latches
- latch is set and reset from input *clock* -> *gated SR latch*
- when *Clk* is equal to 1, signals *S'* and *R'* follow the inputs S and R
![[Gated SR latch.png|500]]
- Logic circuits whose output are uniquely defined for each input valuation are referred to as *combinational circuits*
- circuits with output dependent on present valuation of the input variables but also of their previous behavior are called *sequential circuits*
 - [[Gated SR latch timing diagram.png]]
```ad-important
title: Gated D latch
collapse: none
two signals derived from single input D
**At clock pulse, Q is set to 1 if D = 1 or is reset to 0 if D = 0**
- [[Gated D latch circuit.png]]
- [[Gated D latch timing diagram.png]]
```
## Master-Slave Flip-Flop
- two gated D latches connected
- A 1-to-0 transition of the clock isolates the master from the input and transfers the contents of the master stage to the slave stage
- no direct path from input D to output Q
- slave stage holds value at the output of the flip-flop while master stage is being set to the next-state value
- [[Master-slave D flip-flop.png]]
## Edge Triggering
- *edge triggered*: data present at the input are transferred to the output only at a transition in the clock signal
```ad-note
- *positive (leading) edge triggered*: data transfer at 0-to-1 clock transition
- *negative (trailing) edge triggered*: data transfer at 1-to-0 clock transition
```
## T Flip-Flop
- changes state every clock cycle if its input T is equal to 1 ("toggles" state)
- [[T flip-flop Circuit.png]]
- [[T flip-flop Timing diagram.png]]
## JK Flip-Flop
- for input valuation J = K = 1, the next state is defined as the complement of the present state of the flip-flop -> functions as a *toggle*
- $Q_{next} = J\bar Q+\bar KQ$
- [[JK flip-flop.png]]
## Flip-Flops with Preset and Clear
# Registers and Shift Registers
- operation of all flip-flops in a register is synchronized by a common clock
  -> data written and read from flip-flops at the same time
- 4-bit shift register consists of D flip-flops
	- each clock pulse will cause the transfer of the contents -> "right shift"
- values propagate through to the next gated latch 
  -> no control over the number of shifts during a single clock pulse
  ->> use master-slave or edge-triggered flip-flops
# Counters
- produce signals from high-frequency clock whose frequencies are submultiples of the original clock frequency -> functioning as a *scaler*
- constructed from T flip-flops -> two clock pulses will cause $Q_0$ to change from the 1 state to the 0 state and back to the 1 state
  -> $Q_0$ has half the frequency of the clock
  --> $Q_1$ has quarter frequency of the clock
- *ripple counter*: effect of an input clock pulse ripples through the counter
- [[3-bit up-counter.png]]
# Decoders
- circuit capable of accepting an $n$-variable input and generating the corresponding output signal on one out of $2^n$ output lines
- [[two-input to four-output decoder.png]]
- [[BCD to seven-segment display decoder.png]]
# Multiplexers
- any one of $n$ data inputs can be selected to appear as the output
- choice is governed by a set of "select" input
- [[four-input multiplexer.png]]
# Sequential Circuits
- output determined by its present inputs and on the sequence of previous inputs
- different *states* depending on what the sequence of inputs has been up to a given time
## Design of an up/down counter as a sequential circuit
- *state diagram*: graph where states are represented as cirlces and transitions between states are indicated by values of the input $x$ that will cause this particular transition to occur
	- *state table* used as a different way of presenting the information
		- indicates transition from all present states to the *next states*
	- ![[State assignment example up-down counter.png|500]]
	- $Y_2 = \bar y_2y_1\bar x + y_2\bar y_1\bar x + \bar y_2 \bar y_1 x + y_2y_1x = y_2 \oplus y_1 \oplus x$
	  $Y_1 = \bar y_2\bar y_1\bar x + y_2\bar y_1\bar x + \bar y_2 \bar y_1 x + y_2\bar y_1x = \bar y_1$
	- ![[Implementation of the up-down counter.png|500]]
## Timing Diagrams
- ![[Timing diagram for the up-down counter circuit.png|500]]
- *synchronous sequential circuits*: essential features of circuits controlled by a clock
## Finite State Machine Model
- time delay through the delay elements is equal to the duration of the clock cycle
- combinational logic has no delay
- Inputs to the combinational logic block are outputs of flip-flops
  Outputs of the block are inputs to the flip-flops
- Every clock cycle the new $Y_i$ values are transferred to $y_i$
```ad-info
collapse: none
flip-flops consitute a feedback path from the output to the input of the combinational block introducing a delay of one clock period
```
## Synthesis of Finit State Machines
```ad-info
title: Design process of synchronous sequentail circuits
1. Develop an appropriate state diagram or stable table
2. Determine the number of flip-flops needed, and choose a suitable type fo flip-flop
3. Determine the values to be stored in these flip-flops for each state in the state diagram. This is referred to as state assignemnt
4. Develop the state-assigned state table
5. Derive the next-state logic expressions needed to control the inputs of the flip-flops. Also, derive the expresisons for the outputs of the circuit.
6. Use the derived expressions to implement the circuit.
```

---
References: