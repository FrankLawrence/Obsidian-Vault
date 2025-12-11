---
Tags: 
Created: 2024-10-19 20:09:15
---
(Links:: [[Advanced Network Programming]])
# [MegaPipe](https://www.usenix.org/system/files/conference/osdi12/osdi12-final-40.pdf)
A new networking API for doing high-performance network operations. There were high overheads in small packet processing. In this paper inefficiencies in the Linux kernel networking stack with scalability were pin-pointed. The wanted to answer the following two questions:
1. How does the system perform when we increase number of concurrent connection
2. How does the system perform with increasing number of cores in the system

by *small packet processing* we mean:
- These clients connect to a server machine
- Request a file, operations, or transaction - get a response back
- "Small" request - response (not a data heavy workload) - few bytes to kilobytes
- "Short-lived" - quick connect, disconnect

![[Pasted image 20241019210413.png|400]]
![[91587.png|400]]

This is very stressful to the CPU, and the system cannot use many of the previously discussed tricks
- System need to process millions of packets/sec
- **Per-packet costs dominate**
	- Packet and protocol processing
	- Per packet memory management
	- Scheduling

In this case there are several problems that need to be fixed:

> [!caution]+ Specific Problems
> # Global Accept Queue
> There was a lack of scalability 
> 1. Incoming SYN packets are put in a "`request hash table`"
> 2. Once SYN + ACK is sent, they are moved to a "`accept queue`"
> 3. Once, the application calls "`accept`" they are taken out from the accept queue
> 
> **Problem**: The request hash table and accept queues are shared between all "cores"
> ![[15333.png|400]]
> https://pdos.csail.mit.edu/papers/affinity-accept:eurosys12.pdf
> # Lack of Connection Affinity
> The problem becomes more challenging because previous flow-steering mechanisms only do flow steering after "*sampling*" (checking which ports are open, what the application is doing to reschedule application) some packets. However, with short connections **there aren't enough packets to sample**
> # Implementation Details
> 1. What is a socket
> 	1. It is a file descriptor, complain to the POSIC standard
> 	2. "The POSIC standard requires that a newly allocated file descriptor be the lowest integer not currently used by the process" - figuring out minimum requires coordination between all CPUs (not needed)
> 2. All file descriptors get attached to the Linux [[File Systems#File System Implementation|VFS]] (everything is a file)
> 	1. The VFS has its own set of file instance, inode, and dentry data structures
> 	2. For short connections - lots of gloval state allocation and de-allocation (not needed)
> 3. System calls
> 	1. Is the was we communicate with the operating system
> 	2. But they have performance issues (raises an interrupt/exception, disrupt ongoing flow)

## Solution
1. Partition the Request Hash Table and Accept queue for per-core (change the API)
	1. Application dictated partitioning and accept redirection
	2. Allows multiple threads to listen on to the same port number
2. A special "lightweight" socket descriptor: Not a file, but just an identifier. Avoid the VFS overheads
3. A new channel and message based API which allows system call "batching"
	1. Multiple send/recv requests can be passed in one go, hence, amortizing the overhead of doing a system call
	2. Notification only on when complete messages are sent/received

For the sender side, they used a channel and notification base I/O:
- A simple notification on a separate channel
- per operation - no need to constantly keep track of 
- Efficient, less application involvement

1. Messages in one shot
2. Batching
3. Syscall to megapipe
4. Batch completion event notification

The result is a large improvement in the throughput, when there are few packets sent per connection and **linear scalability** with core count.
![[Pasted image 20241019214639.png|500]]

> [!Danger]+ Issues
> 1. New non-socket API
> 	1. Very hard to convince people to rewrite their networking code
> 	2. New semantics
> 2. Kernel modifications
> 	1. Very hard to convince people to modify their kernels
> 3. Need support from the application
> 	1. Very hard to convince people to tell the networking stack how to partition the listening socket and manage accept queues

# Packet Processing Frameworks
Lots of applications such as firewall, routers, forwarding, load-balancers, and traffic generators process and work on raw network packets - they are middlewares. We want to replace them with multi-core CPU machines because
- high-end switches are expensive
- They have hard-to-use and proprietary systems
- Not flexible (ASIC)
- You have a new protocol, or aggregator or application-level hook? Forget about it

Basic packet processing and small short-lived connections have a lot in common: 
- Less bandwith-heavy, but more *volume* driven
- Small payloads
- Stress on per-packet processing cost

# [Netmap](https://www.usenix.org/system/files/conference/atc12/atc12-final186.pdf)
The key problem: Getting fast access to packets. The options then were:
1. **Raw sockets** (AF_PACKETS): Get direct access to raw data coming in
	1. High overheads, packet copies, per packet system call
2. **Direct buffer access**
	1. Run in the kernel (takes too long)
	2. PF_RING: data copies and shared metadata overheads
3. **Packet filter hooks** (eBPF): Complex, in the kernel, limited changes
	1. Technology in the kernel -> injects arbitrary code at certain locations
	2. Connects to different events, ex: receiving TCP connection
	3. ![[Pasted image 20241019221142.png|400]]

Back then, there were **no high-performance, safe, flexible way of getting access to raw packets!**

> [!bug] Root causes of high overheads
> A single packet is defined by [`struct sk_buf`](https://elixir.bootlin.com/linux/v6.11.4/source/include/linux/skbuff.h#L864) inside Linux, which is: 
> - extremely general packet representation - for any protocol not just TCP/IP
> - contain *pointers* and *functions* for any thing possible on the packet
> - very very large (`struct sk_buf` is more than 200 lines of code)
> - has close to 100 variables to keep track of information
> - [Previous research has shown that 63% of the CPU usage during the processing of a 64 byte large packet is skbuff-related](https://www.net.in.tum.de/fileadmin/TUM/NET/NET-2014-08-1/NET-2014-08-1_15.pdf)
> 
> System calls are not cheap:
> - They trap into the kernel
> - Disrupt ongoing processing
> - Processor ring switch
> - Security checks
> 
> ![[33586.png|500]]
> https://www.usenix.org/legacy/events/osdi10/tech/full_papers/Soares.pdf

What would the performance have to be, if we would be using system calls?
It takes roughly 1 microsecond for the linux kernel to process a packet. How long would it need to be if we handle 10Gbps:
- Ethernet payload is 64 bytes, with the total Ethernet frame (20B ETH headers) is 84 bytes
- 10Gbps = 10.000.000.000 bits/sec
- $10\,Gbps/84\times8=14.880.952$ packets per second
  **=> 67.20 nanoseconds per packet**

We are clearly way off, and need to optimize it everywhere.
## Solution
1. **Better packet buffer management**
	1. All uniform packet data structures: a pool of them are initialized at the boot time (preallocation)
	2. Linear, no fragmentation/reassembly
2. **Give direct and safe access to NICs RX and TX queues**
	1. Zero copy data movement
	2. Very small shared state (a few pointers)
3. **Batched system call processing**
	1. Send/recv multiple packets in a single call

### Packet in-memory layout
- Fixed packet size: 2KB (no fragmentation)
- Memory allocated by the kernel (protected) and shared between the NIC and application
- Multiple queues - per core mapping
	- Each queue has its own file descriptor and memory
### The Zero-copy stack
- Raw packets are built and transmitted directly from the user space (L2 level processing)
- System call only to notify the NIC there is work (so multiple packets can be queued)
- Processing of the `ioctl/select/poll` calls - like any other file descriptor

To achieve this, need support from the NIC driver and the NIC itself with certain capabilities
- Multiqueue interface (virtualization)
- High DMA (to DMA any memory, 64 bits)
### Results
1. At 1GHz speed, netmap can saturate a 14.8 Mpps link (default Linux and BSD cannot, with a single core). E.g., Linux has 4 Mpps/core → ~4 cores
2. Batching helps to achieve “line-rate”  
    a. One cannot achieve line rates without batching b. But batching (typically) increases latency

## User space packet processing
Netmap brought attention to per-packet processing, and also showed the benefit of
- Pre-allocating number of buffers
- Doing system call batching
- Flexible packet processing implementation in user space -> If we can do fast packet processing in user space, then can we build a fast networking stack in user space?

User space programs are not special, it is your application so you can do whatever you want.
### DPDK Framework
Intel started Data Plane Development Kit:
- they wanted to sell more CPUs
- they want to show how fast their CPU was for packet processing

It has since been used in production for software switches, routers, and cloud networking infrastructure.
1. **Data path** - code path where the actual work is done
2. **Control path** - code where resources are managed
3. **Fast path** - common case execution (typically few branches, very simple code)
4. **Slow path** - more sanity checks (more branches, hence poor(er) performance)

- Direct user space packet processing
- A list of standard set of infrastructure libraries
- No device driver modifications needed, uses Linux's UIO framework. Does userspace memory mapped I/O
- Interaction with hardware based on memory mapped I/O (with polling)

> [!info] Key ideas
> 1. No system calls or interrupts - **all polling**
> 2. No kernel overheads in the **data path**, kernel involvement = ZERO
> 3. Multiple libraries and mechanisms supporting
> 	1. Multicore affinity - core/thread pinning
> 	2. Buffer management - packet buffers, NUMA-aware
> 	3. Lockless queue management - using CAS instructions
> 	4. Huge pages - reduces TLB pressure
> 	5. Bulk / burst throughput I/O calls - amortize function call invocations (no syscalls here)

At this point, DPDK is a large framework which is almost rebuilding the whole Linux networking infrastructure in userspace for FAST packet processing: https://doc.dpdk.org/guides/prog_guide/overview.html
# [mTCP](https://www.usenix.org/system/files/conference/nsdi14/nsdi14-paper-jeong.pdf): Scalable User Space TCP Stack
The problem with MegaPipe was that it used a new different API, and made modifications to the kernel. A new solution was developed which runs in userspace because
- Expensive syscall
- Metadata/data copies
- Kernel environment
- Generality vs specialization argument

Between all packet processing kernel, and TCO/IP - there is very limited number of CPU cycles left for the application. Kernel consumes 80% of CPU cycles.
![[16682.png|500]]
## mTCP basic ideas
- TCP stack implementation in userspace
    - Flexibility and easy of development and use
    - Specialization of TCP - common options, fast path
- Leverage packet processing frameworks to deliver performance
    - Uses packet shader (similar idea as netmap) 
    - 10s million packets/sec in user space
- Multicore scalability
    - Per-application thread design  
    - Transparent batching of events and packet I/O

- Application and mTCP threads
    - **Partitioning**: All data structures (file descriptors, TCP state information) partitioned between cores
    - **Pinning**: Threads pinned together on one core, and RSS configured to deliver packet there
    - **Lock-free**: data structures using single producer/single consumer queues
    - **Private per-core**: Each core has its own memory allocator and caching

mTCP also implements cache alignments/packing. CPU caches have cache lines of certain sizes, typically 64 bytes. That is the unit of data transfer between the CPU cache and the DRAM. So you want to align your `tcp_struct` on the cache line size and group together frequently accessed items.
![[Pasted image 20241020014751.png|500]]
Here in this case, due to the unfortunate ordering in which the struct fields are defined, seq and ack number happen to lie on different cache lines. However, often they are processes together. Hence, it makes sense to pack them on the same cache line by reordering their definition order. In the Linux kernel you see many such examples ...

mTCP also makes two TCP-specific optimizations: 3 types of TX queues an TCP cache.
1. TX queues: control, ACK, and data
	1. Small, short-lived TCP connection are control message heavy (SYN/ACK)
	2. Give priority to the "control" packets, when transmitting: Control packets => Ack packets => data packets
2. TCP cache
	1. Lot of new connection means lot of new socket descriptors, buffers, TCBs, queues, pointers, structure allocation
	2. Proposes a **pre-allocated pool** of structures per thread and reuse it constantly

> [!bug] Limitations for mTCP
> - Limited memory protection between the shared mTCP library and application
> - Change in application semantics if they attach to specific "file descriptor" semantics (not all Linux I/O are supported on the fd)
> - By passing all kernel services - packet scheduling, firewalling, routing - who is going to re-implement them in the userspace?
> - Limited number of TX/RX queues, and no unlimited multi-application support

> [!summary] Libraries
> |            | Accept queue | Locality | API          | Event handling   | Packet I/O      | App. changes          | Kernel modifications        |
> | ---------- | ------------ | -------- | ------------ | ---------------- | --------------- | --------------------- | --------------------------- |
> | Netmap     | x            | x        | x            | Syscall          | Batched, events | x                     | Only the NIC driver         |
> | DPDK       | x            | Yes      | x            | x                | Polling         | x                     | Support from the NIC driver |
> | Linux 2.16 | Shared       | x        | BSD sockets  | Syscalls         | Per-packet      | x                     | x                           |
> | Linux 3.19 | Per-core     | x        | BSD sockets  | Syscalls         | Per-packet      | SO_REUSEPORT          | x                           |
> | MegaPipe   | Per-core     | Yes      | lwsocket     | Batched Syscalls | Per-packet      | Yes, new API          | Yes                         |
> | mTCP       | Per-core     | Yes      | mTCP sockets | Batched Syscalls | Batched         | Minimum, mTCP sockets | No (multiqueue)             |


---
References: