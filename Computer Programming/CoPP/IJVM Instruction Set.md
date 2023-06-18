| Hex    | Mnemonic             | Meaning                                                 |
| ------ | -------------------- | ------------------------------------------------------- |
| `0x10` | BIPUSH *byte*        | Push byte onto stack                                    |
| `0x59` | DUP                  | Copy top word on stack and push onto stack              |
| `0xA7` | GOTO *offset*        | Unconditional branch                                    |
| `0x60` | IADD                 | Pop two words from stack; push their sum                |
| `0x7E` | IAND                 | Pop two words from stack; push Boolean AND              |
| `0x99` | IFEQ *offset*        | Pop word from stack and branch if it is zero            |
| `0x9B` | IFLT *offset*        | Pop word from stack and branch if it is less than zero  |
| `0x9F` | IF_ICMPEQ *offset*   | Pop two words from stack; branch if equal               |
| `0x84` | IINC *varnum const*  | Add a constant to a local variable                      |
| `0x15` | ILOAD *varnum*       | Push local variable onto stack                          |
| `0xB6` | INVOKEVIRTUAL *disp* | Invoke a method                                         |
| `0x80` | IOR                  | Pop two words from stack; push Boolean OR               |
| `0xAC` | IRETURN              | Return from method with integer value                   |
| `0x36` | ISTORE *varnum*      | Pop word from stack and store in local variable         |
| `0x64` | ISUB                 | Pop two words from stack; push their difference         |
| `0x13` | LDC_W *index*        | Push constant from constant pool onto stack             |
| `0x00` | NOP                  | Do nothing                                              |
| `0x57` | POP                  | Delete word on top of stack                             |
| `0x5F` | SWAP                 | Swap the two words on the stack                         |
| `0xC4` | WIDE                 | Prefix instruction; next instruction has a 16-bit index |

- *byte*, *const*, and *varnum* are 1 byte
- *disp*, *index*, and *offset* are 2 bytes