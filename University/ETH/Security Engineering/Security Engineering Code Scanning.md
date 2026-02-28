---
Tags: 
Created: 2025-12-16 19:05:27
---
(Links:: )
# Motivation  
![[Pasted image 20251216190643.png|500]]
## Problem Categories
- Input validation and representation
	- Buffer overflows, XSS, injection attacks, etc.  
- API abuse: abuse contract between caller and callee
	- Provide wrong input or make too strong assumptions about output 
- Security features: e.g., authorization engine like Cedar
	- ≠ Secure features, e.g., endpoint implementation does not call Cedar
- Time and state: e.g., race condition example  
- Error handling: handling errors poorly or not at all  
- Code quality: dereference null pointers, infinite loops, etc. 
- Encapsulation: lack of it
- Environment: problems outside of the source code
# Code scanning and security
## Static Analysis
- **Function call graph**
	- A call graph represents *potential* control flow between functions or methods.
	- Nodes in the graph represent functions, and directed edges represent the potential for one function to invoke another
	- What if there is an edge connect to itself?
	- A function can be a part of a function call graph, event if the analysis does not have access to its implementation
	- **Limitation** - e.g., function pointers
- **Control flow graph**
	- The nodes in a control flow graph are basic blocks, and directed edges connect the basic blocks
	- Basic blocks: sequences of instructions that will always be executed starting at the first instruction and continuing to the last instruction
	- Edges: potential control flow path between basic blocks. Back edges in a control flow graph represent potential loops.
	- **Limitation** - Inter-procedural analysis, e.g. function pointer
- **Symbolic execution**
	- Form of analysis where many possible executions of a system are considered simultaneously.
	- Treat input symbolically
	- Use control flow graph
	- Execute the statements
	- Build a tree of feasible paths
	- Propagate symbolic facts about the program in the nodes of the tree (path constraints)
	- Useful to evaluate some assertions statically, generate tests
	- **Limitation** - Path explosion
- **Taint analysis**
	- Taint propagation rules:
	    - *Source* and *Entry point* rules define program locations where tainted data enter the system
	    - *Sink* rules define program locations that should not receive tainted data
	    - *Pass-through* rule propagates or changes the taint
	    - *Cleanse* rule removes the taint
	- Given taint propagation rules
	    - Analyze the control flow graph
	    - Extract all traces (can be optimized)
	    - For each trace apply rules
			- Check and report if tainted data reached sinks
	- **Limitation** - Pointer analysis: typical problem of any data-flow analysis
# What is code scanning?
# Code scanning: Theory and practice
## Data Flow analysis
Determine how data moves through program  
- Traverse control flow graph and note where data generated and used
- Implementation trick: convert function to **Static Single Assignment**
	- Can assign to a variable only once, so make unique with indices 
	- Makes it trivial to determine where value comes from
- Constant propagation
	- If SSA variable assigned a constant, then replace it by constant
- Question: any security relevant applications of constant propagation? 
  Find hard-coded passwords or encryption keys!
# Bug-specific algorithms  
> [!question] How do you check properties?
> **Symbolic Execution**
> ![[Pasted image 20251216194320.png|500]]

> [!question] Which properties do you check?
> 1. Let programmer annotate program with assertions or write rules
> 2. Use standard set based on fault models or bug categories
> 3. Infer rules based on “consistency”  
> 	If a method is called 100 times and return value is used 99 times, then flag non-checking instance as suspicious
## Taint analysis rules
- **Source rules** define locations where tainted data enters system
- **Sink rules** define locations that should not receive tainted data
- **Pass-through** rules define how a function manipulates tainted data
  E.g., if input is tainted, output is similarly (or differently) tainted
- **Cleanse rule**: special pass-through rules that remove taint
  E.g., validation functions

- Taint is just an attribute on data. In practice not binary
	- Recall the binary 2Vars annotations
	- Input from `main()` may not be trustworthy, but guaranteed to be null terminated string
	- Data may have undergone certain checks
	- So represent taint by *different taint flags*  
- Sinks may be dangerous only when data has a certain taint  
- Rules can manipulate taint in either *additive* or *subtractive* manner 
- All of this must be specified, also for user-defined functions
	- Alternative: resort to binary taint, but with many false positives 
	- Engineering effort is most of the work
# Conclusions and practical experience
## Findbugs experience
> simple analysis combined with effective rules 

- Implementation strategies used by the detectors can be roughly categorized into:
- *Class structure and inheritance hierarchy only*. Some of the detectors simply look at the structure of the analyzed classes without looking at the code.
- *Linear code scan*. These detectors make a linear scan through the bytecode for the methods of analyzed classes, using the visited instructions to drive a state machine. These detectors do not make use of complete control flow information; however, heuristics (such as identifying the targets of branch instructions) can be effective in approximating control flow.
- *Control sensitive*. These detectors use an accurate control flow graph for analyzed methods.
- *Dataflow*. The most sophisticated detectors use analysis that takes both control and data flow into account. An example is the null pointer dereference detector.

---
References: