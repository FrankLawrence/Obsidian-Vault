---
Tags: 
Created: 2023-10-03 00:28:59
Links: "[[Secure Programming]]"
---
(Links:: [[Secure Programming]])
# Static Analysis
Static analysis creates a **control flow** and **data flow** through the program, but it doesn't test inputs so it cannot find everything.

> [!tip]- Always compile with `-Wall` and `-Werror`
> - Compiler warns about possible bugs found with static analysis
> - Compilation does not complete until they are fixed
# Dynamic Analysis
- Compiler can also add instrumentation
	- Attempts to find bugs at runtime
	- Example: insert a check on every array access to determine whether it is within bounds
	- More effective than static analysis, but slows down program
# Source Control
- history helps investigate problems
- Reduces the risk of mistakes merging changes
- Allows finding unintended changes


---
References: