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

## Inerrupt Storm
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

**At 100 Gbps the CPU will just take interrupts, and do nothing else**. If it cannot keep up, then the packets will be dropped.

---
References:
