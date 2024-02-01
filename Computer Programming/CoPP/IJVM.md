---
Tags: 
Created: 2023-06-06 15:00:04
---
(Links:: [[CoPP]])
# [[Stack|Stacks]]
- reserved for variables
- register `LV` points to the base of the local variables for the **current procedure**
	- When a second procedure is called, `LV` is adjusted to point to the second local variables
- register `SP` points to the highest word
- **local variable frame**: data structure between `LV` and `SP`
- **operand stack**: stack holds operands during computation of an arithmetic expression
# IJVM Memory Model
- 4 GiB memory
## The Constant Pool
- cannot be written
- contains constants, strings, and points to other areas of memory
- `CPP` points to first word 
## The Local Variable Frame
- at the beginning are the parameters (arguments) with which the method was invoked
## The Operand Stack
- allocated directly above the local variable frame (and part of it)
- `SP` points to the top and changes during execution as operands are pushed onto the stack or popped off
## The Method Area
- "text" area
- `PC` register contains address of the instruction to be fetched next
- treated as a byte array
# [[IJVM Instruction Set]]


---
References: