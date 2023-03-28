---
Tags: 
Created: 2023-03-06 19:07:04
---
(Links:: [[Computer Organization]])
# Some Fundamental Concepts
- processor fetches one instruction at a time and performs the operation
- instructions are fetched from successive memory locations until a branch or jump instruction is encounterd
- processor uses *program counter* to keep track of the address of the next instruction
  After fetch, PC is updated
- instruction is placed in *instruction register*: interpreted, or decoded by control circuitry
- *instruction fetch phase*: fetch an instruction and load it into the IR
	- $IR\leftarrow [[PC]]$
		- Fetch contents of the memory location pointed to by the PC
		- contents of this location are the instruction to be executed -> loaded into IR
	- $PC \leftarrow [PC]+4$
		- increment PC to point to next instruction (for 32-bit computer)
	- carry out operation specified by instruction in the IR
 
> [!summary] Operations specified by an instruction
> - Read the contents of a given memory location and load them into a processor register
> - Read the data from one or more processor registers
> - Perform an arithmetic or logic operation and place the result into a processor register
> - Store data from a processor register into a given memory location
 
- [[Main hardware components of a processor]]
- **A five-step sequence of actions to fetch and execute an instruction**
	1. Fetch an instruction and increment the program counter
	2. Decode the instruction and read registers from the register file
	3. Perform an ALU operation
	4. Read or write memory data if the instruction involves a memory operand
	5. Write the result into the destination register, if needed
# Hardware Components
## Register File
- array of storage elements, with access circuitry that enables data to be read from or written into any register
- two registers are able to be read at the same time
- [[Single memory block.png]]
## ALU
- performs arithmetic operations (addition and subtraction) and logic operations (AND, OR, and XOR)
- contents of two registers specified in the instruction read and become output A and B -> input to ALU InA and multiplexer MuxB -> chooses output of register file or immediate value in IR
- ouput connected to data input to save into destination register
## Datapath
- Two phases of instruction processing: fetch and execution
	- fetching: decoding and generating the control signals that cause appropriate actions in execution section
	- execution: read the data operands specified in an insturction, performs the required computations, and stores the results
- [[A five-stage organization.canvas]]
- actions taken in each stage are completed in one clock cycle
- information in IR generates control signals for subsequent steps -> holds instruction for entire execution
- [[Datapath in a processor.png]]
	- Load and Store instructions
		- effective address of memory operand computed by ALU in step 3
		- Loading reads data from memory selected by multiplexer MuxY to be transferred to register file in next clock cylce
		- Storing reads data from register file (stage 2) and stored in RB to RM (step 3) and into memory (stage 4)
## Instruction Fetch Section
- [[Instruction fetch section of Five-stage organization.png]]
- address to access memory come from PC (fetching instructions) or RZ (accessing instruction operands)
- instruction address generator updates the contents of the PC after each instruction is fetched
- instruction read from the memory is loaded into the IR
	- control circuitry generates signals to control hardware
	- immediate value used either directly as an operand or to compute the effective address of an operand
- address generator increments PC by 4 during straight-line execution or computes new value when executing branch and subroutine call instructions
	- [[Instruction address generator.png]]
	- output of register RA needed when executing subroutine linkage instructions
	- PC-Temp holds contents of PC during process of saving the subroutine or interrupt return address
# Instruction Fetch and Execution Steps
- [[Sequence of actions needed to fetch and execute instruction examples]]
- [[Instruction encoding.png]]
- memory cannot be accessed in one clock cycle -> cache memory can be read or written to in one clock cycle
- processor reads source register before entire instruction can be decoded -> source register address uses same bit position 
- two registers are always read, even though 2 might not be needed
## Branching
- branch or subroutine call instruction loads new address into PC
- subroutine calls save return address 
- target address is specified relative to PC
- space is needed within instruction to specify the OP code and branch condition -> smaller range of addresses accessable
- subroutine calls don't include condition
- Sequence of actions needed to fetch and execute an unconditional branch instruction
	1. Memory address <- [PC], Read memory, IR <- Memory data, PC <- [PC] + 4
	2. Decode instruction
	3. PC <- [PC] + Branch offset
	4. No action
	5. No action
- Sequence of actions needed to fetch and execute the instruction: `Branch_if_[R5]=[R6] LOOP`
	1. Memory address <- [PC], Read memory, IR <- Memory data, PC <- [PC] + 4
	2. Decode instruction, RA <- [R5], RB <- [R6]
	3. Compare [RA] to [RB], If [RA] = [RB], then PC <- [PC] + Branch offset
	4. No action
	5. No action
- address of the subroutine may be computed using immediate value given in the instruction or given by general-purpose register
- `Call_Register R9`: call subroutine with address in R9 -> read and placed in RA -> MuxPC set to 0 input -> data in R9 loaded into PC
- Sequence of actions needed to fetch and execute the instruction: `Call_Register R9`
	1. Memory address <- [PC], Read memory, IR <- Memory data, PC <- [PC] + 4
	2. Decode instruction, RA <- [R9]
	3. PC-Temp <- [PC], PC <- [RA]
	4. RY <- [PC-Temp]
	5. Register LINK <- [RY]
- previous address (return address) in PC saved in temporary Register PC-Temp before return address can be saved in register LINK in register file
## Waiting for Memory
- when requested information is not in the cache and has to be fetched from the main memory, several clock cycles may be needed
- processor-memory interface circuit generates signal called Memory Function Completed (MFC) -> signal on memory Read or Write
	- processor's control circuitry checks for signal to determine when to proceed to next step -> when data is found in cache, assert MFC signal before end of clock cycle -> uninterrupted
	- access main memory -> interface delays asserting MFC -> processor's control circuitry extends duration
	- Step 1 of execution sequence requires memory fetch:
	  Memory address <- [PC], Read memory, Wait for MFC,
	  IR <- Memorydata, PC <- [PC]+4
# Control Signals
- components governed by *control signals*
- results of the actions that take place in one stage are stored in inter-stage registers, to be available for use by the next stage in the next clock cycle
- data selected by the multiplexer matters only during that step of the stage (ex: MuxB and MuxY)-> same selection can be maintained in all execution steps -> less circuitry
- [[Control signals for the datapath.png]]
	- multiplexers are controlled by signals that select which input data appear at the multiplexer's output
	- ALU performs operation determined by *k*-bit control code: ALU_op (Add, Subtract, AND,...)
	- comparator generates condition signals needed for conditional branch instructions
- [[Processor-memory interface and IR control signals.png]]
	- MEM_read and MEM_write initiate read or write operation -> interface asserts MFC signal
	- IR_enable only activated after MFC signal is asserted
	- immediate value has three formats:
		- sign-extended 16-bit value
		- zero-extended 16-bit value
		- 26-bit value
- [[Control signals for the instruction address generator.png]]
# Hardwired Control
- an instruction is executed in a sequence of steps, where each step requires one clock cycle
- the setting of the control signals depends on:
	- contents of the step counter
	- contents of the instruction register
	- the result of a computation or a comparison operation
	- external input signals, such as interrupt requests
- [[Generation of the control signals.png]]
	- instruction decoder interprets OP-code and addressing mode information
	- outputs T1 to T5 indicate which step involved in fetching and executing instructions is being carried out
## Datapath Control Signals
- instruction is loaded into IR and decoded to determine actions needed
- source registers are read and contents become available at outputs A and B
- data flows automatically from one datapath stage to the next on every active edge of the clock signal
- **control signals can be determined by examining the actions taken in each execution step of every instruction**
	- logic expression for RF_write signal in step T5: 
	  
	  $$\text{RF}_{\text{write}}=\text{T5}\times(ALU+Load+Call)$$
	  -> [[Control signals for the datapath.png]]
		- ALU: all instructions that perform arithmetic or logic operations
		- Load: all load instructions
		- Call: all subroutine-call and software-interrupt instructions
	- multiplexer's signal can be implemented as a function of the instruction only: $$\text{B}_{\text{select}}=\text{Immediate}$$ -> [[Control signals for the datapath.png]]
		- Immediate: all instructions that use an immediate value in the IR
## Dealing with Memory Delay
- control signal WMFC executed at any step where Wait for MFC command is issued
- Counter_enable should be 1 when WMFC is not asserted or when MFC is asserted: 
  
  $$\text{Counter}_{\text{enable}}=\overline{\text{WMFC}}+\text{MFC}$$
  -> [[Generation of the control signals.png]]
- PC enabled only when MFC is recieved when fetching new instruction
  PC enabled when branching in step 3
  -> $$\text{PC}_{\text{enabled}}=\text{T1}\times\text{MFC}+\text{T3}\times\text{BR}$$
  -> [[Control signals for the instruction address generator.png]]
# Cisc-Style Processors
- [[Organization of CISC-style processor.png]]
- Interconnect block does not prescribe any particular structure or pattern of data flow
- inter-stage registers not needed -> registers hold intermediate results (temporary registers)
- *bus*: set of lines to which several devices may be connected -> data transfer between devices
- *bus driver*: logic gate that sends a signal over bus
- only one device is driving the bus at any time -> *tri-state gate*: 
	- when turned on, the gate places a logic signal of 0 or 1 on the bus, depending on input value
	- when turned off, gate is electronically disconnected
- data register can be connected to bus line with two control signals, $R_{in}$ and $R_{out}$
	- [[Input and output gating for one register bit.png]]
## An Interconnect using Buses
- [[Three-bus CISC-style processor organization.png]]
- Sequence of actions needed to fetch and execute the instruction: `Add R5, R6`
	1. Memory address <- [PC], Read memory, Wait for MFC, IR <- Memory data, PC <- [PC] + 4
	2. Decode instruction
	3. R5 <- [R5] + [R6]
	- data is read from source registers in step 2
	- data is sent over buses A and B, added together by the ALU, and sent back over bus C
- Sequence of actions needed to fetch and execute the instruction: `And X(R7), R9`
	1. Memory address <- [PC], Read memory, Wait for MFC, IR <- Memory data, PC <- [PC] + 4
	2. Decode instruction
	3. Memory address <- [PC], Read memory, Wait for MFC, Temp1 <- Memory data, PC <- [PC] + 4
	4. Temp2 <- [Temp1] + [R7]
	5. Memory address <- [Temp2], Read memory, Wait for MFC, Temp1 <- [Memory data]
	6. Temp1 <- [Temp1] AND [R9]
	7. Memory address <- [Temp2], Memory data <- [Temp1], Write memory, Wait for MFC
## Microprogrammed Control
- *hardwired control*: signals generated by ciruits that interpret the content of IR and timing signals from step counter
- *micorprogram*:
	- stored on processor chip in small and fast memory *countrol store*
	- desired setting of control signlas in each step determined by microprogram
- control signals consist of one bit -> one *n*-bit *control word* (*microinstruction*) stores *n* control signals at once
- one control word is stored in the microprogram memory for each step in execution sequence of an instruction
- reading instruction requires MEM_read and WMFC signals -> corresponding bits in the control word for signals are set to 1 in steps 1, 3 and 5 
- *machineroutine*: sequence of microinstructions for an instruction like `ADD R5, R6`
- [[Microprogrammed control unit organization.png]]
	- microinstruction address generator generates address to access specific instruction in control store
	- MicroinstrAddrGen. decodes IR instruction to obtain address for corresponding microroutine -> address loaded into $\mu$PC
	- $\mu$PC increments during execution
	- microinstruction `END` marks last microinstruction and causes new machine instruction to be fetched
- **Microinstructions direct the actions of the main processor's hardware components, by indicating which control signals need to be active during each execution step** (control processor within main processor)
- more flexible but slower than hardwired control

---
References: