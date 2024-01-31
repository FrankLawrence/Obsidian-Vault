| Name                             | Assembler syntax         | Addressing function               |
| -------------------------------- | ------------------------ | --------------------------------- |
| Immediate                        | Value                    | Operand = Value                   |
| Direct                           | Location                 | EA = Location                     |
| Register                         | Reg                      | EA = Reg that is, Operand = [Reg] |
| Register indirect                | [Reg]                    | EA = [Reg]                        |
| Base with displacement           | [Reg + Disp]             | EA = [Reg] + Disp                 |
| Index with displacement          | [Reg * S + Disp]         | EA = [Reg] * S + Disp             |
| Base with index                  | [Reg1 + Reg2 * S]        | EA = [Reg1] + [Reg2] * S          |
| Base with index and displacement | [Reg1 + Reg2 * S + Disp] | EA = [Reg1] + [Reg2] * S + Disp   |
