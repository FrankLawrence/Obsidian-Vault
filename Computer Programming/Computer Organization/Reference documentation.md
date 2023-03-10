---
Tags: 
Created: 2023-02-26 21:48:53
---
(Links:: [[Computer Organization]])
# Programming consturcts
## Conditional branching
### If statement
Pseudocode:
```cpp
if(RAX > 1) {
	//IF-code
} else {
	//ELSE-code
}
```
Implementation:
```
	cmpq $1, %rax   # compare RAX to 1
	jg ifcode       # jump to IF-code if RAX > 1
	jmp elsecode    # jump to ElSE-code otherwise
ifcode:
	...
	jmp end
elsecode:
	...
end:
```
- result of comparison stored in special **`RFLAGS`** register
- `jg`: conditional branch instruction
- conditional branch instructions test contents of **`RFLAGS`** register and jumps depending on instruction
## If-then-else statement
Implementation
```
	cmpq $1, %rax
	jg ifcode
elsecode:
	...
	jmp end
ifcode:
	...
end:
```
## While/do-while/for loops
### do-while-loop
```cpp
do {
	//loop code
} while(RAX > 1)
```
```
loop:
	...

	cmpq $1, %rax
	jg loop
```
### while-loop
```cpp
while(RAX > 1) {
	//loop code
}
```
```
loop:
	cmpq $1, %rax  # if RAX <= 1 jump to
	jle end        # the end of the loop

	...            # loop code

	jmp loop       # repeat the loop
end:
```
### for-loop
```cpp
for(RAX = 0; RAX < 100; RAX++) {
	//loop code
}
```
```
RAX = 0;
while(RAX < 100) {
	//loop code
	RAX++;
}
```
## Switch-case statements
# Subroutines
- if we want to execute or call a subroutine, we jump to its first instruction
- subroutines return to the first instruction after the subroutine call -> address is pushed onto the stack right before making the jump
- by convention arguments to subroutines are placed in register:
	- `%RDI`
	- `%RSI`
	- `%RDX`
	- `%RCX`
	- `%R8`
	- `%R9`
- more arguments are pushed in reverse order
- caller saved: preserve the input values of subroutines somewhere (stack)
- callee saved: register has same value they had when subroutine was executed: `%RBX`, `%RSP`, `%RBP`, `%R12`, `%R13`, `%R14`, `%R15`
- if you use the stack, then you need to pop off the arguments yourself after the call returns
- **the stack needs to remain 16-byte aligned**: `%rsp` must be a multiple of 16 when you do a call
	- all calls push 8 byte return address and old `%rbp` is pushed -> aligned regardless if stack isn't used
- subroutines return value in `RAX` register
## Writing your own subroutines
- stack pointer register `RSP`: contains address of first byter after program's memory space -> stack is empty
- stack *grows* downward into program's memory space
- when `push` instruction is executed, the value in the stack pointer register gets decremented by some amount and the pushed value is stored at the new location at which the stack pointer then points.
## Cleaning up the stack
- stack overflow: stack grows too large and overwrites program's code
- add amount of bytes to `%rsp` for amount pushed to stack
  Ex: `addq $8, %rsp` after a number has been pushed with `pushq`
## subroutine example
- subroutine is a block of instrucitons which has a *label*
	- pop the address off the stack and load it into the program coutner once it finishes
	- `ret` instruction pops an eight byte value off the satck and loads it into program counter
- stack pointer keeps moving during subroutine
- base pointer register `RBP`:
	- opon entry of subroutine, we immediately push current value of `RBP` onto the stack
	- copy current stack pointer value to `RBP` 
	- `RBP` will not change during subroutine
	- pop old `RBP` off the stack and return from subroutine
- *subroutine prologue*: storing `RBP` and copying stack pointer 
# Input and output
## Printing to the terminal
- system C library
```
mystring: .asciz "Hello world\n"

	movq $0, %rax               # no vector registers in use for printf
	leaq mytring(%rip), %rdi    # load address of a string
	call printf                 # Call the printf routine
```
- we don't provide entire string as an argument, we load the memory address of the first character of the string
- we can embed special character sequences in our string and pass extra values to `printf`
## Reading from the terminal
- supply at least two arguments to `scanf`
	1. *format string* containing a number of special character sequences
	2. memory addresses at which `scanf` may put the read values
```
formatstr: .asciz "%ld"
	...
	subq $8, %rsp                # Reserve stack space for variable
	leaq $8(%rbp), %rsi          # Load address of stack var in rsi
	leaq formatstr(%rip), %rdi   # Load first argument of scanf
	movq $0, %rax                # no vector registers for scanf
	call scanf                   # Call scanf
```



---
References: