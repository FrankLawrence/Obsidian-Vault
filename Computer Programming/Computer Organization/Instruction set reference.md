| Mnemonic | Operands | Action                       | Description                      |
| -------- | -------- | ---------------------------- | -------------------------------- |
| mov.     | SRC,DST  | DST = SRC                    | Copy.                            |
| push.    | SRC      | %RSP -= 8, (%RSP) = SRC      | Push a value onto the stack.     |
| pop.     | DST      | DSAT = (%RSP) , %RSP += 8    | Pop a value from the stack       |
| xchg.    | A, B     | TMP = A, A = B, B = TMP      | Echange two values.              |
| movzb.   | SRC, DST | DST = SRC (one byte only)    | Move byte, zero extended.        |
| movzw.   | SRC, DST | DST = SRC (one word only)    | Move word, zero extended.        |
|          |          |                              | *Arithmetic*                     |
| add.     | SRC, DST | DST = DST + SRC              | Addition                         |
| sub.     | SRC, DST | DST = DST - SRC              | Subtraction.                     |
| inc.     | DST      | DST = DST + 1                | Increment by one.                |
| dec.     | DST      | DST = DST - 1                | Decrement by one.                |
| mul.     | SRC      | RDX:RAX = RAX * SRC          | Unsigned multiplication.         |
| imul.    | SRC      | RDX:RAX = RAX * SRC          | Signed multiplication.           |
| div.     | SRC      | RDX = RAX%SRC; RAX = RAX/SRC | Unsigned division.               |
| idiv.    | SRC      | RDX = RAX%SRC; RAX = RAX/SRC | Signed division.                 |
|          |          |                              | *Branching*                      |
| jmp      | ADDRESS  |                              | Jump to address (or label).      |
| je       | ADDRESS  |                              | Jump if equal                    |
| jne      | ADDRESS  |                              | Jump if not equal                |
| jg       | ADDRESS  |                              | Jump if greater than             |
| jge      | ADDRESS  |                              | Jump if greater or equal         |
| jl       | ADDRESS  |                              | Jump if less than                |
| jle      | ADDRESS  |                              | Jump if less or equal            |
| call     | ADDRESS  |                              | Jump and push return address     |
| loop     | ADDRESS  |                              | decl %rcx, jump if not zero      |
| ret      |          |                              | Pop address and jump to it.      |
|          |          |                              | *Logic and shift*                |
| cmp.     | A, B     | sub A B (Only set flags)     | Compare and set condition flags. |
| xor.     | SRC, DST | DST = SRC ^ DST              | Bitwise exclusive or.            |
| or.      | SRC, DST | DST = SRC \| DST             | Bitwise exclusive or.            |
| and.     | SRC, DST | DST = SRC & DST              | Bitwise and.                     |
| shl.     | A, DST   | DST = DST << A               | Shift left.                      |
| shr.     | A, DST   | DST = DST >> A               | Shift right.                     |
|          |          |                              | *Other*                          |
| lea.     | A, DST   | DST = &A                     | Load effective address.          |
| int      | INT_NR   |                              | Software interrupt.                                 |
