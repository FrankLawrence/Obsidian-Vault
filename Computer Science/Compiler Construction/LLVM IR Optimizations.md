---
Tags: 
Created: 2025-01-27 17:04:53
---
(Links:: [[Compiler Construction]])
- Different kinds:
	- Space optimizations: improve (reduce) memory use
	- Time optimization: improve (reduce) execution time
- Code transformations must be safe!
	- They must preserve the meaning of the program
	- Conservative behavior: leave unoptimized if we can't prove correctness
- High-level languages sometimes make optimizations inconvenient or impossible to express (e.g. `a[i][j] = a[i][j] + 1;`)
- High-level unoptimized code may be more readable, cleaner, modular (e.g. `int square(x) {return x*x; }`)

> [!note] When to apply optimizations
> # High IR
> - [[#Function Inlining]]
> - [[#Function Cloning]]
> # Low IR
> - [[#Constant Folding]] / [[#Constant Propagation]]
> - [[#Copy Propagation]]
> - [[#Algebraic Simplification]]
> - [[#Common Subexpression Elimination]]
> - [[#Unreachable Code Elimination]] / [[#Dead Code Elimination]]
> - [[#Strength Reduction]]
> - [[#Loop Optimizations]]
> 	- Loop-invariant code motion
> 	- Induction variable elimination
> 	- Loop unrolling
> # Assembly
> - Instruction selection
> - Peephole optimizations
> - Register allocation

## Function Inlining
- Replace a function call with the body of the function
```c
int g(int x) { return f(x)-1; }
int f(int n) { int b=1; while (n--) { b=2*b; } return b; }
```
___
```c
int g(int x) { int r;
			   int n = x;
			   { int b=1; while (n--) { b=2*b; } r-b; }
			   return r - 1; }
```
- Can inline some indirect calls via function pointers, but more difficult
## Function Cloning
- Create specialized versions of functions that are called from different call sites with different arguments:
```c
 void f(int x[], int n, int m) {
	 for(int i=0; i<n; i++) { x[i] = x[i] + i*m; }
}
```
___
Create a specialized version for calls with `f(a, 10, 1)`:
```c
 void f(int x[]) {
	 for(int i=0; i<10; i++) { x[i] = x[i] + i; }
}
```
## Constant Folding
- Evaluate an expression if operands are known at compile time (i.e. they are constants): `x = 1.1 * 2;` => `x = 2.2`
- These might become possible after other types of optimizations
## Constant Propagation
- If value of variable is known to be a constant, replace use of variable with constant: 
  ```c
  n = 10;
  c = 2;
  for (i=0; i<n; i++) { s = s + i*c; }
  ```
  Replace all occurrences of `n` and `c` with 10 and 2
- What if previously variable value becomes constant after propagation?
## Copy Propagation
- After assignment `x = y`, replace uses of `x` with `y`
- Replace until `x` is assigned again:
```c
x = y;
if (x > 1)
s = x * f(x - 1);
```
___
```c
x = y;
if (y > 1)
s = y * f(y - 1);
```
- If there was an assignment `y = z`, then transitively apply replacements
## Algebraic Simplification
- More general form of constant folding: take advantage of usual simplification rules
  ```c
  a * 1 => a
  a * 0 => 0
  a / 1 => a
  a + 0 => a
  b || false => b
  b && true => b
  ```
## Common Subexpression Elimination
- If program computes the same expression multiple times, we an reuse the computed value

```c
a = b+c
c = b+c
d = b+c
```
___
```c
a = b+c
c = a
d = b+c
```
- Common subexpressions also occur in low-level code in address calculations for array accesses: `a[i] = b[i] + 1;`

## Unreachable Code Elimination
- Eliminate code that is never executed
```c
#define debug false
s = 1;
if (debug)
	print("state = ", s);
```
___
```c
s = 1;
```
- Also works for loops where condition is always `false` (loop not executed):
  ```c
  while (false) S; => ;
  while (2>3) S; => ;
  ```
## Dead Code Elimination
- If effect of statement is never observed, eliminate the statement:
```c
x = y + 1;
y = 1;
x = 2 * z;
```
___
```c
y = 1;
x = 2 * z;
```
- Variable is *dead* if value is never used after assignment
- Eliminate assignments to dead variables
## Strength Reduction
- Replaces costly operations (multiple, divides) by cheap ones (adds, subs)
```c
x * 2 => x + x
i * 2^c => i << c
i / 2^c => i >> c
```
- More effective when operated on *induction variables* (loop variables whose value depends linearly on the iteration number):
```c
s = 0;
for (i = 0; i < n; i++) {
	v = 4 * i;
	s = s + v;
}
```
___
```c
s = 0; v = -4;
for (i = 0; i < n; i++) {
	v = v + 4;
	s = s + v;
}
```
# Loop Optimizations
- Loop optimizations are important, effective and numerous (most of the time of execution is spent in loops)

## Loop-invariant code motion
- If the result of a statement/expression does not change during loop and it has no externally-visible side effects, we can hoist it out of the loop
- Often useful for array element addressing computations
	- Invariant code not visible at source level

```c
for(i=0; i<n; i++)
	a[i] = a[i] + (x*x)/(y*y);
```
___
```c
c = (x*x)/(y*y)
for(i=0; i<n; i++)
	a[i] = a[i] + c;
```
## Induction variable elimination
- If there are multiple induction variables in a loop, we can eliminate the ones that are used only in the test condition
- Need to rewrite test condition using the other induction variables
```c
s = 0; v = -4;
for (i = 0; i < n; i++) {
	v = v + 4;
	s = s + v;
}
```
___
```c
s = 0; v = -4;
for (; v < (4*n-4);) {
	v = v + 4;
	s = s + v;
}
```
## Loop unrolling
- Execute loop body multiple times at each iteration
```c
for (i = 0; i < n; i++) { S }
```
___
```c
for (i = 0; i < n-3; i+=4) { S; S; S; S; }
```

- Gets rid of $\frac{3}{4}$ of conditional branches!
- Space-time tradeoff: program size increases

# Order
- **simple dependencies**: (optimization creates opportunity for another)
  e.g. constant/copy propagation and dead code elimination
- **Cyclic dependencies**: (iteratively apply different optimizations)
  e.g. constant folding and constant propagation
- **Adverse interactions**: (optimization interferes with another)
  e.g. common subexpression elimination and register allocation

# Implementing Optimizations
To be able to implement some optimizations, we need information about the program which we get through CFGs:
- Each instruction has two program points (before and after)
- Inside a basic block, the point after an instruction is the same as the point before it's successor instruction
- Basic blocks may have $N$ predecessor/successor program points
- We then ask ourselves: 1. What is the effect of an instruction 2. What is the effect of control flow?
## Liveness Analysis
- Used for [[#Unreachable Code Elimination]] / [[#Dead Code Elimination]]
- `in[I]` = live variables at program point before `I` (Instruction)
- `out[I]` = live variables at program point after `I`
- `in[B]` = live variables at beginning of `B` (Basicblock)
	- same as `in[I]` for first instruction `I`
- `out[B]` = live variables at end of `B`
	- same as `out[I]` for last instruction `I`

To find dead variables we find the **relation** between `in[I]` and `out[I]` as well as `in[B]` and `out[B]`. We work **backwards** with `out[I]` and compute `in[I]`.
- `in[I] = (out[I] - def[I]) ∪ use[I]`
	- Each variable live after `I` is also live before `I`, unless `I` write it
	- Each variable that `I` reads is also live before instruction `I` 
	- `def[I]` = variables defined (written) by instruction I (e.g., x | x=y)
	- `use[I]` = variables used (read) by instruction I (e.g., y,z | x = y+z)

> [!example]

- A variable is live at end of block `B` if it is live at the beginning of one (or more) successor blocks
	- characterizes all possible program executions
- `out[B] = ∪ in[B_s], B_s in succ(B)`

> [!example]-
> ![[Pasted image 20250128182857.png|500]]
> ![[Pasted image 20250128183946.png|500]]
## Copy Propagation
- determine copies available at each program point
- `in[I]` = copies available at program point before `I` (Instruction)
- `out[I]` = copies available at program point after `I`
- `in[B]` = copies available at beginning of `B` (Basicblock)
	- same as `in[I]` for first instruction `I`
- `out[B]` = copies available at end of `B`
	- same as `out[I]` for last instruction `I`

We worked forward, where we compute `out[I]` with `in[I]`:
- `out[I] = (in[I] - kill[I]) u gen[I]`
	- Keep all copies from `in[I]`, except copies `<u=v>` if variable `u` or `v` is written by `I`
	- If `I` is of the form `x=y`, add it to `out[I]`
	- `kill[I]` = copies "killed" by instruction `I` (e.g. `x=y+z` -> `<x=*>`, `<*=x>`)
	- `gen[I]` = copies "generated" by instruction `I` (e.g. `x=y` -> `<x=y>`)
- A copy is available at beginning of block `B` if it is available at the end of all predecessor blocks
- `in[B] = ∩ out[B_p], B_p in pred(B)`

> [!example]
> ![[Pasted image 20250128191050.png|500]]
> ![[Pasted image 20250128191102.png|500]]
> Fixed point reached!
> - The program reach for example copy `z=t` but not `x=y`

# Dataflow Analysis
In general, like the previous examples, we can
- define some information that they need to compute
- Build constraints for the information
- Solve constraints iteratively

and with this, we can check for different types of things. DFA defines a *transfer function* `F(I)` for each instruction `I` in the program (describes how the instruction modifies the information). Then
- **Forward analysis**: `out[I] = F(in[I])`
- **Backward analysis**: `in[I] = F(out[I])`

DFA uses meet operators at split/join points between basic blocks in the CFG. Consider `in[B]` is information at beginning of block `B` and `out[B]` is information at end of `B`: $$in[B] = \sqcap \{out[B_{p}] | B_{p}\in pred(B)\}$$ $$out[B] = \sqcap \{in[B_{s}] | B_{s}\in succ(B)\}$$
- Intersection: $\sqcap = \cap$
- Unoin: $\sqcap = \cup$

## Data-flow Equations
- Transfer functions and meet operators define data-flow equations
- We solve equations via an iterative algorithm, i.e., for the forward version: 
	- For each instruction I, initialize in[I] to start value  
	- Repeatedly apply rules, stop when fixed point reached
- Efficient solution uses a worklist algorithm:
    - Keep list of instructions to evaluate
    - Initialize list to the set of all instructions
    - If out[I] changes after evaluating out[I] = F(in[I]), then add all successors of I to the worklist
- Intuition: only re-evaluate program points where information has changed

> [!example] Liveness Analysis
> - backward analysis, start is $\varnothing$ at all points
> - *Transfer function*: **$F(X) = (X - def[I]) \cup use[I]$**
>   ”Variables live before instruction I include: (1) variables live after I, but not written by I, and (2) variables used by I”
> - *Meet operator*: **Union**
>   “A variable is live at the end of a basic block B if it is live at the beginning of one of its successor blocks”)

> [!example] Reaching definition
> - forward analysis, start is $\varnothing$
> - *Transfer function*: **$F(X) = (X - kill[I] \cup gen[I])$**
>   “Reaching defs after instruction I include: (1) reaching defs before I, but not killed by I, and (2) reaching defs generated by I”
> - *Meet operator*: **Union**
>   “A definition reaches the entry of block B if it reaches the exit of at least one of its predecessor nodes”

To speed up data-flow information propagation , we can skip some CFG subgraphs based on whether the subgraph uses a previously defined variable.
We often also want to reuse previously computed data-flow representations across multiple analyses (for speedup) and can use **definition-use-** and **use-definition chains**:
- Definition-Use chains (DU chains): “Definition of a variable and all the uses, reachable from that definition without any other intervening definitions.”
	- For each definition `D` compute a chain (list) of uses that `D` may reach
- Use-definition chains (UD chains): “Use of a variable, and all definitions of that variable that can reach that use without any other intervening definitions”
	- For each use `U` compute a chain (list) of definition that reach `U`
- Properties
	- Sparse representations of data flow
	- Compute information only at the program points where actually used!
	- No need for CFG represenstataion (with info at each program point)
	- Must re-formulate analysis algorithms using DU chains

# Static Single Assignment
- Problem: Need to compute DU/UD chains and keep them around for analyses – DU/UD chains still need significant space for large programs
- Idea: rewrite program to explicitly express the DU/UD relation in the code
- SSA form: Each variable defined only once
- UD relation: for each use of a variable, there is a unique definition of that variable
- DU relation: for each definition of a variable, there may be multiple uses of that definition
- Results in an implicit representation of the DU/UD relation!

---
References: