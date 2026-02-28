---
Tags: #lecture 
Created: 2026-01-19 22:10:27
---
(Links:: [[System Security]])
> [!info] Testing Software for Bugs
> - no analysis (e.g., random testing)  
> - lightweight analysis using coverage (e.g., grey-box fuzzing)
> - heavyweight analysis using path conditions (e.g., symbolic execution)

Types of fuzzing: 
- random
- grab & mutate
- grammar-based
# Grey-box Fuzzing

- Guide input generation toward a goal  
	- Guidance based on lightweight program analysis
- Main steps  
	- Randomly generate inputs  
	- Get feedback from test executions
- What code is covered?  
	- Mutate inputs that have covered new code
## American Fuzzy Loop (AFL)
- Uses code coverage feedback
- Prioritize mutations that increase code coverage
- Inputs e.g., files read by the program
# Measuring Coverage
- Different coverage metrics  
    - Line/statement/branch/path coverage
- Here: Branch coverage
- Branches between basic blocks
- Rationale: Reaching a code location not enough to trigger a bug, but state also matters
- Compromise between  
    - Effort spent on measuring coverage • Guidance it provides to the fuzzer
## Efficient Implementation
Instrumentation added at branching points:
```
cur_location = /*COMPILE_TIME_RANDOM*/; 
shared_mem[cur_location ˆ prev_location]++; 
prev_location = cur_location >> 1;
```
- Globally reachable memory location that stores how often each edge was covered
- Combine previous and current block into a fixed-size hash
- Shift to distinguish between “A” followed by “B” from “B” followed by “A”
## Detecting New Behaviours
- Inputs that trigger a new edge in the CFG: Considered as new behaviour
- Alternative: Consider new paths; More expensive to track; Path explosion problem
## Edge Hit Counts
- Refinement of the previous definition of “new behaviours”
- For each edge, count how often it is taken
- Approximate counts based on buckets of increasing size 1, 2, 3, 4-7, 8-15, 16-31, etc.
- Rationale: Focus on relevant differences in the hit counts
## Mutation Operators
- **Goal**: Create new inputs from existing inputs
- Random transformations of bytes in an existing input 
- Bit flips with varying lengths and stepovers  
- Addition and subtraction of small integers
- Insertion of known interesting integers
# Dynamic Symbolic Execution
- The Main Idea: Analyze One Execution Path!
	- Run the program under one concrete input
	- Collect the values of all variables at each executed statement. This information often called an “execution trace”.
	- Mark certain inputs as symbolic
	- Track the relationship between variables in the execution trace and symbolic inputs as a formula
	- At symbolic branch conditions, assert that the condition evaluates to the value in the execution trace  
	    (e.g. constraint C1 evaluates to “true” on the example next slide)
	- Calculate Symbolic Formula for path constraints:  
	    The logical conjunction of all the symbolic constraints
- **No false positives**
# Sanitizers
- Sanitizers instrument the binary during compilation
- Detect violation at runtime
- **Example**: if a fuzz input triggers memory corruption, the sanitizer reports it and terminates the program

> [!question] How does ASAN work?
> - Replace malloc and free implementations
> - Regions surrounding malloc-ed memory and free-ed memory are poisoned
> - Memory accesses to poisoned memory force a crash
> - Poisoning is implemented with shadow memory

- If compiler allocates *more memory than was requested*, address sanitizer does *not catch* access to the “over-allocated” memory
- **Undefined Behavior Sanitizer** checks for various heuristics:
	- Bitwise shifts out-of-bound
	- Integer overflow
	- Dereferencing misaligned pointers
- Fuzzers use *branch coverage* to explore program space, but doesn't perform well when *specific values* are required (e.g. magic bytes)

## ASAN Shadow Memory
- 8 bytes of program memory map to one byte of shadow memory
	- 8 bytes poisoned -> Negative shadow value
	- 0 bytes poisoned -> 0
	- First $K$ bytes are unpoisoned -> $K$
- Insert redzones between different buffers

![[ASAN Shadow Memory.png|700]]

---
References: