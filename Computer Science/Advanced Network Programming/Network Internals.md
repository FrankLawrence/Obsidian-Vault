---
Tags: 
Created: 2024-10-08 14:43:37
---
(Links:: [[Advanced Network Programming]])
# Transferring data between the end host and the NIC

![[Transferring data between NIC and end-Host.png|400]]
- What can we do here?
  - CPU does polling (LLC <-> on broad memory)
  - CPU does data copy (LLC -> on broad memory -> DRAM)

> [!question]- Problems?
> - Waste of CPU cycles
> - High load on the CPU cannot do anything else

Instead of using expensive CPU cycles to transfer data to DRAM, the **DMA engine** is programmed to tell where to deposit the data. Interrupts are sent to the LLC when new data arrives, and when frame has been successfully sent to DRAM.

## [[Processes and Threads#^4c1449|What happens when there is an interrupt]]
1. Device raises interrupt request
2. Processor interrupts program in execution
3. Interrupts are disabled
4. Device is informed of acceptance and, as a consequence, lowers interrupts
5. Interrupt is handled by **service routine**
6. Interrupts are enabled
7. Execution of interrupted program is resumed

## Interrupt Storm
Imagine a situation where a CPU is constantly receiving interrupts:
1. The CPU gets an interrupt
2. It processes interrupts by executing ISR
3. Start normal processing
4. Interrupt again

No "actual" work progress can be made. The system is alive, but is **locked**: **livelock**. In comparison a **deadlock** is waiting for some resource to do the work. Interrupt storms often happen on the **receive path** because a NIC/system cannot control when to receive the packet (but it _can_ control when to transmit).
How many interrupts are sent is dependent on how many packets are received.

> [!example]
> A data packet is 84 bytes (64 bytes + 20 byte ETH header)
> - $1\; Gbps = \frac{84*8}{1} * 10^9 = 0.6 \text{ microseconds}$
> - $10\; Gbps = \frac{84*8}{10} * 10^9 = 67.2 \text{ nanoseconds}$ (close to DRAM access)
> - $100\; Gbps = \frac{84*8}{100} * 10^9 = 6.72 \text{ nanoseconds}$ (less than a DRAM access)

**At 100 Gbps the CPU will just take interrupts, and do nothing else**. If it cannot keep up, then the packets will be dropped. There are several *mitigations* for interrupt storms:
1. Interrupt coalescing
	- Don't generate interrupt on every packet, but "n" packets to amortize the cost of taking interrupt
	- A typical value depends upon (a) NIC buffering capacity; (b) network speed; and (c) accepted delay due to batching of "n" packets
2. Polling: Disable interrupts all together, and use CPU polling to check for new packet arrivals
3. Hybrid
	- In practice, a hybrid strategy of these two are used
	- Interrupts => Polling => Interrupts
	- There is a threshold, when the rate exceed then switch to polling, then to interrupts

> [!tip]- Want to compile and run your own kernel?
> Install from [here](https:://wiki.ubuntu.com/KernelTeam/GitKernelBuild)
> ```bash
> make olddefconfig
> make clean
> make -j $(getconf _NPROCESSORS_ONLN) deb-pkg LOCALVERSION=-anp23
> ```
> You will then get: `linux-image-<version>-anp23_amd64.deb` and `linux-headers-<version>-anp23_amd64.deb` files
> - Install with `sudo dpkg -i <deb_package>`
> - `sudo upgrade-grub2 && sudo sync`
> - `sudo reboot` 

**What NIC / driver do you use?**
1. Active NIC interfaces are displayed with `ip a`
2. Print driver with `ethtool -i <interface>`
## The device interface: Rings
This **producer-consumer ring pattern** is a very common data structure (and design pattern) in many areas in operating systems, virtualization, storage, networking, process-process communication etc.
![[Ring Device Interface.png|500]]
## Linux packet receive path
A networking device in Linux is represented by `struct net_device`
- Contains functions for all device I/O activities, management, bringing up and down the Device
- **Very very very large structure** ~ 350 LOC
- Makes the contract between a device driver and the rest of the networking stack in the kernel

1. Packet received
2. DMA to the memory using the next free ring buffer address
3. Raise an interrupt
4. Run IRQ handler
5. ACK and clear IRQ
6. napi_schedule

> [!danger] The problem with interrupts
> When processing an interrupt:
> - Interrupt handling is a **high priority work**
> - **All further interrupts are disabled** (on the *CPU*, not just the specific device which generated the interrupt)
> 	- If there are more packets coming in we will miss out on them
> 	- If ring buffer overflow - then packets are dropped
>
> - Many long events in the incoming packet processing - memory allocation, scheduling of application process to consume incoming data, TCP ACK generation, TCP transmission processing ... a lot of work.
>   -> We can not disable interrupt for this long to do all this working the interrupt handler

# NAPI
**New API** is the implementation of *interrupt mitigation technique* that we looked earlier (all info in `struct napi_struct`).
- By default interrupt: check if NAPI is already requested to be scheduled
- In the NAPI processing:
	- Interrupts on the NIC are disabled
	- Process certain number of "weight" packets in one go (is it still worth to do polling?)
	- Poll the device driver (by calling a driver provided function) to check if there are more packets ready for processing
	- If not enough packets available, then yield
  - The driver will enable the interrupt-driven notification again on the NIC

The main entry point for `NET_RX_SOFTIRQ` processing is: `statci void net_rx_action(strucdt softirq_action *h)`.

Polls the network device, where
- Packets are built (`skbuff`)
- Pushed in the netstack for processing
- Return the "budget" consumed
- If budget consumed, or time to reschedule: break
## SKB basic Idea
One of the most important data types in the Linux kernel. Represents a data packet in processing. Has headers, trailer, data, metadata, Linux specific details. Logically it contains
- link list pointers
- which netdev
- which socket
- place for headers/trailers for various protocols and headers
- various Linux specific accounting and reference counting information

# Top-Half and Bottom-Half Processing
- **Top-Half**:
	- Mostly cleaning hardware registers
	- Copying out pointers
	- Scheduling the bottom half, if not already there
	- These are *ISRs*
	- Very high priority, very small
- **Bottom-Half** (SoftIRQs)
	- Identify the memory where the packet was stored
	- Most of the protocol processing
	- These are *SoftIRQs*
	- High priority
	- Have interrupts enabled (can be preempted)
	- Can preempt other kernel threads, user processing

## Deferred Work Processing Framework
Linux Kernel has multiple mechanisms for deferred work processing.
- **SoftIRQs**: A specific number of predefined SoftIRQ / core for specific tasks (e.g. RX, TX, timer). *No blocking calls*. Concurrent execution of same type of softirq on different CPUs.
- **Tasklets**: A more flexible SoftIRQ that can be allocated and used by different kernel subsystems. No blocking calls. No concurrent execution of the same tasklet on different CPUs, serialized.
- **WorkQueues**: A kernel thread pool with an immediate or delayed work execution in a process contexst. Can have blocking calls. Managed by the kernel.
- **Kernel Threads**: A generic kernel thread, to be used for any purpose by the caller.

There is a priority with the SoftIRQs defined. Apart from softIRQs, there is also kthreads, workers, tasklets.

# Continuing SKB
## Driver polling function
Data packets are built and pushed into `netif_receive_skb();`. This does not do much, a bit of accounting, backlog processing, various tracking hooks before being delivered to the networking layer at `ip_rcv()`.
## IP processing
When `ip_rcv()` is called, we first deliver IP packets to the higher protocol layers with `ip_local_deliver()`, which then, if the ip_address matches, sends it to `ip_forward` where the packet is sent to `ip_rx()` where the data is processed.
If data needs reassembling, `ip_frag_reasm()` is called. Once the whole packet is ready, find the appropriate transport protocol function and call it.

## TCP
Once again, we move the data pointer within SKB to point to the TCP header field. If there is already a socket, we call the fastpath (`tcp_v4_rcv()`) to handle data. Other functions are called for when we deal with other types of TCP mechanisms.

Inside the data processing (`tcp_v4_do_rcv()`), the TCP State machine is processed, and there are specific function calls for certain states. 

Finally data is given to the use by adding everything to a receive queue. When a user application calls `recv()` then this queue is processed and consumed. If `poll()` -> signal that there is new data available.

The `recv()` system call, called from by a user thread, has to enter the kernel, but it is **not** a softirq (non privileged kernel execution).


---
References:
