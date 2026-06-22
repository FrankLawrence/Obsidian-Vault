---
Tags: 
Created: 2026-06-09 23:26:53
---
(Links:: [[Advanced Computer Architecture]])

Binary incompatibility revolves around a piece of code having been compiled for a type of hardware, and being able to run that same code unmodified and still profit from micro-architectural improvements of the new target. A simpler approach is to fix it in software, but this does not give us the performance of running it natively.
There are two known approaches:
- **Emulation**: Instruction by instruction simulation of the source architecture
- **Static Translation**: Conversion from machine code of the source architecture to the machine code of the target architecture

Emulation involves "reversing" the binary code into the instructions of the *source architecture*, and then having equivalent functions in code (such as C) and recompiled into the new architecture (we literally emulate the entire processor). This gives *poor performance*.
Static translation is synonymous to having a *fitting translation from source architecture instruction to target architecture instruction*:

| Source              | Target       |
| ------------------- | ------------ |
| `add $a1, $a1, $v1` | `ADD BX, CX` |
| `addi $a1, $v0, 12` | `ADD BX, 12` |
| `mul $v1, $v1, $a0` | `MUL EX, DX` |

> [!caution] Difficulties of static translation
> - **Code identification**: all code must be discovered statically and separated from embedded data (impossible in large programs to enumerate all control flow paths)
> - **Self-modifying code**: What to do with it? Additional hardware to allow support of source architecture?
> - **Precise Exceptions**: no 1-to-1 relation between target instructions and source ones
> - **OS**: Support of shared libraries and system calls
> 
> Static translation will never work!

The best idea is a hybrid approach: **Merge emulation and translation to get the best of both worlds**. We start by emulating while *exploring the code* and *profiling*. We then *translate and optimise frequently used code*. We do this by grouping related code (such as basic blocks) and try to translate these iteratively.

![[DBT Execution Flow.svg|500]]

Typical optimisations to translate code include regular compiler optimizations:
- ILP scheduling (data and control speculation)
- Loop unrolling
- Alias analysis
- Load-store telescoping
- Copy propagation
- Combining
- Unification
- Limited dead-code elimination

On a DBT (dynamic binary translation) system, there is an additional component that lives under the [[Operating System]] and above the processor: **DBT Engine**. Note, because it does not have access to "outside" instructions, it cannot use instructions such as `malloc` for memory used during translation, and must therefore have it's own memory structures. Take the Transmeta Crusoe TM5400 processor as example:

![[Transmeta Crusoe.svg|600]]

> [!caution] Difficulties of DBT
> - Self-modifying code
> 	- We can mark the `code` section as read-only in the [[Translation Lookaside Buffer|TLB]]!
> 	- When exception is raised, we can switch to emulating
> - Precise exceptions
> 	- Asynchronous exceptions are not a problem, as we can delay it until the end of a group and invoke a translated exception handler
> 	- Synchronous exceptions can be emulated...
> 		- When encountering this exception during translation, revert to beginning of current translated group, re-emulate source architecture to find the exact point of the exception and invoke translated exception handler
> 	- Reverting to previous state requires *own memory* (*shadow registers* and *store buffers*)
> 		- Shadow Registers: Get the value of the main registers at the end of a group
> 		- Gated Store Buffer: Holds pending stores for commit at the end of a group
> - Address translations, aliasing
> - Self-referential code
> - Management of translation cache
> - Real-time code
> - Boot code

**Additional Optimisations in DBT**:
- *Block Reordering*: Make target image execution as sequential as possible
- *Memory Colouring*: Improve mapping of translated code to fit target memory heirarchy
- *Code Specialization*: Clone procedures based on constant parameter values

**Benefits of DBT**:
- Compatibility with native implementations and across different VLIWs sizes
- Reliability and possibilities to upgrade
	- Software patches for bugs in translator
	- Software patches for optimiser enhancements
	- Translator can be used to hide hardware bugs
- Low hardware cost -> fast in-order implementation
- Higher instruction-level parallelism -> Dynamic groups can be made arbitrarily large
- Low-power consumption (memory consumes less than logic?)

> [!caution] Issues with DBT
> - Reduced resources for the user
> 	- Cycles: lost performance for translation
> 	- Memory
> - Slow at start (emulation) and real-time difficulties
> - Debugging difficulties
> 	- Target machine code far removed from source code
> 	- Non-deterministic behaviour of real-systems

> [!question] Open problems of DBT
> - Can a DBT VLIW machine be ever any better than a well-conceived superscalar?
> - Better light-weight optimisations possible?
> - Real-time problems solvable?
> - Which translation cache management policy is best?
> - Target architecture every exposed to users?

---
References:
- [[1. Dynamic Binary Translation.pdf]]