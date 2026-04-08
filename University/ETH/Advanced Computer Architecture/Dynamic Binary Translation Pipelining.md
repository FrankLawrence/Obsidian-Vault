---
Tags: 
Created: 2026-03-18 16:43:46
---
(Links:: [[Advanced Computer Architecture]])

The first step to increasing *speed* of computers, is through *instruction level pipelining* (*ILP*). To the end, an upper bound on the speed we can achieve is the speed through the **critical path** (a path through a combinational circuit, built from logic gates); aka. the fastest you can travel from start to finish in a circuit. This stems mainly from the [[Kondensator & Kapazität|discharge of capacitors]]. 
For a circuit to classify an input as 0, the voltage on that input must drop low enough for it to be identified as 0.
Some limitations to pipelining are **data- & control-dependencies**, where the next instruction to be executed relies on a value in the previous one, leading to wasted time.

## Scope for parallelism is limited
- **Control hazards** limit the usability of the pipeline
	- Must squash fetched and decoded instruction following a branch 
- **Data hazards** limit the usability of the pipeline
	- Whenever the next instruction cannot be executed, the pipeline is stalled and no new useful work is done until the “problem” is solved (e.g., cache miss)
- **Rigid sequencing**  
	- Special “slots” for everything even if sometimes useless (e.g., MEM before WB)  
	- Every instruction must be coerced to the same framework 
	- Structural hazards avoided “by construction”

![[Simple Pipeline with Forwarding.svg|700]]
- By adding paths from the output of the intermediate cycles, to the execution cycle, we can skip later steps
- We can *execute* the different instructions **out of program order**, and piece them back in the correct order using a [[#Reorder Buffer|ROB]]
	- We continue fetching and decoding even and especially if one cannot execute previous instructions
	- Keep writeback ("commit") waiting if there is a structural hazard, without slowing down execution

![[Dynamically Scheduled Processor.svg|630]]

## Reservation Stations
*R*ead-*A*ter-*W*rite data hazards are prevented using **reservation stations** (RS). Instructions are added (*no ordering*, like parking lot), and the reservation station inserts operands by checking when they become available. *Tags* are inserted as placeholders for operands that await the result from previous instructions. It checks that the Execution Unit is free (Structural Hazard), and then starts execution.

![[Reservation Station.svg|600]]

> [!info]- WAW and WAR data dependencies
> ![[Data Dependencies.svg|800]]
> Dynamic scheduling creates two problems: *Write-after-write* and *write-after-read* dependencies

WAW and WAR are *name dependencies*, and can easily be fixed by simply avoiding the use of the same name. We **rename the destination register** whenever a **new value is created** (implicit register renaming). Unavailable operands are identified by the name of the reservation station in charge of the originating instruction.

![[Superscalar Processor.svg|700]]

Exception handlers should know exactly where a problem has occurred, especially for nonterminating exceptions (e.g., [[Page Fault]]) so that they handle the event and resume exactly where the exception occurred. Of course, one assumes that everything *before* the faulty instruction was executed and everything after was not. With OOO dynamic execution it might no longer be true...

> A processor can do whatever it wants provided that it gives the **appearance of sequential execution** (i.e., the architectural machine state is updated in program order)

## Reorder Buffer
By adding a reorder buffer (ROB) to the commit unit, we can continue executing instructions out of order. Even if an instruction raises an exception, we drop all instructions that appear after, and finish instructions up until the instruction causing the exception. *Tags* are used as placeholders in the [[#Reservation Stations]], and are *removed* one we have a value (the tag is replaced with the actual value).

![[Reorder Buffer.svg|700]]

When a synchronous exception happens,...
- we do not report it but we *mark the entry* corresponding to the instruction which caused the exception in the ROB
- when we would be ready to commit the instruction, we raise the exception instead
- we **trash** the content of the ROB and all [[#Reservation Stations|RS]]
- **squash** (remove) all instructions which followed the exception

> [!question] Where do we get the necessary information during decode time?
> When decoding an instruction, we are supposed to put, for each operand, either a tag or a value in the corresponding [[#Reservation Stations|reservation station]]—but how do we know if we can read the register file, for instance?!

Since the reorder buffer contains all instructions that need to be committed (and their destination registers), we have two cases:
1. None of the operands are in the ROB -> read value from [[Register File|RF]]
2. At least one operand is in the *register* column of the ROB -> Get *value*, if it exists, or *tag* (with multiple dependencies, get the newest tag)


The way we detect and resolve dependences through memory (a store at some address and a subsequent load from the same address) is the same as for registers.
For every **load**, check the ROB:
1. If there is no store to the same address in the ROB, get the value from memory (i.e., from the cache) 
2. If there is a store to the same address in the ROB, either get the value (if ready) or the *tag*
3. If there is a store to an unknown address in the ROB or if the address of the load is unknown (not yet computed), we **wait**!

This makes things harder:
- Loads need to wait in memory reservation station for addresses of operands (normal), but...
- Ready loads (with known addresses) need to keep checking ROB until the address of **all preceding stores are known**

## Load and Store Queues
This is better implemented using seperate [[Load-Store Queues.svg|load and store queues]] (for better coordination with ROB)

![[Load and Store Queues.svg|700]]

- All ready loads (known addresses) are checked concurrently
- Each load compares its address with all preceding store addresses and does (approximately) the following:
	- If any of the preceding stores *misses* the address -> *do nothing*
	- If all preceding stores have an address and there is *no collision -> issue the load* if there are available memory ports
	- If the load address equals one or more of the store addresses and if the last of the colliding stores has the value -> memory bypass = load is executed and the returned value comes from the store queue
	- If the load address equals one or more of the store addresses and if the last of the colliding stores has no value yet -> do nothing (will be a memory bypass later)
- This behaviour is essentially that of an RS but with the additional issue of checking for emerging collisions in the store queue

Stores are only executed if
1. The address and the data for the store are known
2. All proceeding stores have been executed
3. The store is enabled from the ROB (we *still need to keep precedence for previous instructions in the ROB*)

This behaviour is essentially that of an RS combined with the reordering of a ROB.
# Register Renaming
Registers can be renamed in multiple locations, we hold the renamed values in the [[Reorder Buffer.svg|ROB]], whilst having architectural registers in the [[Register File|RF]].

![[Register States.svg|600]]

Initialisation: First N registers are “Architectural Registers”, others “Available”
- Available -> Renamed Invalid
  Instruction enters the Reservation Stations and/or the ROB: register allocated for the result (i.e., register uninitialised) 
- Renamed Invalid -> Renamed Valid 
  Instruction completes (i.e., register initialised) 
- Renamed Valid -> Architectural Register 
  Instruction commits (i.e., register “exists”) 
- Architectural Register -> Available 
  Another instruction commits to the same AR (i.e., register is dead)
- Renamed Invalid and Renamed Valid -> Available 
  Squashing

---
References: