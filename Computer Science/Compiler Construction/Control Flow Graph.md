---
Tags: 
Created: 2025-01-27 05:14:51
---
(Links:: [[Compiler Construction]])
A **control flow graph** is a representation, using graph notation, of all paths that might be traversed through a program during its execution.
- **Control flow analysis** aims to determine the execution order of program statements or instructions
- **Basic Block**: A linear sequence of program instructions having one entry point (the first instruction executed) and one exit point (the last instruction executed)
- **Control flow graph (CGF)** is a directed graph in which the nodes represent basic blocks and the edges represent control flow paths
	- A Control flow graph specifies all possible execution paths

> [!example]- CFG with basic blocks
> ![[CFG and Basic Blocks.canvas|CFG and Basic Blocks]]

> [!info] Some refer to CFG when they mean ICFG
> - CFG: representing control flow for a single procedure 
> - ICFG: representing control flow for a program

We say a node `d` of a CFG **dominates** node `n` if *every* path from the entry node of the graph to `n` passes through `d` (`d dom n`).
- Every node `n` dominates itself.

> [!example]-
> ```mermaid
> flowchart LR
> 	entry[entry] --> a[1]
> 	a --> b[2] --> c[3] --> d[4] --> e[5] --> g[7] --> h[8] --> i[9]
> 	a --> c
> 	d --> c
> 	d --> f[6] --> g --> d
> 	h --> j[10] --> g
> 	h --> c
> 	i --> a
> ```
> | Block | Dom            | IDom |
> | ----- | -------------- | ---- |
> | 1     | {1}            | -    |
> | 2     | {1,2}          | 1    |
> | 3     | {1,3}          | 1    |
> | 4     | {1,3,4}        | 3    |
> | 5     | {1,3,4,5}      | 4    |
> | 6     | {1,3,4,6}      | 4    |
> | 7     | {1,3,4,7}      | 4    |
> | 8     | {1,3,4,7,8}    | 7    |
> | 9     | {1,3,4,7,8,9}  | 8    |
> | 10    | {1,3,4,7,8,10} | 8    |

- A node `d` *strictly* dominates a node `n` if `d` dominates `n` and `d != n` (`d sdom n`).
- Node `d` of a CFG post dominates node `n` if every path from `n` to the exit node passes through `d` (`d pdom n`)
- Every node post dominates itself
- Each node n (except exit node) has unique immediate post dominator

> [!example]-
> ```mermaid
> flowchart LR
> 	a[1] --> b[2] --> c[3] --> d[4] --> e[5] --> g[7] --> h[8] --> i[9]
> 	a --> c
> 	d --> c
> 	d --> f[6] --> g --> d
> 	h --> j[10] --> g
> 	h --> c
> 	i --> a
> 	j --> k[exit]
> ```
> | Block | Pdom                | IPdom |
> | ----- | ------------------- | ----- |
> | 1     | {3,4,7,8,10,exit}   | 3     |
> | 2     | {2,3,4,7,8,10,exit} | 3     |
> | 3     | {3,4,7,8,10,exit}   | 4     |
> | 4     | {4,7,8,10,exit}     | 7     |
> | 5     | {5,7,8,10,exit}     | 7     |
> | 6     | {6,7,8,10,exit}     | 7     |
> | 7     | {7,8,10,exit}       | 8     |
> | 8     | {8,10,exit}         | 10    |
> | 9     | {1,3,4,7,8,10,exit} | 1     |
> | 10    | {10,exit}           | exit  |

In a depth-first traversal of a tree, we go to an unvisited node. If there are none left, backtrack to your parent. In this context a **retreating edge** $m \to n$ connects a descendent $m$ to its ancestor $n$. A **cross edge** is between to nodes, neither of which are an ancestor of each other.
- **Back edge**: ancestor dominates its descendent
- A flow graph is **reducible** if every retreating edge in a flow graph is a back edge
	- take any DFST for the flow graph, remove the back edges, the result should be acyclic

> [!example]- Irreducible graph
> ```mermaid
> flowchart LR
> 	A[A] --> B[B] 
> 	A --> C[C]
> 	B --> C --> B
> ```
> - Either B --> C or C --> B will be retreating edges. These are not back edges since the descendents are not dominated by the other.

The **natural loop** of a back edge $b \to h$ is the set of nodes $x$ such that $h$ dominates $x$ and there is a path from $x$ to $b$ not containing $h$.
> [!example]-
> ![[Not a Natural loop.canvas|Not a Natural loop]]

An *inner loop* is a loop that contains no other loops. Loops an either be disjoint or completely nested within the other.
# Front-end Transformations
- **Goal**: convert linear structure of input program into hierarchical structure (and perhaps then to a lower-level IR)
- **Input**: source program  
- **Output**: abstract syntax tree (+ symbol table) → lower level IR
## Optimization
- **Goal**: perform high-level analysis and optimization of program  
- **Input**: E.g., AST (+ symbol table) from front-end  
- **Output**: Low-level program representation such as 3-address code (TAC)
- **Tasks**:
	- Procedure/method inlining
	- Array dependence analysis
	- Loop transformations: unrolling, permutation, tiling, jamming, distribution

To optimize on the AST level, we can create larger basic blocks (reduces jumps). For this we convert the AST into a directed acyclic graph to track dependencies.
> [!info] AST to dependency graph
> - Replace arcs by downward arrows
> - Insert data dependencies from use of variable $V$ to preceding assignment to $V$
> - Insert data dependencies between consecutive assignments to $V$
> - Add roots to the graph (output variables)
> - Remove ;-nodes and connecting arrows
## Desugaring
Some syntax features are just syntactic sugar. Example:
`for (i=0; i<n; i++){do_something();}` -> `int i=0; while(i<n){do_something(); i = i + 1;}`
## Context Analysis
- We should bind variables (and function calls) to the appropriate definitions
- To determine this, we must apply the language's scoping rules
- For every new "scope" we encounter, push a scoping level **lookup table** on a **stack**
- Every **definition** in that scope is added to the lookup table on the top of the stack
- Every **use** (of a variable or function) is looked up in the stack of scoping levels (just as you wood in normal computer architecutre)
## Type checking
Apply the language's various typing rules:
- The operand type that we can use with certain operators
- The number of arguments to a function call
- The types of these arguments
- The types of assigned variables versus the variable type
- The use of void in place of other function return (not allowed)
# Intermediate Code Generation
- Allows language-independent, machine-independent optimizations and transformations

> [!question] What makes a good IR?
> - Easy to translate from AST
> - Easy to translate to assembly
> - Narrow interface: small number of node types (instructions)

- Some optimizations require high-level structure, others low-level code
  -> Create multiple IR stages
- Some optimizations are targeted for a specific *computer architecture*

As mentioned, usually there are two IRs:
- One high-level IR that is language independent but is closer to a language
- One low-level Ir that is machine independent but is close to machine code

## High-Level IR
- High-level intermediate representation is essentially the AST
	- Must be expressive for all input languages
- Preserves high-level language constructs
	- Structured control flow: if, while, for, switch, etc.
	- variables, methods
- Allows high-level optimizations based on properties of source language (e.g. [[LLVM IR Optimizations#Function Inlining]])
## Low-Level IR
- Low-level representation is essentially an *abstract machine*
- Has low-level constructs
	- Unstructured jumps, instructions
- Allows optimizations specific to these constructs (e.g. register allocation, branch prediction)
- Alternatives for low-level IR:
	- *Three-address code* or *quadruples*: `a = b OP c`
	  -> easier dataflow analysis
	- *Tree representation*
	  -> easier instruction selection
	- *Stack machine* (like Java bytecode)
	  -> easier to generate
### Low IR Instructions
- Assignment instructions:
	- Binary operations: `a = b OP c`
		- Arithmetic, logic, comparisons
	- Unary operation `a = OP b`
		- Arithmetic, logic
	- Copy instruction: `a = b`
	- Load/store: `a = *b, *a = b`
	- Other data movement instructions
- Flow of control instructions:
	- `label L`: lavel instruction
	- `jump L`: Unconditional jump
	- `cjump a L`: Conditional jump
- Function call
	- `call f(a1, ..., an)`
	- `a = call f(a1, ..., an)`
	- Is an extension to quads
- IR describes the Instruction Set of an abstract machine
### Temporary Variables
- The operands in the quadruples can be:
	- Program variables
	- Integer constants
	- Temporary variables
- *Temporary variables* = new locations
	- Use temporary variables to store intermediate values
### Arithmetic / Logic Instructions
- Abstract machine supports a variety of different operations
- Arithmetic operations: ADD, SUB, DIV, MUL
- Logic operations: AND, OR, XOR
- Comparisons: EQ, NEQ, LE, LEQ, GE, GEQ
- Unary operations: MINUS, NEG
### Data Movement
- Copy instruction: `a=b`
- Load/store instructions: `a = *b` `*a = b`
- Address-of instruction: `a = &b`
- Array accesses: `a = b[i]` `a[i] = b`
- Field accesses: `a = b.f` `a.f = b`
### Branch Instructions
- Label instruction: `label L`
- Unconditional jump: go to statement after label `L` : `jump L`
- Conditional jump: test condition variable `a`; if true, jump to label `L`: `cjump a L`
- Alternative: two conditional jumps: `tjump a L` `fjump a L`
### Call Instruction
- Supports function call statements `call f(a1,...,an)`
- ... and function call assignments: `a = call f(a1,...an)`
- No explicit representation of argument passing, stack frame setup, etc.

---
References: