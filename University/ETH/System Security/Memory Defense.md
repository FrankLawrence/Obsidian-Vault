---
Tags: #lecture 
Created: 2026-01-19 02:42:39
---
(Links:: [[System Security]])
# Non-executable Data  (DEP)
- set regions of memory non-executable using `NX` bit
- **Prevents foreign code injection**
# Software Fault Isolation (SFI)
- Goal: **Fault Isolation**
	- Confine read/write to certain region M
	- This goal is also called "address sandboxing"
- Compiler instruments the changes in the program
- Seperate verifier used to check if resulting program is ok
	- Smaller "Trusted Computing Base" (TCB)
	  > "The trusted codebase for ensuring security properties"
- Attacker can still *jump* passed the checks (on fast SFI implementation)
	- operate only on new reserved register, instead of the original one
# Control Flow Integrity (CFI)
- Follow the statically configured Control flow graph (CFG) at runtime
- Some implementations do not make use of *directionality*, but just check if there is an edge between two functions
- Functions may have several callers -> hard to keep track which one to return to
	- **Shadow stack** prevents return address from being tampered with: `ret` instruction is converted into 
	  ```
	  mov  %reg, -0x1000000[%esp]
	  jmp  [%reg]
	  ```
		- Store return addresses on hidden/secret stack
		- store on both main stack and shadow stack, and compare for equality at function return
		- Where to put it in the virtual address space? 
		- How to stop the attacker from corrupting the shadow stack? 
		- Store retrieve from the shadow stack impacts performance 
		- Size of the shadow stack?
- **Randomized tag**: attacker can't jump, unless the value is matching
	- Created when program loaded in memory -> attacker should not be able to read memory!
	- Assumption: Attacker cannot modify shadow stack
# Address Space Layout Randomization (ASLR)
- At load time, randomize base pointer of stack, bss, code, etc.
- Higher entropy leads to lower likelihood that attacker can guess target
## Instruction set randomization (ISR)
- **Goal**: Randomize machine instrusction encoding to defeat *foreign code injection*
- Program opcodes are xor'ed with secret key stored in CPU to get actual opcode ($D3 \oplus K = C3 \;(\text{return opcode})$)
## Data Space Randomization (DSR)
- **Goal**: Randomize data representation to defeat *data corruption*
![[74310.png|600]]
- Also uses a secret key in the CPU
# Stack Canaries
# Guard Pages
- Placed around critical memory regions, so that *contiguous* memory read/writes from the attacker are stopped -> attacker has to write directly to the address
# Hardware Implementations
## Intel Memory Protection Keys (MPK)
- CPU contains Permission Register (PKRU)
- Tag memory pages with PKEYs
- PKRU dictates which tags the CPU may access
	- If privilege is escalated, and other memory pages are trying to be accessed, this will be blocked
## Pointer Authentication in ARMv8.3-A
General purpose hardware primitive approximating pointer integrity • Adds Pointer Authentication Code (PAC) into unused bits of pointer 
- Keyed, tweakable MAC from pointer address and 64-bit modifier
- PA keys protected by hardware, modifier decided where pointer created and used
- **Prevents arbitrary pointer injection**
- Modifiers do not need to be confidential  
- Visible or inferable from the code section / binary 
- Keys are protected by hardware and set by kernel 
- Attacker cannot generate PACs

---
References: