---
Tags: #lecture
Created: 2026-01-09 23:33:55
aliases: TEE
---
(Links:: [[System Security]])
# Trusted Computing Primitives
- **Trusted Boot**: Prove to *remote party* that SW you have loaded on your machine
- **Remote Attestation**: Perform a *measured launch* of the OS kernel/VMM
- **Sealed Storage**: Protect *sensitive information* by binding it to the platform & release it (allow it's decryption) only under a specific SW & HW combination
- **Isolated Execution**: Ensure *sensitive code* runs in isolation from other *malicious programs* that are running on the *same machine*

Trusted Execution Environments were developed to sandbox a confidential application, such that other untrusted sources (such as other applications, or the OS) cannot gain access to the private information.
Just like how a [[Hypervisor]] needs to restrict the [[Virtual Machine|VMs]] from each other, the OS needs to *isolate processes*. This is done using *page tables* for each process to restrict memory access.
This same idea can be applied to hypervisors and their VMs:

![[Hypervisor Page Tables.svg|700]]

> [!question]
> The OS programs:
> - VA -> GPA
> - VA -> PA
> - GPA -> PA
> - PA -> VA
> 
> Hypervisor programs:
> - VA -> GPA
> - VA -> PA
> - GPA -> PA
> - PA -> VA

# Confidential Virtual Machines (CVM)
- Design 1: Hard partition
	- Create a trusted counterpart (Hypervisor) to the hypervisor that allows us to create and manage CVMs
	- Split all resources between hypervisor and trusted hypervisor to be used for VMs and CVMs
	- *Problems*
		- Replicate management in both hypervisor and trusted hypervisor
		- Cloud service provider loses control of some resources
		- Might not be ideal for resource allocation
- Design 2: De-Privilege Hypervisor
	- Hypervisor split into a trusted shim and untrusted host
	- Hypervisor creates, configures, and schedules VMs 
	- Trusted shim enforces security by monitoring hypervisor's configuration changes (e.g. memory mappings)
	- Machine can execute many CVMs, but only *one untrusted VM*

> [!example] Android
> Android deploys pKVM as a trusted shim in hypervisor mode. Trusted services execute in CVMs to isolate them from potentially compromised Android (e.g. update compilation, model updates, passkey)
> -> Concept may work for phones, but not scalable in cloud

- Design 3: *Equally privileged hypervisor and trusted shim*
![[CVM Architecture.svg|400]]

> [!question] How do we isolate VMs and CVMs?
> Using Stage-2 Translations just like [[Hypervisor Page Tables.svg|before]], only this time for each hypervisor seperately.

- trusted shim and untrusted hypervisor program stage-2 translations for their own VMs
- CVM/VM Stage 1: Configured by CVM/VM itself
- CVM/VM Stage 2 and Hypervisors stages 1: Configured by trusted/untrusted hypervisor
![[CVM Stage-2 Translation.svg|600]]

> [!failure] Hypervisor can access CVM's physical memory directly
> A solution to this, is to create a barrier, which prevents such memory mappings
> ![[CVM Barrier.svg|900]]

> [!question] Should the trusted hypervisor perform resource management for CVMs and the hypervisor for their VMs?
> No, since the trusted hypervisor would overpower the hypervisor, but we require equal privileges. Instead, *delegate intensive tasks to hypervisor, lightweight checks in shim*.

## Creating a Non-confidential VM
1. Starting point: Hypervisor runs and has a stage 1 page table mapping already set up
2. Hypervisor reserves memory for new $VM_{1}$
3. Hypervisor creates stage 2 page tables for $VM_{1}$
4. Hypervisor informs $VM_{1}$ OS of its GPA memory range
5. $VM_{1}$ starts and sets up its own stage 1 translations
![[TEE Non-confidential VM.svg|800]]

## Creating a Confidential VM
1. Starting point: Hardware always isolates trusted memory from hypervisor
2. Hypervisor and Shim have set up their stage 1 page tables
3. Hypervisor allocates memory for a VM
4. Hypervisor transfers VM memory to Shim
   **Check**: all memory is scrubbed before transferring to Shim
5. Hypervisor creates stage 2 page tables for the future CVM
6. Hypervisor transfers stage 2 page tables to shim for its use on CVM
7. **Shim checks page tables of CVMs for overlaps**; if none are detected, it enables their use for the newly formed CVM
8. Hypervisor creates information for CVM's memory range, passes it securely to the shim
9. **Shim checks GPA information** and passes it to CVM
10. CVM starts and sets up its own stage 1 page tables

![[TEE Confidential VM.svg|1000]]

> [!summary]
> - Shim and hypervisor have separate physical memory spaces that prevent cross-boundary memory accesses
> - Hypervisor prepares launch of new CVM by reserving memory and creating stage-2 page tables
> - Shim changes memory affiliation of CVM pages and receives stage-2 page tables
> - Shim checks both to eliminate malicious mapping attacks during CVM creation time
> - After all checks pass, shim starts execution of CVM
> ---
> - Untrusted hypervisor has memory reserves, it can create non-confidential VMs directly
> - Trusted shim lacks free memory and sophisticated allocation functions
> - It turns to hypervisor for heavy lifting by reserving memory and creating structures
> - Shim verifies that memory-related operations of hypervisor were executed correctly before starting CVM

## Shim Memory Checks
> [!question] Why does the shim need to check memory that is transferred from hypervisor and the stage 2 page tables?
> - Assume: Hypervisor creates $CVM_1$ and $CVM_2$ stage 2 page tables without overlays.
> - Attack: Hypervisor creates $CVM_2$ stage 2 page table that has overlaps with $CVM_1$. If shim does not detect that it would compromise $CVM_1$'s security guarantees

# Arm Confidential Computing Architecture (CCA)

> [!question] Why do we need to build new TEEs?
> - We want to add features (e.g. memory sharing)
> - We want to support Non-CPU TEEs (e.g. FPGAs, GPUs, network cards)
> - We want to add defenses (e.g. protection from nation-state adversaries)
> - We want to create a new eco-system (e.g. sovereign smartphone)

## CCA with Realm Management Extension (RME)

![[Worlds in ARM CCA.svg|800]]
- Features of Arm CCA
	- Full software stack
	- Isolation from the host OS and hypervisor
	- Hardware-based security mechanisms
	- Realm can contain a full OS stack
	- Attestation
	- Memory encryption
- Isolation in Arm CCA
	- **Granule Protection Table** (GPT) defines safeguards between realm memory and hypervisor (real world)
	- GPTs mark arbitrary ranges of physical memory to belong to one of the 4 worlds (normal, secure, realm and root); smallest is 4KB (page size)
	- **Granule Protection Check** component *enforces* access rules
	- GPT *defines* mappings; GPC *enforces* access to memory regions
	- Pretty much all components are subject to GPC
- Intra-world isolation of Realm VMs
	- RMM programs stage-2 translation tables just like [[CVM Stage-2 Translation.svg|before]], from guest physical address to host physical address
	- RMM ensures intra-CVM (Realm VM) isolation: VMs cannot have the same host physical page mapped into their GPA space, if it's a realm-world page
- Protection from physical adversaries
	- RMM can request per-CVM memory protection key
	- Hardware memory encryption engine: confidentiality and integrity
	- Encryption-decryption on the bus between CPU and [[DRAM]]
## Arm CCA Memory Encryption

- Memory Encryption Context (MEC)
- Provides per-VM or per-memory unique encryption context
- Assigned by trusted software (RMM)
- Uses system registers and page table bits
- Can setup shared keys between VMs

# RISC-V Keystone
- [[RISC-V]] specifications are permissive open-source licenses: implement without paying royalties
- Platforms: [[QEMU]], FPGA, Emulators
- Software support: Compilers, Kernels, libs
- Easy to prototype based on open-source cores: Rocket & Boom, Riscy00, CV6

RISC-V has three software privilege levels (in increasing order of capability): user-mode (U-mode), supervisor mode (S-mode), and machine mode (M-mode). The processor can run in only one of the privilege modes at a time.
- Higher privileges the closer to hardware
- No virtualisation or hypervisor technology
- During execution, processor cores are assigned to enclaves
	- Uses PMP registers to configure which Memory addresses the processor cores may access
- Security Monitor (RISC-V version of RMM)

![[Keystone Architecture and Trust Model.svg|1000]]
## RISC-V Physical Memory Protection
- PMP required for Keystone to isolate enclave memory regions
- PMP registers used by process to identify which memory locations are accessible (similar to GPC and GPT in Arm CCA)
- If no PMP entry matches with an address, the memory access will be rejected by default

1. Security Monitor first reserves space for itself with permission bits set to 0 (first PMP entry so it cannot be overridden)
2. SM sets the last PMP entry to cover the entire DRAM (permission bits to 1), allowing the OS to access the rest of the memory and start booting
3. When the SM enters an enclave, it flips the permission bits of the enclave’s PMP entry and the last (OS) PMP entry
	1. OS can create memory region as a communication buffer

- On *multi-core systems*, each core has its own PMP enforcement
- One or more cores can run OS, while the other(s) can run enclaves
- When a core changes PMP (e.g create/delete), SM sends *inter processor interrupts* -> other cores get notified of new PMPs and update locally (same for Arm CCA and GPT)
## Analysis of Keystone

> [!question]- Can Keystone create confidential virtual machines
> - There are two privilege levels you can execute on, which is necessary for OS and application
> - Intel SGX **cannot**

> [!question]- Who is in-charge of memory management and scheduling?
> Untrusted Operating System (just like Intel SGX)

> [!question]- Can we have flexible memory partitions?
> No
> > [!question]- Number of enclaves?
> > N-2, since one is for the SM, another for the OS
> 
> > [!question]- How is physical memory allocated to each enclave?
> > - OS has to create *contiguous* memory regions
> > - Arm CCA can pick *any* page

> [!question]- Is there a trusted shim to manage the enclaves? 
> The SM is responsible for a lot, but it doesn't setup and manage any page table translations.

> [!question]- Is there a stage-2 translation check?
> No, because RISC-V doesn't even support *virtualization*

> [!question]- What are we trading off?
> - In flexible memory allocation
> - Limited by number of enclaves (PMP entries)

# TEEtime
- **Motivation**: Phone manufacturers can observe all user data, and exploits cause data leakage of other applications
- Google created pKVM hypervisor
	- Creates VMs for hardware tasks/execution (e.g. access to peripherals) -> prevents access to other resources when bugs in OS are found
- Current TEEs are still *over-privileged*

> [!note] TEEtime Proposal
> - Simple and pragmatic for sovereignty
> - Doesn't disrupt existing OSes and apps
> - Hypervisor is not in the TCB
> - Only firmware modifications
> - Peripherals directly assigned to domains
> 	- Some peripherals are *exclusive* (touchscreen), other can be *proxy* (storage and network)
> - Isolation enforced by existing hardware

- We need the sovereign domain (e.g. running Signal messaging app) to have isolated memory, access to the touchscreen, access to network and access to storage
## TrustZone Address Space Controller (TZASC)
- TZASC component monitors data transferred between bus (CPU) and DRAM
- Supports multiple memory regions and access-control settings
- **Identity-based filtering**
	- Assign an ID to bus master (e.g. CPU, GPU)
	- Assign memory regions to specific bus masters
	- Append ID each memory bus transactions
	- Check accessor vs memory regions IDs
- Only firmware can configure region IDs
- Only limited number of regions

- **Challenge 1**: Execution and Memory Isolation 
	- We want that secure monitor *configures* isolation, hardware mechanisms *enforce* it.
	- Memory access based on core ID (may put other core to sleep)
	- Blocking access to memory of currently inactive domain
- **Challenge 2**: Peripheral Assignment
	- When to attach and detach a peripheral to a domain?
	- If and how to share a peripheral between domains?
	- How to transfer a peripheral between domains?
- **Challenge 3**: Peripheral Isolation
	- *Peripheral Access*: Read from and write to the memory-mapped peripheral address regions
	- Apply isolation at address-space level
- **Challenge 4**: Interrupt Management
	- Domain needs to configure, route, receive and handle peripheral interrupts
	- Cannot apply address space based isolation here
	- Isolate configuration setting and enable correct routing

> [!tip] Interrupt handling on ARM
> 1. Each core can configure the General Interrupt Controller (GIC)
> 2. All device interrupts are initially disabled
> 3. Core enables display interrupts with `active` bit
> 4. Display may now send interrupt signal, setting `pending` bit
> 5. GIC routes interrupts to *any* core to be handled
> 6. Interrupt `pending` bit reset

To enable interrupt handling on the GIC with TEEtime, we need additional `secure` and `route to` bits:
- CPU cores may *not* configure GIC
- Setting `secure` bit stops `active` and `pending` bits from changing
- Isolating routing: Set explicit interrupt routings

**Supported Peripheral Access Modes**
- So far: Peripheral can be owned by only one domain at a time
- TEEtime supports different modes of peripheral sharing:
  Exclusive, Handover, Multiplexing, Read Only, Proxy
- Secure Chat app example:
	- CPU + Memory -> Spatial
	- Screen -> Handover
	- Ethernet, Storage -> Proxy

> [!example] Sovereign Applications
> Modes:
> - exclusive (E)
> - handover (H)
> - proxy in legacy (P_l)
> - proxy in app (P_s)
> - multiplexing (M)
> 
> | Stakeholder  | Sovereign Application    | Peripheral (Mode)                           |
> | ------------ | ------------------------ | ------------------------------------------- |
> | User         | Secure Chat              | Display (E+H), Network (P_l), Storage (P_l) |
> |              | Secure Browsing          | Network (P_l), UART (M)                     |
> |              | Secure Data Vault        | Storage (P_l), UART (M)                     |
> |              | VPN                      | Network (P_s)                               |
> | Manufacturer | Device Status Check      | Network (E), Display (H), Button (H)        |
> | OS           | Biometric Authentication | Storage (P_l)                                            |

## Analysis of TEEtime

> [!question]- Can TEEtime create confidential virtual machines
> Management remains bare metal (no hypervisor), but existing code can be *ported*

> [!question]- Who is in-charge of memory management and scheduling of domains?
> The legacy domain

> [!question]- Can we have flexible memory partitions?
> No, only contiguous regions
> > [!question]- Number of domains?
> > Usually 9
> 
> > [!question]- How is physical memory allocated to each domain?
> > Domain asks for memory, which the legacy domain finds and then transferred to SM

> [!question]- Is there a trusted shim to manage the domain? 
> No stage-2 translations, so no

> [!question]- Is there a stage-2 translation check?
> No

> [!question]- What are we trading off?
> Same as with Arm CCA

# AMD SEV-SNP & Attacks
AMD Secure Encrypted Virtualization: Isolating entire VMs
- Design choices:
	- Isolate Virtual Machines from Hypervisor
	- VMs can execute code on ring 3,2, and 1 (SGX only supports ring 3)
	- Full Guest OS support
- Resists low skilled physical attacker
	- cold boot attacks, but not bus tampering
	- Everything outside the CPU die is untrusted

AMD SEV-SNP's **Secure Nested Paging**:
- Hardware creates exception when Hypervisor attempts to write to protected VMs memory
- Hypervisor receives *encrypted data*, when reading from protected VMs memory

AMD SEV-SNP's **Reverse Map Table**:
- The RMP table contains an entry for every DRAM page in the system
- Contains 4 metadata fields for each page:
	1. Physical page: The physical memory address of the page
	2. Hypervisor write: if it belongs to CVM, or can be written to by hypervisor
	3. Guest ID: Which VM owns the page
	4. Guest Physical Address: Maps physical address to guest physical address (inside VM)
- Indicates whether the hypervisor can write the page, and if it is assigned to an active guest
- Keeps entire table in protected memory -> no limitation to amount of CVMs

**Attestation**:
- Guest launch measurement
	- Hash of initial guest memory before VM starts
	- Signed by AMD-SP
- Guest secret injection: Only at launch immediately after verifying the measurement
==TOOD: diagram==

**Key management**:
- SEV uses an ARM co-processor to manage the different encryption keys for each VM
	- This is known as the AMD Platform Security Processor (PSP), and sometimes it is referred as AMD SP
- At VM creation, a new key is created on the co-processor to encrypt the VM memory
- Each new VM has a different encryption key

## Analysis of AMD SEV-SNP
> [!question]- Can it create confidential virtual machines? 
> Yes

> [!question]- Who is in-charge of memory management and scheduling? 
> Untrusted hypervisor

> [!question]- Can we have flexible memory partitions? 
> yes
> > [!question]- Number of CVMs? 
> > As many as want
> 
> > [!question]- How is physical memory allocated to each CVM? 
> > can be arbitrary mappings

> [!question]- Is there a trusted shim to manage the CVMs? 
> Yes, the PSP

> [!question]- Is there a stage-2 translation check? 
> Not explicitly

> [!question]- How is the hypervisor and CVM memory isolated from each other?
> Reverse map table

> [!question]- What are we trading off? 
> Performance overhead is very low

## Attack #1 on AMD SEV-SNP
- The hypervisor is in charge of *managing*, hardware isolates -> many forget the importance of the hypervisor

> [!danger] Interrupt Handling Attack on CVMs
>  - Hypervisor programs physical page fault
>    -> Interrupt from CPU causes CVM to exit
>    -> Data in cache can be read (timing side channel)
>  - Solution: *Virtual interrupts* can be handled by CVM

- Some instructions that a VM executes require assistance from the hypervisor (e.g. CPU id requires info on the physical CPU core)
	- Hypervisor cannot read and write to VM memory to update value!
- AMD SEV-SNP allows hardware to create special interrupt (`#VC`) which gives VM permission to handle the interrupt itself 
- Create *shared buffer* between hypervisor and VM to *read* and *write* to register

**WeSee Attack**:
- Hypervisor can *fake* the `#VC` interrupt **at any time**, and fake the `exit_reason` -> loss of integrity and confidentiality
- There are other interrupts which allow for *kernel memory read and writes*

*Impact*:
1. Leak KernelTLS keys: Session key stored in kernel memory
2. Disable `iptables` firewall: `iptable` rules to filter network traffic in the kernel
3. Root shell: Code pages in the kernel

## Attack #2 on AMD SEV-SNP
- **Legacy [[system call]] flow**:
	1. userspace program calls system call with instruction in `rax` register
	2. CPU switches to kernelspace in VM
	3. syscall updates userspace registers and memory (e.g. set return value in `rax`)
- We can abuse syscall executed in kernelspace if the hypervisor can set their own value for syscall instruction
	- Injecting values into `rax` larger than 385 will change `rax` return value to -4!

**Heckler Attack**:
- OpenSSH validates based on `rax`'s value
- Hypervisor sends *fake* interrupt to VM before branch condition
	- Hypervisor can change protected register state of CVM
- Attacker has to do this *exactly* after the `authenticate` call, but before the branch

For an attacker to implement this attack, they need to send the syscall *exactly after* the `authenticate` function call, which writes the return value to `rax`, but *before* the branch condition, so that it can change the `rax` value to what they want. The hypervisor cannot read the encrypted caches of the CVM, but it can find this specific moment by [[tracking the page-level execution state of CVMs]] which are unencrypted.
### More Interrupt-based Attacks
Signal Generating Interrupts:
- When the VM's kernelspace receives an interrupt from the CPU to handle, that originated from a user application (such as a divide by 0 error), it creates a fitting signal to be sent (e.g. SIGTRAP, SIGILL, SIGSEGV, etc.) to the app
- The hypervisor can create a fake interrupt that invokes the interrupt handler to send a specific signal, causing the code to change path

Malicious devices (I/O) interrupts:
- When a device is done computing, and storing data, it sends an interrupt to show that it has complete its operation (better than CPU idling)
- Hypervisor may send an interrupt while the I/O task has not complete, only having stored *part of the result* in the VM's memory
- Many devices do not blindly trust the interrupts, instead the **MMIO value must match**, which only the device driver can update
	- interrupt handler code is still executed, but halted

### Mitigations for Interrupt Attacks
- HotFix for WeSee: compare the **last instruction** in application with the `#VC exit_reason` to see if they match
	- This is not sufficient to stop all malicious `#VC` injections
- AMD SEV-SNP introduced *Interrupt Modes*:
	- Create a new VM called the *trusted bridge*
	- Create new interrupt (`#HV` Doorbell)
	- All interrupts are blocked, and doorbell interrupt must first be sent to trusted bridge (*restricted* mode)
	- Trusted bridge checks all interrupt IDs if there is a pending interrupt
	- In *alternate* mode, blocks all interrupts if it thinks it doesn't need devices interrupts for the specific task
## Attack #3 on AMD SEV-SNP
The AES engine integrated into the AMD System-on-Chip (SOC) automatically encrypts the data when it is written to the memory and automatically decrypts the data when it is read from memory. SEV hardware encrypts the VM's memory using 128-bit [[AES]] symmetric encryption.
Specifically it uses XOR-Encrypt-XOR encryption mode, which is gives the *ciphertext* given the *same plaintext* in the *same physical address* during the lifetime of a guest VM.
From AMD SEV-SNP's **Secure Nested Paging**, we know that the hypervisor can *read the encrypted ciphertext*
## Attack #4 on AMD SEV-SNP
- SPD chip on DIMM memory is responsible for advertising the size of the memory stick
- If SPD is not checked correctly, we can advertise a larger memory size
- Memory mappings will overlap, so that memory of the CVM is accessible from an untrusted VM

![[81318.png|600]]

# Comparison

## Overview & Architecture

|TEE|Category|Isolation Granularity|Privilege Levels|Hardware Requirements|Maturity/Availability|
|---|---|---|---|---|---|
|**Intel SGX**|Process-level|Individual processes (enclaves)|Ring 3 only (user mode)|Intel CPUs with SGX support|Mature, widely deployed (being phased out)|
|**AMD SEV-SNP**|VM-level|Full virtual machines|Rings 0-3 (full OS)|AMD EPYC CPUs (Milan+)|Production ready, growing adoption|
|**ARM CCA**|VM-level|Full virtual machines (Realms)|EL0-EL2 (full stack)|ARMv9 with RME|Announced 2021, hardware coming 2027|
|**RISC-V Keystone**|Enclave|Process enclaves|U-mode & S-mode|RISC-V cores with PMP|Research/prototype stage|
|**ARM TEEtime**|Domain-level|System domains|EL0-EL2|ARM with TrustZone features|Research prototype|

## Trusted Computing Base (TCB)

|TEE|TCB Components|TCB Size|Who Manages Security|
|---|---|---|---|
|**Intel SGX**|• CPU hardware<br>• SGX microcode<br>• Enclave code itself|Minimal (~40KB for SDK)|Hardware + enclave developer|
|**AMD SEV-SNP**|• CPU hardware<br>• AMD PSP (Platform Security Processor)<br>• VM guest OS/apps|Medium (~100KB PSP firmware)|Hardware + PSP firmware|
|**ARM CCA**|• CPU hardware<br>• Root firmware (TF-A)<br>• Realm Management Monitor (RMM)<br>• Realm VM OS/apps|Medium-Large (~200KB+ firmware)|Hardware + RMM|
|**RISC-V Keystone**|• RISC-V hardware<br>• Security Monitor (SM)<br>• Enclave runtime<br>• Enclave code|Small-Medium (~50-100KB)|Hardware + SM + runtime|
|**ARM TEEtime**|• ARM hardware<br>• Secure Monitor firmware<br>• Sovereign domain apps|Small (~50KB firmware)|Hardware + firmware|


---

## Key Takeaways

1. **Process vs VM Tradeoff**: SGX/Keystone offer smaller TCB but limited functionality; SEV-SNP/CCA offer full OS support but larger attack surface
    
2. **Memory Model**: Flexible allocation (SEV-SNP, CCA) beats fixed/contiguous (SGX, Keystone, TEEtime) for practical deployments
    
3. **Interrupt Vulnerability**: VM-level TEEs (SEV-SNP, potentially CCA) share similar interrupt-based attack vectors
    
4. **Physical Security**: All struggle with sophisticated physical attacks; encryption helps but isn't complete solution
    
5. **Deployment Readiness**: SEV-SNP is production-ready now; CCA is future; SGX is legacy; Keystone/TEEtime are research
## Questions
> [!question] Looking at the ARM Realm Extensions, Mallory says: “Why do we even need the realm state? We could just run Realms (VMs in realm state) as VMs in the non-secure state on top of a hypervisor alongside the host OS. As we rely on a hypervisor in the realm state to isolate different Realms anyways, a Realm’s TCB would not be bigger in such a setup.” Give two reasons why Mallory is wrong and a Realm’s TCB is in fact smaller than the TCB of a VM running in non-secure state.
> One reason is that realm hypervisor (RMM) typically has a smaller codebase than the normal world hypervisor because they are designed to be small and more easily verifiable.
> Another reason is that, if realms were deployed into the non-secure world, they are isolated only by the (untrusted) hypervisor. Hence, other VMs, OSes and applications inside the non-secure state might potentially break this isolation, so they need to be considered part of the TCB as well. 
> A third reason is that in ARM TrustZone, secure world can access non-secure world memory by design. Hence, we also need to include the whole secure world into the TCB of our realms, effectively rendering them ineffective.


---
References:
- https://docs.keystone-enclave.org/en/latest/Security-Monitor/index.html