---
Tags: 
Created: 2023-03-28 17:11:39
---
(Links:: [[Computer Organization]])
# Hardware Multithreading
- *process*: program together with any information that describes its current state of execution
- *thread*: independent path of execution within a program / thread of control whose state consists of the contents of the program counter and other processor registers
- processor has several identical sets of registers, including multiple program counters, each dedicated to different thread -> *hardware multithreading*
	- No time wasted during [[Context switch]]
	- switching thread completed within one clock cycle
	- state of previous thread preserved in own set of registers
- *course-grained* multithreading: switch on event not fixed time interval (e.g. cache miss)
- *fine-grained/interleaved* multithreading: switch threads after every instruction is fetched 
	- instructions independent from predecessor 
	- reduce of stalls and data dependencies
	- takes longer to complete all instructions
# Vector (SIMD) Processing
- programs loop over vector (array) of data and performs operations
- processor has multiple ALUs
- *single-instruction multiple-data* / *vector* instructions: operate same instruction on multiple data elements in parallel
	- operations performed must be independent (*data parallelism*)
- *vector registers*: holds several data elements (number fiven by $L$ the *vector length*)
	- IA-32 architecture has 128-bit vector registers
		- $L=2$ for 64 bit integer data elements
		- $L=16$ for 8 bit integer data elemnets
- $L$ consecutive elements beginning at memory location $X+[Rj]$ are loaded into vector register $Vi$ $$\text{VectorLoad.S} \qquad \text{Vi,X(Rj)}$$
### Vectorization
- using vector instructions reduces the number of instructions that need to be executed 
- enables operations to be perfomred in parallel on multiple ALUs
- *vectorizing compiler*: identifies loops and generates vector instructions
> [!example]+ Loop vectorization
> - $9N$ instructions are executed for $N$ passes through the loop
> - vectorized loop requires only $N/L$ passes to process all of the data in the arrays
> ```
> for (i=0; i < N; i++)
>   A[i] = B[i] + C[i];
> ```
> ```
> 		Move                 R5, #N       R5 is the loop counter
> LOOP:   Load                 R6, (R3)     R3 points to an element in array B 
> 		Load                 R7, (R4)     R4 points to an element in array C
> 		Add                  R6, R6, R7   Add a pair of elements from the arrays
> 		Store                R6, (R2)     R2 points to an element array A
> 		Add                  R2, R2, #4   Increment the three array pointers
> 		Add                  R3, R3, #4   
> 		Add                  R4, R4, #4   
> 		Subtract             R5, R5, #1   Decrement the loop counter
> 		Branch_if_[R5]>0     LOOP         Repeat the loop if not finished
> ```
> ```
> 		Move                 R5, #N        R5 counts the number of elements to process
> LOOP:   VectorLoad.S         V0, (R3)      Load L elements from array B 
> 		VectorLoad.S         V1, (R4)      Load L elements from array C
> 		VectorAdd.S          V0, V0, v1    Add L pairs of elements from the arrays
> 		VectorLoad.S         V0, (R2)      Store L elements to array A
> 		Add                  R2, R2, #4*L  Increment the array pointes by L words
> 		Add                  R3, R3, #4*L
> 		Add                  R4, R4, #4*L 
> 		Subtract             R5, R5, #L    Decrement the loop counter by L
> 		Branch_if_[R5]>0     LOOP          Repeat the loop if not finished
> ```
- extent of performance improvement is limited by the vecotr length $L$ -> determines number of ALUs that can operate in parallel
## Graphics Processing Units (GPUs)
- floating-point calculations needed in high-res three-dimensional graphics -> calculations often independent
- GPU chip contains hundreds of simple cores with floating-point ALUs to perform them in parallel
- program written for processing cores -> cores execute same instructions, but operate on different data elements
- data needed by GPU transferred from main memory to GPU memory
- specialized instruction set and hardware architecture
- programs written with CUDA C (Compute Unified Device Architecture) have special keywords to label functions executed on GPU chip
# Shared-Memory Multiprocessors
- shared-memory multiprocessor have access to the same memory
- tasks in different processors have access to shared variables
- distribute memory across multiple modules for simultaneous access from processors
- *Uniform Memory Access*: system which has same network latency for all accesses from the processors to the memory modules
- *Non-Uniform Memory Access*: place a memory module close to each processor for better performance -> collection of *nodes*
	- request to access a remote memory module must pass through the network
## Interconnection Networks
- *bandwidth*: capacity of transmission link to transfer data (carries control information that coordinates transfer of data)
- *effective throughput*: actual rate of data transfer
- information transfer through network in form of *packets*
	- links are narrower for reduced cost -> packets divided into smaller pieces and transmitted in one clock cycle
### Bus
- set of lines that provide a single shared path for information transfer
- simple bus does not allow new request to appear on the bus until the response for the current request has been provided
- *split-transaction bus*: request and corresponding response are separate events -> no idle time waiting for response
- time between request and corresponding response may vary
- each request has unique tag for identification with corresponding response
### Ring
- point-to-point connections between nodes -> high average latency between two nodes
- *hierarchy* of rings: nodes on the same lower-level ring need not traverse the upper-level ring
	- [[Ring-based interconnection networks.png]]
	- upper-level ring is bottle neck for many nodes between different lower-level rings
### Crossbar
- provides link between any pair of units connected to the network
- many simultaneous transfers if the same destination is not the target of multiple requests
- [[Crossbar interconnection network.png]]
- for $n$ processors and $k$ memories, $n\times k$ switches are needed
### Mesh
- each node has four connections to it's horizontal and vertical neighbors
- wraparound connections between nodes at opposite boundaries of the mesh (*torus*)
# Cache Coherence
- processors have own cache and own versions of variables (other caches contain old, incorrect value)
- *cache coherence*: consistent view of shared data in multiple caches by using [[The Memory System#^e84a34|write-back]] and [[The Memory System#^74f904|write-through]] operations on multiprocessor systems
## Write-Through Protocol
- *update*: broadccast written data to the caches of all processors in the system -> update contents of affected cache block if present in its own cache
- *invalidation*: all copies in other caches invalidated through broadcasting, while value in memory updated
## Write-Back Protocol
- all copies in other caches invalidated with broadcast request
- other processors must request block to be forwarded from current owner -> data sent to appropriate memory module which changes ownership
- later requests are serviced by memory module containing block
- creates less traffic than write-through -> doesn't need to write to appropriate memory module
## Snoopy Caches
- each processor cache has controller circuit that *snoops* all transactions on the bus
- processor who requests invalidation becomes owner of the block
- processor (owner) snoops for *read request* and asserts signal on bus to prevent memory from responding
	- sends data on the bus -> accepted by cache of requesting processor and updates block in memory -> memory acquires ownership
- when two processors change same block, one cache gets invalidated
	- other processor is granted to broadcast *read-exclusive request*: combines read request and invalidation request for the same block 
	- owner of block invalidates copy in cache
- eliminate unnecessary interference between bus and cache circuitry by having duplicate set of tags, that can be accessed separately by snooping circuitry
## Directory-Based Cache Coherence
- memory module has directory to indicate which nodes may have copies of a given block in the shared state
- if block is modified, directory identifies node that is current owner
- write request: individual invalidations sent to nodes that may have copies of the block in question
# Message-Passing Multicomputers
- each node in the system is it's own complete computer with own memory
- to share data between nodes, computer with source of data must send message containing data to the destination computer
- communication unit at every node to interpret, decode message and copy data to and from memory of the node

# Parallel Programming for Multiprocessors
# Performance Modeling
- New execution time: $$T_{\text{new}}=T_{\text{orig}}(f_{\text{unenh}}+f_{\text{enh}}/p)$$
  $T_{\text{orig}}$: execution time on some computer
  $f_{\text{enh}}$: fraction of the execution time affected by enhancement
  $p$: factor by which the portion of time is reduced due to the performance enhancement
- **Amdahl's law**: *speedup* is ratio $T_{\text{orig}}/T_{\text{new}}$: $$\frac{1}{(f_{\text{unenh}}+f_{\text{enh}}/p)}$$
- Amdahl's law can provide insight in the improvemnet factor before implementation -> helps justify effort and expense to implement the enhancement

---
References: