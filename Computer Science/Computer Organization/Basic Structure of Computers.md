---
Tags: 
Created: 2023-02-06 00:44:36
---
(Links:: [[Computer Organization]])
# Computer Types
## Four categories
- **Embedded computers**: integrated into a larger device or system in order to autmotaically monitor and control a physical process or environment
  Used for a specific purpose
- **Personal computers:** dedicated individual use 
  Support a variety of applications: general computation, document preparations, copmuter-aided designn, audiovisual entertainment, interpersonal communication, internet browsing
- **Servers and Enterprise systems:** shared by potentially large number of users who access them from personal computer over public or private network.
  May host databeses and provide information processing for government agency or commercial organization
- **Supercomputers and Grid computers:** 
	- Supercomputers: used for highly demanding computations needed in weather forecasting, engineering design and simulation and scientific work.
	- Grid computers: more cost-effective alternative; combine large number of personal computers and dist storage units in physically distributed high-speed network;
	  high performance through distributed workload across gird
# Functional Units
## Main Parts
- input: accepts coded information from human operators
- memory: stores received information to be processed later
- arithmetic and logic: processing, steps stored in memory
- output: results sent back through control units
- *processor*: arithmetic and logic circuits
- *machine instructions*: commands that 
	- govern the transfer of information within a computer and between it's I/O devices
	- specify arithmetic and logic operations to be performed
- *program*: list of instructions which performs a task
- *Data*: numbers and characters that are used as operands by the instructions
- each instruciton, number, or character is encoded as a string of binary digits called *bits*
## Input Unit
- keyboard press results in corresponding letter or digit automatically being translated into corresponding binary code transmitted to processor
## Memory Unit
- **Primary memory**: programs must be stored in this memory while they are being executed
- semiconductor storage cells store one bit
	- handled in groups of fixed size *words*
	- one word can be stored or retrieved in one basic operation
	- *word length*: bits in each word (16, 32, or 64 bits)
- each word location is associated with an *address*
- accessed by specifying address for control command to start retrieval process
- *random-access memory*: memory in which any location ca be accessed in short and fixed amount of time
## Cache Memory
- used to hold sections of a program that are currently being executed, along with associate data
## Secondary Storage
- less expensive, permanent for large amount of data
- information is accessed infrequently
- access times for secondary storage are longer
## Arithmetic and Logic Unit (ALU)
- operands are brought into processor and operation is performed
- logic operations: addition, subtraction, multiplication, division, or comparison of numbers
- operands are stored in high-speed storage: *registers*
## Output Unit
- send processed results to the outside world
  -> dual role of graphic units -> *input/output*
## Control Unit
- coordinates operations of information processing, and input/output 
- sends control signals to other units and senses their states
- control circuits generate *timing signals* and determine when actions takes place
- controls data transfer between processor and memory
- [[Operation of a computer]]
# Basic Operational Concepts
- individual instructions are brought from the memory into the processor which executes the specified operations
  Ex: `LOAD R2,LOC`: Read contents of memory location
- transfers between memory and processor are initiated by sending the address of the desired memory location to the memory unit and asserting the appropriate control signals
- ![[Connection between the processor and the main memory.excalidraw]]
	- *Instruction register*: hold the instruction that is currently being executed; output available to control circuits
	- *Program counter*: contains memory address of next instruction to be fetched and executed
- devices may raise *interrupt* signal -> processocer provides requested service by saving state to memory and executing program *interrupt-service routine*
# Number Representation and Arithmetic Operations
## Integers
- leftmost bit is 0 for positive numbers and 1 for negative numbers
- [[Binary, signed-integer representations]] 

> [!definition] $n$-bit signed numbers using 2's-complement representation system
> > [!definition]+ Addition
> > add their $n$-bit representations, ignoring carry-out bit from most significatn bit position
> > the sum will be the algebraically correct value in 2's-complement representation if the actual result is in the range $-2^{n-1}$ through $+2^{n-1}-1$
> 
> > [!definition]+ Subtraction
> > from 2's complement of $Y$, add it to $X$ using the *add* rule. Again, the result will be the algebraically correct value in 2's-complement representation if the actual result is in the range $-2^{n-1}$ through $+2^{n-1}-1$

- *arithmetic overflow*: answers do not fall within the representable range
- *2's-complement*: form bit complement of the number and add 1
### Sign Extension
To represent a signed number in 2's-complement form using a larger number of bits, repeat the sign bit as many times as needed to the left
### Overflow in Integer Arithmetic
- examining the signs of the two summands and the sign of the result
- when both summands have the same sign, an overflow has occured when the sign of the sum is not the same as the signs of the summands
## Floating-Point numbers
- position of the binary point is variable and is automatically adjusted as computation proceeds -> binary point is said to *float*
- $-1.0341 \times 10^2$
	- 5 *significant digits* of precision
	- *scale factors* $10^2$ indicate actual position in respect to significant digits
 
> [!summary] Representation of binary floating-point number
>  - a sign for the number
>  - some significant bits
>  - a signed scale factor exponent for an implied base of 2
 
 - [[Single precision 32-bit basic IEEE format.png]]
# Character Representation
- encoding in ASCII (American Standard Code for Information Interchange)
- highest-order bit is set to 0
- alphabetic and numeric characters are in increasing sequential order when interpreted as unsigned binary numbers
# Performance
- performance is affected by the compiler that translates programs into machine language
- increase performance by performing a number of operations in parallel
	- the next instruction could be fetched from memory at the same time that an arithmetic operation is being performed on the register operands of the current instruction (*pipelining*)
	- mulitcore processors and multiprocessors (*shared-memory multiprocessor*)
# Historical Perspective
## The First Generation
- programs and their data located in the same memory
- Assembly language prepared programs
- vacuum tube technology to implement logic functions
- I/O functions performed by devices similar to typewriters
- magnetic core memories and magnetic tape storage devices were developed
## The Second Generation
- transitor invented and replaced vacuum tube
- magnetic disk storage devices developed
- high-level languages to make preparation of application programs easier -> compilers developed 
## The Third Generation
- fabricate many transistors on a single silicon chip (Texas Instruments)
- integrated-circuit memories 
- introduction of microprogramming, parallelism and pipelining
- cache and virtual memories developed
## The Fourth Generation
- large sections of main memory of small computers could be implemented on single chips
- integration of multiple processors and cache memories on a single chip
- processot, memory, and I/O circuits on a single chip
- notebook computers, mobile telephones interconnected by wired or wireless local area network
- parallelism and hierarchical memories evolved

---
References: