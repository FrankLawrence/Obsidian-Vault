---
Tags: 
Created: 2023-03-22 22:42:40
---
(Links:: [[Computer Organization]])
# Basic Concept - The Ideal Case
- [[Pipelined execution-the ideal case.png]]
- one instruction takes five cycles to complete its execution, instructions are completed at the rate of one per cycle
# Pipeline Organization
- information (register addresses, immediate data, operations) carried through the pipeline
	- information is held in *interstage buffers* 
- [[A five-stage pipeline.png]]

> [!important] Interstage Buffers
> - B1: feeds the decode stage with newly-fetched instruction
> - B2: feeds the compute stage with two operands read from register file/immediate value/PC
>   control signals move through the pipeline
> - B3: holds result of ALU operation (written into register file of memory)
> - B4: feeds write stage with value to be written into the register file

# Pipelining Issues
- [[Pipelined execution-the ideal case.png]]
- If the result of $I_j$ is destination register of $I_{j+1}$, then it is necessary to wait until the new value is written into the register by instruciton $I_j$
- instruction *stalled* in decode stage for three cycles
- *hazard*: condition that causes pipeline to stall
# Data Dependencies
> [!example]
> ```
> Add      R2, R3, #100
> Subtract R9, R2, #30
> ```
> - source register identifier from interstage buffer B1 (Subtract) compared with destination register identifier in interstage buffer B2 (Add)
> - Subtract instruction held in B1 during cycles 3 to 5
> - NOP instruction set in B2 does not modify memory or register file
> - NOP creates one clock cycle (*bubble*) that passes through stages to the end of the pipeline

## Operand Forwarding
- desired value is available at end of cycle 3 (After ALU operation)
- value loaded into RZ can be forwarded to where it is needed in cycle 4
- [[Modification of the datapath to support data forwarding from register RZ to ALU inputs.png]]
	- multiplexers at ALU input select either a value read from the register file in the normal manner, or the value available in register RZ
- to forward value in RY (stage 5 writing), to ALU (stage 3 compute), MuxA and MuxB require another input
## Handling Data Dependencies in Software
- compiler handles data dependencies by inserting explicit NOP instructions
	- same effect on execution time as a stall
- simplifies the hardware implementation of the pipeline
- code size increases and execution time is not reduced
- compiler can move useful instructions into the NOP slots
# Memory Delays
- memory access cause pipeline stalls
- [[Stall caused by a memory access delay for a Load instruction.png]]
	- a cache miss causes all subsequent instructions to be delayed
- [[Stall needed to enable forwarding for an instruction that follows a Load instruction.png]]
	- second instrction must wait till value from cache is retrieved in RY
	- one-cycle stall or inserting useful instruction after Load
# Branch Delays
- branch instructions alter sequence of execution
## Unconditional Branches
- Branching
	- instruction fetched in cycle 1
	- decoded in cycle 2
	- target address computed in cycle 3
	- program counter is updated and instruction fetched in cycle 4 -> two-cycle delay *branch penalty*
- reducing branch penalty requires branch target address to be computed earlier in the pipeline
- [[Instruction address generator.png]] must be modified:
	- second adder needed in decode stage to compute branch address
	- if instruction decoder decides there is a branch -> target address is ready before end of the cycle 
	- fetch target instruction in next cycle
## Conditional Branches
- comparator that tests branch condition can be moved to decode stage
	- conditional branch decision made at same time that the target address is determined
## The Branch Delay Slot
- *branch delay slot*: location that follows a branch instruction
- instruction in delay slot cannot be $I_{j+1}$, since it should be executed even when the branch is taken
- compiler attempts to find a suitable instruction to occupy the delay slot
- move an instruction preceding the branch instruction (only if the data dependencies are preserved)
- if an instruction is found, then there will be no branch penalty
- *delayed branching*: branching takes place one instruction later than it appears
## Branch Prediction
- second instruction is fetched while branch decision is decoded (cycle 2)
- decision for fetch instruction made when incrementing PC during fetching of first instruction (cycle 1)
### Static Branch Prediction
- the same choice is used every time a conditional branch is encountered (assume branch will not be taken) -> full branch penalty
- backward branch at end of a loop taken most of the time -> instructions fetched using branch target address as soon as known
### Dynamic Branch Prediction
- processor hardware keeps track of branch decisions every time a branch instruction is executed (use result of most recent execution of a branch instruction)
- the next time the instruction is executed, the branch decision is likely to be the same as the last time
- decision for branch instruction will always be the same except for the last pass through the loop
- repeated execution of the same loop results in mispredictions in the first pass and the last pass
- on last pass state will be changed from "Strongly likely to be taken" to "Likely to be taken" -> prediction correct at next encounter with loop -> only last pass mispredicted
### Branch Target Buffer for Dynamic Prediction
- branch prediction must take place in cycle 1 (same time branch instruction is fetched)
- branch target buffer identifies branch instructions by their addresses
- each branch instruction is executed -> address of instruction and outcome of branch decision recorded in buffer
- lookup table includes
	- address of the branch instruction
	- one or two state bits for the branch prediction algorithm
	- branch target address
- processor checks the branch target buffer for an entry containing the same instruction address -> use state bits to predict whether branch is likely to be taken
# Resource Limitations
- instruction stalls occur if there's only 1 cache that supports only one access per cycle
- Fetch stage accesses cache every cycle -> Load or Store instruction stalls fetch
- using separate caches for instructions and data allows fetch and memory stages to proceed simultaneously
# Performance Evaluation
- *basic performance equation*: execution time of a program has a dynamic instruction count of $N$ is given by $$T=\frac{N\times S}{R}$$
  $S$: average number of clock cycles to fetch and execute one instruction
  $R$: clock rate in cycles per second
- *instruction throughput*: number of instructions executed per second $$P_{np}=\frac RS$$
- idealy a new instruction is fetched every clock cycle -> through put with pipelining $$P_p=R$$
## Effects of Stalls and Penalties
- assume that Load instructions consitute 25% of the dynamic instruction count, and that 40% of these Load instructions are followed by a dependent instruction
	- increase over the ideal case of $S=1$: $$\delta_{\text{stall}}=0.25\times 0.40\times 1 =0.10$$
	- throughput reduced to: $$P_p=\frac{R}{1+\delta_{\text{stall}}}=\frac{R}{1.1}=0.91R$$
	- stall eliminated when nearby instruction is put between Load instruction and dependent instruction
- increase over the ideal case of $S=1$ due to cache misses: $$\delta_{\text{miss}}=(m_i+d\times m_d)\times p_m$$
  $m_i$: fraction of all instructions that are fetched incur cache miss
  $d$: fraction of all instructions are Load or Store instructions
  $m_d$: fraction of Load/Store inst. incur cache miss
  $p_m$: cycles stalled

## Number of Pipeline Stages
- Increasing the number of pipeline stages can improve instruction throughput, but also lead to more potential dependencies and branch penalties that reduce its benefits.
- ALU delay is an important factor that affects the processor clock cycle time.
- Using a pipelined ALU and a long pipeline (20+ stages) can aggressively reduce the cycle time and enable clock rates of several GHz.

# Superscalar Operation
- multiple execution units -> handle several instructions in parallel
- *multiple-issue*: several instructions started at the same time in different units
- *fetch unit*: fetches multiple instructions in one cycle
- *dipatch unit*: decodes instructions in queue and sends them to appropriate execution units
- [[A superscalar processor with two execution units.png]]
## Branches and Data Dependencies
- *speculative execution*: subsequent instructions based on an unconfirmed prediction are fetched, dispatched, and possibly executed but labeld as being speculative
- wait until validation
- on false prediction, fetch and dispatch correct instruction
- when an instruction is dispatched to an execution unti, it is buffered (in *reservation stations*) until all necessary results from other instructions have been generated
- reservation stations save result of execution unit and tag them
- other buffered reservation stations copy value for their own instruction
- compiler should space arithmetic and memory instructions apart so there is on data dependency
## Out-of-Order Execution
- *imprecise exceptions*: instruction causes exception (unaligned memory access), and succeeding instruction modifies destination register
- results of execution of instructions must be written into destination locations strictly in program order (first $I_j$ then $I_{j+1}$)
- results retained by execution unit itself or buffered in temporary register
## Execution Completion
- To improve performance, execution units should execute instructions with ready operands out-of-order but complete them in program order to allow precise exceptions
- Results are written into temporary registers and later transferred to permanent registers in correct program order during the commitment step
- *Register renaming* allows multiple temporary registers to be allocated for different permanent registers.
- The *commitment unit* guarantees in-order commitment using a separate queue called the *reorder buffer*
- Instructions are dispatched for execution in program order, and when completed, results are transferred from temporary to permanent registers and the instruction is removed from the queue
- Instructions complete execution out-of-order but are retired in program order

---
References: