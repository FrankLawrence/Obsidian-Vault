---
Tags: #lecture 
Created: 2026-01-14 01:31:42
---
(Links:: [[System Security]])
# Basics
We know (from [[Computer Organization]]) that the CPU uses a runtime [[Stack]] to store local variables, function arguments, control information (such as return address) and that it *grows down* into smaller memory addresses. On each new function call, a new stack frame is created.
# Buffer Overflows (Spatial Memory Error)
# Integer Overflow (Type Error)
# User-after-free & Double Free (Temporal Memory Error)
## Double Free: The Write-Anywhere Exploit
```
1. char *a = malloc(0x20);  
2. free(a);  
3. free(a);  
4. char *b = malloc(0x20);  
5. strcpy(b, "\x78\x56\x34\x12");  
6. malloc(0x20) /* This is yet another 'a', we can ignore this */  
7. char *controlled = malloc(0x20); /* This is in the location we want */
```
- Calling free twice → chunk is both allocated and free  
- Line 4: write into the use data of `b`, we're writing into the `fwd ptr` of `a`
- controlling `fwd ptr` → control where the next chunk gets allocated  
- Line 6: the next `malloc` will return a again.  
- Line 7: The `malloc` after that → an arbitrary write
## Heap Overflow
- Usually, the C library manages memory allocations 
- Stores each allocated block in a linked list  
- Can re-allocate a previously freed blocks  
- Requests the OS for pages when given pages are full
# Format String Bugs (Spatial Memory Error)
# Bad Casting (Advanced Type Errors)
- Background:
	- C++ class subtyping & binding
		- A pointer to class T’ can safely point to a superclass T 
			- AllfunctionsinclassTareimplementedinsubclassT’  
			- Upcastingissafe,downcastingisnot
# Code Injection
> [!definition] Code Injection
> A memory exploit that hijack control to jump to attacker’s data payload

**Requirements**: 
1. Write attack payload in memory
	- We can insert code into stack, data or bss regions
2. Have attack payload be executable
	- Some memory areas have DX/NX bit to mark "no-exec" but writeable
	- Code section should be executable
	- Stack rarely needs to be executable -> set NX
	- Heap can be non-execute, but only if there is no dynamic code generation
3. Divert control-flow to payload
	- We can corrupt code pointers and non-code pointers 
## Corruption Targets
X86 and ARM cannot distinguish a pointer from a non-pointer

> [!definition] Code pointer
> A memory address, the value of which is directly used as a control-flow transfer target (in machine code) under benign inputs

> [!definition] Code pointer corruption
> Forging the runtime value of a code pointer to be invalid!

> [!definition] Type Safety for Code Pointers
> - Enforce that code pointers point to code-segment only! 
> - Enforce that control transfers use code pointers

# Code Reuse
> [!definition] Code Reuse
> A memory exploit that hijacks control to jump to attacker controlled code address

---
References: