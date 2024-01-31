---
Tags: 
Created: 2023-02-20 00:13:24
---
(Links:: [[Computer Organization]])
# Memory Organization
- memory is addressable using 32-bit addresses
- 8 bits: byte
  16 bits: word
  32 bits: doubleword
  64 bits: quadword
- [little-endian](https://en.wikipedia.org/wiki/Endianness) addressing used
# Register Structure
- 32-bit general-purpose registers for integer data operands or addressing information
- 8 additional regists for floating-point instructions
- more registers for vector-processing instructions
- *segmented memory model*: different areas (*segements*) of the memory with different usages
	- *code segement*: holds instructions of a program
	- *stack segment*: contains processor stack
	- 4 *data segments*: holding data operands
	- each segment identified by selector value in memory addresss space
- in *flat* memory model of IA-32: any memory location accessable through 32-bit address; segment registers point to address 0 in memory
- Instruction Pointer: program counter and contains address of next instruction to be executed
- Status register: holds condition code flags and program execution mode bits
	- condition code flags: contain info about results of arithmetic operations
	- program execution mode bits: associated with input/output operations and interrupts
# Addressing Modes
- basic addressing modes: Immediate, Absolute, Register, and Register indirect
- *Immediate mode*: The operand is contained in the instruction. It is a signed 8-bit or 32-bit number, with the length being specified by a bit in the OP code of the instruction. This bit is 0 for the short version and 1 for the long version
- *Direct mode*: The memory address of the operand is given by a 32-bit value in the instruction
- *Register mode*: The operand is contained in one of the eight general-purpose registers specified in the instruction
- *Register indirect mode*: The memory address of the operand is contained in one the eight general-purpose registers specified in the instruction
- *Base with displacement mode*: An 8-bit or 32-bit signed displacement and one of the eight general-purpose registers to be used as a base register are specified in the instruction. The effective address of the operand is the sum of the contents of the base register and the displacement.
- *Index with displacement mode*: A 32-bit signed displacement, one of the eight general- purpose registers to be used as an index register, and a scale factor of 1, 2, 4, or 8, are specified in the instruction. To obtain the effective address of the operand, the contents of the index register are multiplied by the scale factor and then added to the displacement.
- Instruction notation: $$\texttt{OP} \quad  \texttt{dst, src}$$
- [[IA-32 addressing modes]]
# Instructions
- 
# Assembler Directives
# Example Programs
# Interrupts and Exceptions
# Input/Output Examples
# Scalar Floating-Point Operations
# Multimedia Extension Operations
# Vector Floating-Point Operations


---
References: