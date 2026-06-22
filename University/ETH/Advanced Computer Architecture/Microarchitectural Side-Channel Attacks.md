---
Tags: 
Created: 2026-06-14 22:07:14
---
(Links:: [[Advanced Computer Architecture]])

> [!question] Why do we have hardware security
> 1. Software is complext: We cannot trust OSes and hypervisors to be bug free ([[Trusted Execution Environments|TEE]])
> 2. People can attack micro-architectural side-channels: Sharing with other users gives them the ability to discover our secrets (e.g. shared caches and processors)
> 3. Physical monitoring gives way of [[Physical Side-Channel Attacks]]: Users cannot physically protect their computing hardware as it is often in the cloud or embedded and remote

We will assume knowledge of the [[CIA Security Triad]] and [[Threat Model]]. In addition, we coin the term *isolation* to be the possibility to prevent any interaction between users and processes, often used to guarantee confidentiality and integrity.

See details on [[Rowhammer]] for attacks on memory to compromise *integrity*.  Rowhammer effectively *violates memory protection* ("if I can read, I can also write") which is a key ingredient to privilege separation across processes. By accessing locations in neighbouring rows one could gain unrestricted memory access and privilege escalation:
- Allocate large chunks of memory, try many addresses, *learn weak cells*
- Release memory to the OS
- Repeatedly map a file with read-write permissions to *fill memory with [[Compressed Files/A typical page table entry.png|page table entries]] (PTEs)*
- Use Rowhammer to *flip (semirandomly) a bit in one of these PTEs*; it will now point to the wrong physical page
- Chances are that this physical page contains PTEs too, so now *accessing that particular mapping of the file (RW) actually modifies the PTEs*, not the file
- Attacker can arbitrarily change PTEs and *memory protection is gone*

This is not that simple in practice, but people have managed to make it work.
An additional attack related to DRAMs are [[Architectural Support for Security#Cold Boot Attack|cold boot attacks]].

> [!definition] Covert Channel
> A covert channel is an *intentional communication* between a sender and a receiver *via a medium not designed to be a communication channel*.
> ~ Szefer, 2019
> 
> If we isolate a critical process inside a virtual machine (or an *enclave*), a covert channel may allow a rogue programme inside of the isolated process (a *trojan horse*) to leak a secret to some malicious receiver without anyone to notice (no conventional communication channel visible).

See System Security lecture on [[Side Channel Attacks]]. Based on the *existence of microarchitectural state*, that is state not (normally) visible to the programmer–because architectural state is known and thus, apart from bugs, inherently protected! This becomes an issue when hardware components are shared. An approach of dealing with these attacks is through physical replication and isolation.
Physical attacks are based on the physical nature of the computing system and are usually more difficult to exploit and protect against.

Two popular approaches to **breaking isolation and confidentiality** is through [[Timing Side-Channel Attack]], and through [[Cache Side-Channel Attack]].

> [!question] What problem arrises when caches are large?
> Remember that a virtual memory address is [[Page Table Translation.svg|translated]] into a physical address via a [[Four-Level Page Tables.svg|page table]], where the lowest bits address into the exact page (which is usually 4KB large).
> When querying for a page, the operating system returns if it is available, but we never know the exact physical address (except for the offset).
> 
> When accessing caches, the lower 6 bits are used to index inside the 64 bytes of a cache line, and further bits are used to index in the entire cache for a [[The Memory System#Set-Associative Mapping|specific cache set]].
> 
> If the index is *within* the offset size of specific (virtual) memory address I am requesting, then I can completely control which cache set I am targeting -> side-channel attack is easy! This is guaranteed for **L1 cache**, since it cannot wait for the entire page table traversal, but not for the *last level cache* (LLC)
> 
> ![[Cache SCA–large caches.svg|600]]

Cache attacks that make use of the last level cache are required when the attacker and victim processes are on *different cores*. This is also useful because larger caches have more sets, which gives the attacker a *better resolution*, as they can more precisely see what is being accessed. Important to note is the **inclusion property**: Data in L1D cache is guaranteed to be in L2 and similarly in L3 cache. This means that if the attacker evicts something in L1D, it is evicted also in L2 and L3.

One (more brute-force) approach is using **huge virtual pages**. Some applications benefit from a larger page size than 4kB, and this can be set for the (guest) operating system. On a shared host, two virtual machines would share the L3 cache, but the attacker VM can set their effective *offset* to be large, and thus be able to affect *all index bits* used for addressing the cache sets.

A different approach is the following: 
1. First fill the L1 cache (*prime*)
2. Run the victim's code (which will hopefully overwrite a cache set)
3. Find misses (*probe*)

![[Pasted image 20260615185104.png|700]]

==TODO: learn and understand==

> [!question] What to do when attacker and victim are asynchronous
> When the attacker runs in a [[Virtual Machine]], and the victim is in *another one*, synchronization is not possible!
> We can **flush+reload** [^4], similar to prime+probe, where we use the `clflush` instruction of x86 to evict a specific cache line which depends on virtual machine page deduplication (if two users load the same executable or libraries, only one is kept in memory). Attacker and victim use different virtual addresses in different virtual machines, but the physical address is the same!
> The attacker can now track accesses to code to infer the internal state of the victim. [^1]

**Cache attack possible mitigations**:
- Hardware solutions
	- Various forms of partitioning and randomization
	- Practically *none is truly effective and efficient*, and thus viable for general use
- Generic software solutions
	- Clumsy, difficult to generalize, costly, not always effective
	- **Cache colouring** -> OS uses physical page allocation to reserve sets for some processes (using the few bits in the physical page that overlap with the index for cache addressing -> *partitioning*)
- Application-specific solutions: possibly the safest option

# Combined Attacks to Break Isolation and Confidentiality
[[Meltdown]] [^3] is an attack which allows *reading all memory of a process* (including *protected kernel data*), taking advantage of the implementation of microarchitectural features. It exploits a [[race condition]] between *memory access and protected checks*. It ultimately exploits the microarchitectural nature of caches (something is left in the cache upon exception because *the cache is not part of the architectural state*)

> The attacker executes a *forbidden access* and *speculatively uses the result* to obtain *nonarchitectural side-effects* that reveal the secrets *before the forbidden access is squashed*.

Most OSes map physical kernel memory pages into *every* user's virtual memory space. This minimizes the cost of some exceptions (e.g. fast interrupt handling, less [[Translation Lookaside Buffer|TLB]] flushes). The access to pages are protected, and kernel pages are only available to be read in [[Permission Levels Ring.svg|kernel mode]], *but* everyone can address them!
The idea is to execute a *forbidden access* and *speculatively use the result* with *nonarchitectural side-effects* that reveal the secrets before the forbidden access is squashed.

![[Meltdown.svg|1000]]

A possible mitigation against this attack is to *change the processor design* such that we test privilege level *before* making the result of a speculative access available. Alternatively, we can *better isolate user space and kernel space* memory (**Kernel page-table isolation** on Linux)

---

The [[Spectre]] [^2] attack follows a somewhat similar process using speculative execution instead of out-of-order execution, but makes it possible to *read all memory*. It uses wrongly predicted, and thus speculatively executed, branch predictors. A misspeculation does not affect the architectural state, but it may *affect microarchitectural structures* (e.g. caches). The goal is to get the victim to speculatively execute *leaky* code whose nonarchitectural side-effects reveal the secrets.

```c
if (x < array1_size)
	y = array2[array1[x] * 4094];
```

1. With an appropriate value for `x` we can read anything we want.
2. If we can get the processor to *mispredict the condition*, the access will be speculatively performed (but the *value will be removed from the [[Exploiting ILP Dynamically#Reorder Buffer|ROB]]*)
3. If we use the speculatively loaded value from `array1[x]` for a **legitimate** memory access, the *trace of it will remain in the cache*

Mitigations against spectre are almost [impossible](https://meltdownattack.com) without significant performance costs (such as disabling speculative execution and separate branch predictors per process/thread). You could also add serialization instructions between branches and loads or make it impossible through JavaScript in browsers.

---
References:
- [[3. Hardware Security -- Microarchitectural Side-Channel Attacks.pdf]]

[^1]: Yarom and Falkner, USENIX Security ’14

[^2]: P. Kocher, J. Horn, A. Fogh, D. Genkin, D. Gruss, W. Haas, M. Hamburg, M. Lipp, S. Mangard, Th. Prescher, M. Schwarz, and Y. Yarom, Spectre Attacks: Exploiting Speculative Execution, IEEE S&P, May 2019

[^3]: M. Lipp, M. Schwarz, D. Gruss, Th. Prescher, W. Haas, A. Fogh, J. Horn, S. Mangard, P. Kocher, D. Genkin, Y. Yarom, and M. Hamburg, Meltdown: Reading Kernel Memory from User Space, USENIX Security, August 2018

[^4]: Y. Yarom and K. Falkner, FLUSH+RELOAD: a High Resolution, Low Noise, L3 Cache Side-Channel Attack, USENIX Security, August 2014
