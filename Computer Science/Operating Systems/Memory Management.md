---
Tags: 
Created: 2023-12-09 19:14:00
---
(Links:: [[Operating Systems]])
# Memory Abstractions
The simplest way of managing memory, is to have no memory abstractions whatsoever.
![[Three simple ways of organizing memory with an operating system and one user process.png|600]]
a) allows us to accidentally write into the operating system's code
b) prevents us from writing to the operating system by having it in Read Only Memory
These examples don't work with multiple programs. A Naive approach would be to move each program to a dedicated region. We would have to however *add the offset* to all addresses in the program. This can take a lot of time making it very inefficient.
![[Relocation problem process.png|500]]
In these examples we also have the problem of access to *restricted memory*. For this the CPU had a set of memory keys associated with each of the blocks of memory. If the current process had the correct protection key, it would have access to it's associated memory locations.
In current computers, we have a **base register** and **limit register** which give an illusion of the process having it's own memory space. The base register does the *dynamic relocation* automatically, and the limit register enforces *protection*.

> [!question]- What if we run out of memory?
> Swap processes that are currently not in use with a new process **or** we use [[#Virtual Memory]].

Swapping may lead to memory fragmentation. This means that we might have enough memory for a process, but it can't be loaded in because the free memory locations are next to each other. For this we use **memory compaction**.
![[Memory allocation changes as processes come into memory and leave it.png|700]]
Processes can grow, and as such we need to have extra room for this. The question is how much extra room should we allocate, and what should happen to a process when we run out of memory?
![[Allocating memory for processes.png|500]]

> [!question] How do we know which part of the memory is allocated?
> 1. Bitmaps: Finding holes requires (slow) scanning
> 2. List: Common vs. separate process/hole lists
> 	1. Slow allocation vs. slow deallocation
> 	2. Holes sorted by address for fast coalescing
> 	3. Holes can be sorted by size
> 
> In practice a doubly [[Linked List]] is often used which makes freeing easy.

> [!info] Memory allocation schemes
> - First Fit: Take first fitting hole. Simplest option
> - Next Fit: Take next fitting hole. Slower than first fit in practice
> - Best Fit: Take best fitting hole. Prone to fragmentation
> - Worst Fit: Take worst fitting hole. Poor performance in practice
> - Quick Fit: Keep list of holes of different sizes. Poor coalescing performance
> - Buddy allocation scheme

## Buddy Memory Allocation
![[Operation of the buddy algorithm.png|600]]

This algorithm starts with a 64 "chunks" of memory. When we need to allocate memory, for a process we split the chunk in half to most suitably fit the process's memory needs. This allocator is very fast and *limits external fragmentation* but does not consider *internal fragmentation* (wasted space inside a page). It is therefore often combined with the **slab allocator**. When accessing memory, you'd use the slab allocator which itself gets memory from buddy.
The slab allocator supports memory allocation for the kernel. The kernel needs many different temporary objects, with different and *specific* sizes. These objects are often allocated and freed. The slab allocator is used as a **cache for specific objects (or object sizes)**. By **reusing freed objects**, under favourable circumstances, you can avoid having to reinitialize them (leaving them partially filled in). 
Since all slots have the same (known) size, we can easily find the $n$-th object in the slab.
![[Slab allocator.png|800]]
A slab contains:
- a pointer to the start of memory with object slots
- index of the next free slot ("slot for next allocation")
- bufctl: array of indicies ("after this free slot has been used for allocation, the next free slot is...")
- Slots for objects

For generic object sizes, we use sizes with different powers of two.
# Virtual Memory
So far memory can only be given to processes in contiguous pieces. With virtual memory we create for the process the **illusion of a large address space**. The RAM is then known as the *physical* memory. The **MMU (Memory Management Unit)** is responsible for getting the virtual memory's associated physical memory.
Modern systems use paging: We divide physical and virtual memory into pages of fixed size (e.g. 4096 bytes). 

![[The relation between virtual and physical memory addresses is given by the page table.png|500]]
![[The internal operation of the MMU with 16 4-KB pages.png|500]]

Page table's have entries of size 32 or 64 depending on the underlying processor architecture, with the following structure:
![[A typical page table entry.png|700]]

Two important bits used later on for [[#Page Replacement Algorithms]] are:
- Modified ("dirty") bit: Set when the page has been modified. If so it first must be written back to disk before it can be replaced.
- Referenced ("accessed") bit: Set when page is accessed. We choose entries which have not been accessed to be replaced rather than those which are being accessed.

The side effect is however that with higher bit systems, the virtual address space becomes very large leading to a large page table. We therefore now use **multiple-level page tables**. On x86 systems, the `CR3` register points to the top-level page table. Since a page is $2^{12}$ Bytes (4KB) large and each entry is in this case 32 bit ($2^2$ Bytes), each page table holds $2^{12}/2^{2}=2^{10}=1024$ entries. On a 32 bit system, the top 10 highest order bits are used to index the first page table, the next 10 the second level page table and the last 12 bits are used as an offset in the page itself. These multi layer page tables are "walked" by the hardware MMU.
On x86-64, only 48 bits or 52 are used for addressing, but we use four-level page tables:
![[Four-Level Page Tables.canvas|Four-Level Page Tables]]

A second way of addressing the data on a machine is to use **inverted page tables**. With a 64-bit system there are $2^{52}$ pages and as such $2^{52}$ table entries. Instead of having a page table with all the entries, we can have a [[hash table]], where the virtual address is first hashed, and then searched for in a [[Linked List]].

## Translation Lookaside Buffer
In both circumstances we have to wait sometimes very long by loading in new tables or traversing the linked list in the hash table. The solution to this is a cache of the previous translations known as a **translation lookaside buffer**, hoping the programs exhibit a high degree of locality.

> [!example]- Translation Lookaside Buffer
> 
> | Valid | Virtual Page | Modified | Protection | Page Frame |
> | ----- | ------------ | -------- | ---------- | ---------- |
> | 1     | 140          | 1        | RW         | 31         |
> | 1     | 20           | 0        | R X        | 38         |
> | 1     | 130          | 1        | RW         | 29         |
> | 1     | 129          | 1        | RW         | 62         |
> | 1     | 19           | 0        | R X        | 50         |
> | 1     | 21           | 0        | R X        | 45         |
> | 1     | 860          | 1        | RW         | 14         |
> | 1     | 861          | 1        | RW         | 75         |

> [!question] How many TLBs?
> - These caches are very expensive and we therefore only have a **small number of entries** (64-128). 
> - Sometimes there are *separate entries* for huge pages and normal pages. 
> - Often we use separate TLBs for *code and data* because the access different. 
> - In addition we can also have **multiple levels of TLB** (like with CPU caches).

> [!question] When to flush TLB?
> - When we change process (context switch), the virtual page might not be the right one, but that of the previous process, so we clear the TLB. 
> - To improve on this, we give each entry an id of it's process. Now we don't need to flush since we can distinguish which translation belongs to which process.

> [!question] When to expect many TLB misses?
> - After a flush, there will be no entries, so we'll have many misses. 
> - When a program has little locality, we will have to change pages all the time.

If we don't want a page to be flushed, we can set the **global** bit flag on the PTE (page table entry). This way, it is always readily available.

# Page Replacement Algorithms
When a page doesn't show up in the TLB or a translation isn't found, these are know as **TLB miss** and [[Page Fault]] respectively. There are two ways of handling TLB misses:
- by the *OS*: OS may preload TLB entries that it *expects* to need later (say the entries for a server to which current process sends msg)
- by the *hardware*: Hardware walks PTs and fills TLB
	- if the mapping is found, fill new TLB entry (*soft miss*)
	- if the mapping isn't found, page fault (*hard miss*)

With page faults, there are a few cases:
- Access violation (*segmentation fault*)
- Legal access, PF handler needs to fix up page tables:
	- The page is already in memory (*minor PF*; page isn't mapped properly)
	- The page must be fetched from disk (*major PF*)

Computers might use more virtual memory than they have physical memory. This leads to page faults and we have to swap out an old page. The question is which. 
## Not recently used algorithm
We divide all pages into four categories from the two bits $R$ and $M$: 
- Class 0: not referenced, not modified
- Class 1: not referenced, modified
- Class 2: referenced, not modified
- Class 3: referenced, modified

We prefer to replace the lowest classes.
## First-in, first-out algorithm
- Build queue of faulting pages and replace the head. However, the oldest page may still be useful.
## Second-chance algorithm
- Improved FIFO to preserve important pages
- It looks for an old page that has not been referenced in the most recent clock interval
- For each visited page:
	- If $R=0$ then replace page
	- If $R=1$ then set $R=0$ and move page to tail
 
![[Second change algorithm.png|700]]
## Clock algorithm
We keep all page frames on a circular list, with the hand pointing to the oldest page. When a page fault occurs, the page the hand is pointing to is inspected. If $R=0$, evict the page, otherwise clear $R$ and advance hand.
## Least recently used algorithm (LRU)
The idea here is to replace the page that has been **unused for the longest**. This is not implemented that often since it is extremely *expensive* to keep a list of pages sorted. 
Instead we can use **Not Frequently Used** which keeps a counter for each page, periodically adding the $R$ bit to the counter. We then replace the page with the lowest counter.
We can improve on this by using **aging** which adds $R$ to the leftmost bit and then shifts the counter to the right. This aging algorithm simulate LRU in software.
## Working set algorithm
The **working set** is a set of pages that a process is currently using. If the working set is in memory, we get few page faults, otherwise we get many page faults. If the memory is too small for the WS the we get a lot of page faults (thrashing). Most programs however exhibit locality of reference.
The working set of a process is the set of pages it has referenced during the past $\tau$ seconds of virtual time (time consumed by the process).
The basic idea is to find those pages which have not been referenced and whose age is older than that of $\tau$. If the $R$ bit is 1, then we write the current virtual time into the age field. If all pages are in the WS, then we take the page who has the smallest age.
## WSClock algorithm
WS is cumbersome as we must scan the page tables for each PF. WSClock combines Clock and WS and is more efficient.
- Circular list of page frames (initially empty)
- When page is referenced -> add to list with time
- Advance hand + check if $R==0$ (else $R\to0$, skip after updating time)
- `if(R==0 && age > t && not dirty)` -> simply replace
- `if(R==0 && age > t && dirty)` -> schedule write and continue
- If hand comes all the way around -> 2 cases:
	- At least one page scheduled for write
	- No page scheduled for write -> any clean page otherwise the current page
# Design Issues for Paging Systems
- Demand paging:
	- Lazily allocate pages to each process
	- Exploits temporal locality to avoid frequent PFs
- Prepaging: 
	- Typically based on the **working set** mode, i.e. the set of pages a process currently needs to execute
	- Load the process working set in memory in advance when scheduling the process to avoid frequent PFs
	- Need for **working set estimation** algorithms

What happens when we allocate memory? Library functions such as `malloc` under the hood call different syscalls: `brk`, `sbrk` or `mmap`. On linux, `malloc()` uses the `brk()` syscall for small areas an `mmap()` syscall for large areas. `brk` extends the heap, while `mmap` creates a new mapping in virtual memory.
This allocates space in the virtual memory area, but it isn't backed yet in physical memory. Only once we try to access the memory, do we get a page fault, and we assign a physical page frame.
![[Demand paging 1.png|500]] 
![[Demand paging 2.png|500]]

If we plot the user and system time for allocating areas of different sizes we get the following:
![[User and system time, memory allocation.png|500]]
At a certain point the time jumps up because we use the `mmap` system call to allocate memory. After that the time increases for the system time, because it takes longer to deallocate the memory chunks.

Another issue is whether we should only replace those pages that are from the local process, or replace pages that belong to other process.
- **Local allocation**:
	- Replaces only pages of the current process
	- Assumes static process memory allocation size
	- Problems:
		- A growing *working set* leads to **thrashing**
		- A shrinking working set leads to wasted memory
- **Global allocation**:
	- Replaces pages owned by any process. Strategies:
		- Treat every global page equally
		- Dynamic memory balancing using *working set size* estimation
		- Selectively swap out processes (or OOM kill)

Since almost every process is in need of more memory, we can swap out processes to **relive the memory load**, so that the other processes can complete. 
Another issue is the question about the page size. With smaller page sizes, we can *limit internal fragmentation* and *minimize space wastage*. This does however mean that program need *more pages* which causes more page faults, and each entry in a TLB is more *valuable*.

We also need to consider whether we store separate caches and TLBs for *code and data*.
We must also consider **sharing pages** since it is common that several programs are using the same libraries, even on the same user. The OS should be able to detect, when a process exits, which pages are still needed. For this to be possible, we use dynamic link libraries, which uses **position-independent code**. In addition, when we fork, we can keep using the same pages. Only when the child process starts writing to a file, does the OS make a copy so that none of the changes are visible in the parent. This is known as **copy-on-write**.

Shared libraries take inspiration from **memory mapped files**. Instead of writing to a file, the file can be loaded into memory one page at a time as a single character array, where it can be easily accessed be the program.

Paging works best if we have plenty of free pages. We use a paging daemon that sleeps and then evicts pages if free pages are in short supply. These pages can still be accessed, and aren't cleared. When a process needs to load in a page (due to a PF), it doesn't have to look around, and can simply take a free page (which has already been saved to disk by the daemon if it was dirty).

> [!info]- Page Fault Handling
> 1. The hardware traps to the kernel, saving the program counter on the stack. On most machines, some information about the state of the current instruction is saved in special CPU registers.
> 2. An assembly-code routine is started to save the general registers and other volatile information, to keep the operating system from destroying it. This routine calls the operating system as a procedure.
> 3. The operating system discovers that a page fault has occurred, and tries to discover which virtual page is needed. Often one of the hardware registers contains this information. If not, the operating system must retrieve the program counter, fetch the instruction, and parse it in software to figure out what it was doing when the fault hit.
> 4. Once the virtual address that caused the fault is known, the system checks to see if this address is valid and the protection is consistent with the access. If not, the process is sent a signal or killed. If the address is valid and no protection fault has occurred, the system checks to see if a page frame is free. If no frames are free, the page replacement algorithm is run to select a victim.
> 5. If the page frame selected is dirty, the page is scheduled for transfer to the disk, and a context switch takes place, suspending the faulting process and letting another one run until the disk transfer has completed. In any event, the frame is marked as busy to prevent it from being used for another purpose.
> 6. As soon as the page frame is clean (either immediately or after it is written to disk), the operating system looks up the disk address where the needed page is, and schedules a disk operation to bring it in. While the page is being loaded, the faulting process is still suspended and another user process is run, if one is available.
> 7. When the disk interrupt indicates that the page has arrived, the page tables are updated to reflect its position, and the frame is marked as being in the normal state.
> 8. The faulting instruction is backed up to the state it had when it began and the program counter is reset to point to that instruction.
> 9. The faulting process is scheduled, and the operating system returns to the (assembly-language) routine that called it.
> 10. This routine reloads the registers and other state information and returns to user space to continue execution, as if no fault had occurred.

To manage complex systems it is best to separate policy and mechanism. Memory management systems are divided into three parts:
1. A low-level MMU handler
2. A page fault handler that is part of the kernel
	1. Machine independent
	2. Contains mechanism for paging
3. An external pager running in user space
	1. Machine independent
	2. Can implement the *policy*

![[Page fault handling with an external pager.png|500]]

Sometimes it's better not to think of the entire memory as a *single continuous space*, but **different segments**, to allow each table to **grow and shrink independently** of each other. Segmentation was often used with paging. With the MULTICS implementation, we have a descriptor segment with entries (segment descriptors) which point to the associated segment's page table.

![[MULTICS virtual memory.png|500]]
![[MULTICS virtual memory segment descriptor.png|500]]

Just like before, MULTICS also provides a TLB for caching.

---
References: [[Chapter 03 - Memory Management Alt 3-20.pptx.pdf]]