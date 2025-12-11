---
Tags: 
Created: 2023-12-10 14:22:12
---
(Links:: [[Operating Systems]])
# Principles of I/O Hardware
I/O device made available to software via an interface. We distinguish between 2 types of interfaces:
- Block devices: Stores data as **fixed-size blocks**. Allows for random access of information and transfer of blocks.
- Character devices: All data is stored as a **stream of characters/bytes** not blocks. This means we cannot seek through the data but only read it **sequentially**.

It can sometimes be hard to distinguish which interface a device belongs to, and some do not fit in this model.

I/O devices have to communicate with the CPU, and this is done via a **device controller**. It offers an *electronic interface* in form of *I/O registers*. We can perform R/W operations on those registers to ask the controller to perform actions. The CPU sends commands to the controller registers via a shared *bus*.

We differentiate between two types of data transfer: Parallel and serial data transfer. With *parallel* data transfer, we have multiple pins that are used for data, for example the parallel port used 8 pins representing the individual bits in a byte. With *serial* data transfer, we have multiple data pins, but the data is transferred in time blocks.
We take a look at the most popular example of this: USB. Here we have *two* data pins; one positive and one negative pin. We use two pins to combat noise. Each time segment is roughly 83 ns in length and the two pins can set their voltages to be in one of three states:
- K: D+ is set to low, and D- is set to high
- J: D+ is set to high, and D- is set to low
- 0: Both are set to low

To send data, we start with a specific sequence of K and J signals indicating the start and a separate one to convey the end of the packet.

Now that we have physically connected the device, we need the software to be able to communicate with it. There are two approaches: **Port mapped I/O** and **Memory mapped I/O**.

With port-mapped I/O, I/O registers are accessed via dedicated port numbers and special instructions.
With memory-mapped I/O, I/O registers are mapped into addresses of main memory and accessed like memory.
Hybrids of these methods exist.

> [!question] What mechanism (PMIO vs. MMIO) is more flexible and efficient to implement device access control for unprivileged user processes?

## Waiting for I/O

When we send a command, we expect a response, but if it takes time we have to wait an poll a status bit for when it changes to finished. This wastes a lot of CPU cycles. In comparison if we use **interrupts**, we won't have to wait for a change. Device controllers trigger an interrupt to signal that an I/O request is complete. Each controller has an **interrupt vector**, and the CPU runs a specific handler when the event occurs.
When the I/O request doesn't take that long, then it's better to use polling since it's faster.

Interrupt terminology:
- **Trap**: Deliberate action from a program
- **Fault (exception)**: usually not deliberate
- **Hardware interrupt**: device such as printer or network sends a signal to the CPU

Interrupts can be **precise** or **imprecise**. In the case where it is precise, there are four properties:
- The PC saved in a know place
- All instructions before that pointed to by PC have fully executed
- No instruction beyond that pointed to by PC has been executed
- Execution state of instruction pointed to by PC is known

![[Precise vs imprecise interrupts.png|500]]

Generally, if we want to transfer data from a hard disk to memory, the CPU has to work hard, accepting interrupts and handling on them. For this reason the DMA (Direct Memory Access) controller takes most of the burden off of the CPU.
![[DMA controller.png|600]]
On PCI(e) systems each PCI device may become "Bus Master" and perform DMA (**first-party DMA**)
- Device and DMA controller are combined
- **You have to trust your hardware**, since devices will have access to memory. The device has access to the virtual memory, so the CPU can change permissions for the I/O MMU (Memory management unit)

> [!question] You're comparing the performance of two systems, both using no interrupts for device data transfer, but one using DMA + polling while the other system uses polling alone. Do you expect comparable or different performance for the two systems?

# Principles of I/O Software
Goals:
- Device independence: I/O software should be able to handle different types of devices and provide abstractions
- Uniform naming: We want an abstraction as to not have to address a device by it's name
- Error handling: Errors should be handled closest to their source. Our software stack handles errors closer to lower level when errors arise there, since there is more information and we don't need to send the errors upwards.
- Buffering: We need to buffer incoming packets over a network until we have received the entire packet
- Synchronous versus asynchronous: Programs don't want to deal with interrupts so the OS turns asynchronous operations into blocking operations.

Because writing to I/O can take long, we use **interrupt-driven I/O**. Instead of polling the I/O device when it's ready, we take a byte of data and send it. When the device is ready to accept another byte it sends and interrupt. In the time in between, the CPU has enough time to switch process and do something else.

The I/O is built upon other layers:
1. Hardware
2. Interrupt handlers
3. Device drivers
4. Device-independent operating system software
5. User-level I/O software

These are the typical steps after a **hardware interrupt completes**:
1. Save registers (including the PSW) not already saved by interrupt hardware
2. Set up context for interrupt service procedure
3. Set up a stack for the interrupt service procedure
4. Acknowledge interrupt controller. If no centralized interrupt controller, re-enable interrupts.
5. Copy registers from where they were saved to process table
6. Run the interrupt-service procedure. It will extract information from the interrupting device controller’s registers.
7. Choose which process to run next. If the interrupt has caused some high-priority process that was blocked to become ready, it may be chosen to run now.
8. Set up the [[MMU]] context for the process to run next. Some TLB set- up may also be needed.
9. Load the new process’ registers, including its PSW. 
10. Start running the new process.

**Device drivers**, as mentioned before, sit between the CPU and the device. They accept abstract requests (such as a read request to the device) from device-independent layer and *transform them into commands for the device*. They can accept *multiple requests* and save them in a queue. We usually need to *wait for the completion* of a request before we can send a new command. After getting the response, it checks it for errors and then sends a response to the device-independent layer.

On top of the device drivers sit the **device-independent software** such as the [[File Systems|file system]]. These must implement the following functions:
- Uniform interfacing for device drivers: Driver interface, naming, protection
- Buffering: Necessary for both character and block devices
- Error reporting: Hardware-level, driver-level
- Allocating and releasing dedicated devices
- Providing a device-independent block size: Unify blocks/characters across devices

On the top most layer there is the **user-level I/O software**. This provides a uniform interface between user and the devices. This is commonly done with library interfaces such as C standard I/O library (`fopen`, `fclose`, `fflush`, `fprintf`). It also provides multiplexing to exclusive devices (such as printing multiple files).

> [!summary]
> ![[Layers of the I-O system and the main functions of each layer.png|600]]

> [!question] You're given a system where 5 out of 10 devices have soft real-time requirements. How would you design your interrupt handling logic to cater to these requirements?
# I/O Devices
Nowadays, there are generally two types of storage devices: **SSD**s and **HDD**s. 

|     | Power       | Heat        | Noise       | Vibration   | Weight      | Durability  | Speed       | Cost        | Capacity    |
| --- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- |
| SSD | `fas:Check` | `fas:Check` | `fas:Check` | `fas:Check` | `fas:Check` | `fas:Check` | `fas:Check` | `fas:Times` | `fas:Times` |
| HDD | `fas:Times` | `fas:Times` | `fas:Times` | `fas:Times` | `fas:Times` | `fas:Times` | `fas:Times` | `fas:Check` | `fas:Check` |

**Solid-State Drives** are purely electronic and have the following functionalities:
- DRAM or NOR/NAND flash memory based
	- No mechanical parts (only controller+memory)
	- Can be used as part of SSHDs
- NAND flash memory
	- Organized in cells, pages, blocks
	- Controller can read/program at page granularity
	- Controller can erase at block granularity
	- RND/SEQ access latency comparable
	- Reads more efficient than writes
	- Wear levelling to improve write endurance
	- Garbage collection to implement write operations
 
**Hard Disk Drives** on the other hand use magnetic disks to store data. In a single hard disk drive we can store multiple **platters** with as many tracks as heads. There are two different types of addressing modes: **CHS** (Cylinder-head-sector) and **LBA** (Logical Block Addressing). Each platter is divided into tracks and each track has multiple sectors.

When reading or writing to a hard disk, the majority of the time is spent seeking (moving the arm to the proper cylinder and rotating the sector to come under the head). The most basic scheduling algorithm is **shortest seek first** and is a definite improvement over first come first serve. It can however lead to starvation if there are a lot of disk addresses close to each other.
![[Shortest Seek First.png|700]]
The **elevator algorithm** is an improved version of shortest seek first. It moves like an elevator going all the way up and then back down.

When writing or reading from a hard disk, we can come across a few different types of errors:
- Programming Errors: Device driver has a bug and requests for nonexistent sector
- Transient Errors: Cause by dust on the head (trying again might succeed)
- Permanent Errors: Disk block physically damaged
- Seek Errors: The arm was sent to cylinder 6 but it went to 7
- Controller Errors: Controller refuses to accept commands

On some hard drives, when a sector fails (maybe due to damage), it is possible to remap this sector to spare one. 
![[SCR-20231210-rarm.png|600]]
a) A disk track with a bad sector
b) Substituting a spare for the bad sector
c) Shifting all the sectors to bypass the bad one

Each computer has a clock which oscillates millions of times per second. We can use advanced programmable clocks that decrement a counter in a register to send a hardware interrupt in known periods. Using a 16 bit register, for example, we can set a timer between 1 and 65536 $\mu s$ if the oscillator has a frequency of 1 MHz. The clock usually has to fulfil a couple duties: 
1. Maintaining the time of day
2. Preventing processes from running longer than allowed
3. Accounting for CPU usage
4. Handling alarm system call from user processes
5. Providing watchdog timers for parts of system itself
6. Profiling, monitoring, statistic gathering

**Soft timers** avoid interrupts. Instead, whenever the kernel is running for some other reason, just before it returns to user mode it checks the real-time clock to see if a soft timer has expired. Soft timers stand or fall with the rate at which kernel entries are made for other reasons. These reasons include:
- System calls
- TLB misses
- Page Faults
- I/O interrupts
- The CPU going idle

One of the most popular windowing systems is the X window system. It has a client and server part, with some message types:
1. Drawing commands from program to workstation
2. Replies by workstation to program queries
3. Keyboard, mouse, and other event announcements
4. Error messages

---
References: [[Chapter 05 - Input-Output Alt 5-35.pptx.pdf]]