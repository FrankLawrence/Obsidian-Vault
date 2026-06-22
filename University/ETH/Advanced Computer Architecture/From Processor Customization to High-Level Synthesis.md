---
Tags:
  - lecture
Created: 2026-05-11 22:36:57
---
(Links:: [[Advanced Computer Architecture]])
- Not every task requires the same physical hardware capabilities, some might even cause restrictions (such as power usage or size) -> leave out those things we don't need
- Create specialized hardware for different tasks
# Automatic Processor Customization
- Compiler as design tools: use feedback-loop (*compiler-in-the-loop*) to test different hardware configurations (ex. using new instruction sets, using different number of registers) for the use case
- **Instruction Set Extensions**: Create a new [[Execution Unit]] that is specific to your task, which will complete the computation faster than when using regular instructions (ex: create a dedicated multiplication unit, instead of adding multiple times)
	- Allows us to collapse a subset of direct acyclic graph nodes into a **single functional unit** (**AFU**)

> [!example] 
> The following example is code that computes a shift-and-add unsigned 8x8-bit multiplication
> ```c
> /* init */ 
> a <<= 8; 
> /* loop */ 
> for (i = 0; i < 8; i++) {
> 	if (a & 0x8000) { 
> 		a = (a << 1) + b; 
> 	} else { 
> 		a <<= 1; 
> 	} 
> };
> return a & 0xffff;
> ```
> 
> Using *software predication*, we can transform it into the following:
> ```c
> /* init */ 
> a <<= 8; 
> /* loop */ 
> for (i = 0; i < 8; i++) {
> 	p1 = - ((a & 0x8000) >> 15); 
> 	a = (a <<= 1;) + b & p1;
> };
> return a & 0xffff;
> ```
> In a regular pipeline, using *forwarding paths*, we can achieve one computation of a loop iteration in ~6 cycles. Note, in hardware, we can *ignore bitwise operations with constants*, since these computations can make use of *direct wiring*. As a result, we only need transistors (and in turn cycles for computation) for the the last `and` operation and the `addition` operation (using the ALU).
> In this example, we can now create a new execution unit, specifically for this shift-and-add multiplication instruction; the resulting [[DAG]] (directed acyclic graph) is the following:
> 
> This can be further optimized: using the associativity property of addition, we can transform the tree into one which is more *balanced*, resulting in the *log* of the path length. Alternatively, we can use a [[carry-save adder]], which runs in $O(1)$ [[Time Complexity|time complexity]].
> The resulting path has as a worst case of 3-4 clock cycle length.

Adding additional execution units, we can now transform different types of operations completely.

> [!question] Why is hardware better
> - Spatial computation: Use hardware for parallelism, instead of sequential scheduling with software
> - No time quantization: Allows for operation chaining (scheduling multiple execution units in one clock cycle)
> - Hardware is different
> 	- Constants may be propagated
> 	- Precision can be tuned (bit-width analysis)
> 	- Arithmetic components can be optimized

- *Automatic ISE Discovery*: There were attempts at create programs, which take as input a C program, and find (large, often complex and used often) subgraphs, for which custom execution units would be put into hardware

# Statically Scheduled High-Level Synthesis
- Use c/C++ specification to build hardware
- How does this compare to [[VLIW]] scheduling?
	- Exact resources are not fixed, but instead a constraint
	- Clock cycle may be constrained but is in general not fixed; pipelining is not fixed
	- No register file (which allows connecting everything to everything) but instead direct connections between multiplexers and data-paths
- Some problems may not have an immediate analogy in VLIWs: There may be cheaper ways to achieve lower latency
  Ex: the same computation can be achieved with less execution units in the same amount of time
  
> [!example] Finite Impulse Response
> - The array `shift_reg` is static and represents the last 4 samples of `x`
> - This could be in a function which receives a stream of `x` (the input signal) and produces at each call an element of `y` (the output signal)
> 
> $$y_k=\sum\limits_{i=0}^{3}c_{i}x_{k-1}$$
> ==TODO==

## Classic HLS and VLIW Compilation
- Both try to schedule *statically*, whilst reducing *control decisions*
- *Uncertainty*, such as execution latency and memory dependencies, pose limitations on both strategies
- In both cases, the *user* has to decide where and how to optimize, based on the user constraints -> not a trivial task
# Dynamically Scheduled High-Level Synthesis
- Classic HLS relies on static scheduling, which is dictated by compile-time information -> *limited parallelism* with limited information at compile-time

> [!warning] What traditional HLS does not do well
> - Uncertain memory accesses can cause fake [[Data Dependencies.svg|RAW dependencies]]; dynamic scheduling can serialize memory accesses on actual dependencies
> - Static scheduling is great for *embedded applications*, but terrible for general purpose computers (Out-of-Order superscalar processor perform better)
> 
> $$
> \begin{array}{c|c|c}
> & \text{Statically Scheduled} & \text{Dynamically Scheduled}\\
> \hline
> \text{Computer Architecture} & \text{VLIW Processors} & \text{Out-of-Order Superscalar Processors} \\
> \hline
> \text{High-Level Synthesis} & \text{Traditional HLS} & \text{???} \\
> \end{array}
> $$

- Instead of having *central static controller* triggering operations, have local decisions decide based on data availability via *communication between components* -> **Asynchronous circuits**
- This requires creating **dataflow circuits** from the program
	- *SELF (Synchronous Elastic Flow)* protocol:
		- Every component communicates via pair of *handshake signals*
		- Data propagated as soon as memory and control dependencies resolve
	- We keep all the standard components (ex. functional units, memory interface, buffer), except that the only execute when input is correct/given
	- We add additional components specific to dataflow control: fork, join, branch and merge
	- These elastic circuits are still **strictly synchronous** and perfectly adapted to traditional VLSI and [[FPGA]] flows
## Synthesising Dataflow Circuits
1. Create intermediate [[Control Flow Graph|graph representation]] of C program (via compiler framework)
2. Construct the datapath -> each operator corresponds to a functional unit
3. Implement control flow
	1. A Merge for each variable entering the BB
	2. A Branch for each variable exiting the BB
	3. A fork after every node with multiple successors

> [!example]
> ```c
> for (i=0; i<N; i++) {
> 	hist[x[i]] = hist[x[i]] + weight[i];
> }
> ```
## Buffers and Performance

- Adding buffers to the CFG does not change the circuit functionality, but only its performance
- Each combinational loop (one that does not restrict flow of tokens) in the circuit needs to contain at least on buffer
- **Backpressure from slow paths prevents pipelining** -> use FIFO buffer

## The problem with memory
- These circuits to not take RAW dependencies into account
- Traditional processor [[Load and Store Queues.svg|LSQs]] allocate memory instruction *in program order*
- Dataflow circuits have **no notion of program order**!

> [!info] Basic Idea
> We don't know which instruction is executed next at the end of a basic block (in the example, could either be next block or first instruction of the same block)
> An LSQ for dataflow circiuts whose only difference is in the *allocation policy*:
> - *Static knowledge* of memory accesses program order inside each basic block
> - *Dynamic knowledge* of the sequence of basic blocks *from the dataflow circuit*
> 
> ![[Pasted image 20260613143226.png|500]]

## Speculation
- Contain speculation in a region of the circuit delimited by special components
	- Issue speculative tokens (pieces of data which might or might not be correct)
	- Squash and replay in case of misspeculation
- **Components**
	- Speculator: Dataflow component which can, besides its standard functionality, also inject tokens before receiving any at its input(s)
	- Save units: input boundary of the speculative region; reissues when previous computation is squashed
		- On each path to any component that could combine the token with a speculative
		- As close as possible to the paths carrying speculative tokens
	- Commit units
		- Output boundary of the speculative region
		- Propagate speculative tokens that turn out to be correct, squash misspeculated data
		- on each path from the speculator to an exit point, a store unit, or the speculator
		- As far as possible from the speculator

==TODO: Not finished==

> [!summary] What to expect from dynamic HLS?
> - Two hopes derived from the VLIW vs. OoO analogy:
> 	- Significantly better performance in control dominated applications with poorly predictable memory accesses
> 	- Better out-of-the-box performance
> - The former is almost certain, the second less so
> - A major issue is the *hardware overhead* of supporting dynamic schedules
> 	- Probably tolerable for the bulk of the circuits
> 	- Yet, LSQs represent quite tangible overheads, esp. in FPGAs (but could be hardened there)
> - Probably statically-scheduled HLS remains the best choice for classic DSP-like applications

# Conclusions
- Customizable processors and high-level synthesis are promising techniques to *accelerate program execution through customized hardware*
- We may need more of this in a post-Moore (no transistor scaling) and post-Dennard (no power scaling) world
- Yet, all these techniques require *considerable manual work and expertise* (at least to obtain decent results)
- Current research tries both to improve the quality of HLS-generated circuits as well as moving further up the level of abstraction to extract more easily more parallelism (e.g. domain-specific languages)

---
References: