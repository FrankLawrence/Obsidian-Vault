---
Tags: 
Created: 2026-06-15 23:27:18
---
(Links:: [[Advanced Computer Architecture]])
# The classical approach to confidentiality and integrity
The key to isolation across processes is the creation of a memory indirection
– Processes “speak” in terms of virtual memory addresses (conventional addresses defined independently per process) and they need ultimately to be converted into physical addresses (the ones used on the electrical buses to the memory chips)
– A central trusted entity (e.g., the OS) is charged of the allocation of these virtual memory addresses and of the translation
– Isolation is achieved by the trusted entity allowing only translations compliant with the desired isolation property
## Processor Privilege Levels
The key to the ability of limiting the possible translations depends on the existence of processor privilege levels
- Some instructions can be executed only in some privilege levels
- Instructions lowering the privilege level do not need to be restricted to a particular level: there is no harm in deciding that one can do less
- Critical is the mechanism to raise the privilege level, of course
	- Link raising the privilege level to a predefined change in control flow (i.e., some form of jump): if the privilege level raises, only some specific code can be executed
	- Usually in the form of a software exception instruction: raise the privilege and then raise the exception to execute the exception handler
	- If the virtual memory mechanism has been used well to protect the exception handler code, there is confidence that when the privilege level is high, only the OS can be executing

## Classical Privilege Levels
- Traditionally, multiple privilege levels (or rings) with varying capabilities tuned to some particular purposes
- Lower levels (or inner rings) add to the capabilities of levels above (or rings outside)
- In practice, most processors evolved to have only two privilege levels: user mode and kernel mode (names vary)

![[Permission Levels Ring.svg|500]]

## Virtual Machines 
- At the turn of the millennium there started to be (renewed) interest in hosting virtual machines (complete OS and applications) inside another OS and, in particular, inside a dedicated monitor (**hypervisor**)
- In particular, **full virtualization**: run the very same OS and applications in the virtual machine that one would run on the bare hardware
- Many reasons:
	- **Consolidation** of multiple small machines in a powerful one (lower cost and energy)
	- **Flexible deployment** (no need to buy a machine upfront)
	- **Lower dependence** from the hardware details (easy to move across servers)
	- **Better isolation** (not processes of the same OS but different OSes)

### Software-Based Virtualization
- Mostly, the ingredients for process virtualization enable also full virtualization:
	- Memory is accessed via TLBs, violations results in exceptions being raised, etc.
- Achieving full virtualization on a CPU not meant for it is challenging:
	- If guest OSes need to be isolated, they cannot run all in kernel mode
	- But if guest OSes run in user mode, how can they do their job?!
- The classic approach is called trap-and-emulate:
	- Guest OS will create exceptions when trying to do its normal job (loading a TLB)
	- Hypervisor will check the pertinence and, if appropriate, emulate
	- Many key data structures will be replicated (shadow page tables)
- But some instructions simply behave differently in user and kernel mode!
	- **Dynamic Binary Translation** (remember?!...) to rewrite the functionality with user mode instructions
- VMware achieved full software virtualization in 1999 (its author is not too far away…)

### Hardware-Assisted Virtualization
Around 2005-06, both AMD and Intel introduced *ISA extensions and hardware support* for full virtualization and progressively extended it
- **More privilege levels** (Ring -1, Hypervisor)
- **Another level of address translation** (nested paging) supported by the hardware page walker
- Interrupt virtualization
- [[IOMMU]] virtualization
## High-Privilege Levels
![[Higher-Privilege Levels.svg|400]]
- **System Management Mode** (Ring -2)
	- First introduced by Intel and now in all x86 processors
	- Guarantee some management functionality in firmware even if the OS or the hypervisor are compromised; accessible by dedicated interrupts
	- Mostly used for power and thermal management or handling hardware errors
- **Platform Security Engine** (Ring -3)
	- Intel’s Management Engine (ME) or AMD’s Platform Secure Processor
	- **Physical isolation** through a piece of hardware independent from the processor
		- Power up and down the processor, network connected, reserved main memory, etc.
- Not just more levels but **dedicated hardware** and **physical isolation**
	- FSM or small processor independent of the main cores
		- Intel: ARC (from ARC International, now Synopsys), Quark
		- AMD: ARM
- Largely implement [[Security by Obscurity]]

# The Universal Ingredients of (Hardware) Security Recipes
# Trusted Computing Base and Trusted Execution Environments
- The set of trusted hardware and software components which can be object of an attack
- Important: what is **trusted** is not necessarily **trustworthy**!
- The purpose is to separate clearly
	1. what is *supposed to be trustworthy* and simply may not be such because of *bugs or conceptual oversights* from
	2. what is *clearly untrustworthy* and against which the system has been designed with explicitly defenses

> [!important] Make the TCB small
> The smaller it's surface area, the less likely it is for the attacker to be able to get access to other components.

The TCB evolves mainly due to new business models such as cloud computing: The cloud users trust their own apps, OS and processor manufacturer, but not the cloud operator. Thus also means that the *computer hardware* (such as the bus) is not trusted.

The goal is to create an environment where *only protected software resides and executes*, supported by a *minimal TCB*. These are known as **trusted execution environments**, and the main challenge is to protect the software state of the TEE given the fact that its state is unavoidably *dispersed all over the system* and specifically outside of the TCB and inside untrusted software and hardware components. 

There are many approaches to secure confidentiality between a component inside and outside the TCB, such as *encryption*: Symmetric encryption ensures confidentiality outside the processor, but this implies *hardware encryption/decryption modules* at the edge and locally stored keys. 
We can *isolate* data (e.g. using page tables) but **memory management cannot be under the control of untrusted entities**. TEEs and TCB hardware should be in charge of their own page management.
*Architectural and microarchitectural state* across all parts of the system need to be **flushed** before untrusted entities control the system (classic target of [[Side Channel Attacks]]). The challenge is to *identify all places* where there is confidential state.
Lastly we also need to guarantee *integrity* of data stored outside the processor, which can be enforced using *one-way hashing*. This usually implies *hardware modules* at the edge and *locally stored nonces and root hashes*.
# Intel Software Guard Extensions (SGX)
> [!caution]
> Execute *critical software on a remote computer owned and maintained by an untrusted party*, with some integrity and confidentiality guarantees

- Needs two fundamental properties
	- **Isolation**
		- Each secured environment is protected from all other software running on the machine (including OS, hypervisor, etc. and other secured environments)
	- **Attestation**
		- Provide a proof that the software running inside the protected environment is genuine and untampered
- TCB reduces to the CPU chip (hardware) and the critical code (software)

---
- Data owner *trusts the hardware* running in a remote computer operated by an *untrusted infrastructure owner*
- The trusted hardware establishes a *secure container (enclave)* and supplies the user with a *proof* that they are accessing a specific piece of software running into the enclave
- The data owner *uploads encrypted* data that the software in the enclave decrypts and processes
- The enclave software *encrypts the results* and sends them back to the data owner
- The system software of the *infrastructure owner* is in charge of managing resources and devices as in ordinary systems, but has *no access to the code and data of the enclave*

> [!info] Functionality of an Enclave
> 1. The remote user launches their untrusted app 
> 2. Untrusted code in the application, through an untrusted OS, asks SGX to setup the enclave and copy there code and data from unprotected memory; initial payload is unprotected 
> 3. Once done, the enclave is marked as initialized and the content is cryptographically hashed into a final measurement hash 
> 4. The remote user can undergo a software attestation process to obtain a proof, through the measurement hash, that the enclave is setup properly 
> 5. Trusted code in the enclave can now decrypt the payload, now protected by being inside the enclave 
> 6. Trusted code in the enclave can be invoked through mechanism similar to those used to switch to kernel mode 
> 
>  > [!question] What happens during exception?
> > Exceptions while executing enclave code are handled by SGX first (see later) to protect secrets

![[Intel SGX Physical Memory Organization.svg|700]]

- **PRM**: Processor Reserved Memory = pages reserved by SGX for enclaves
	- Defined in the [[BIOS]], adjacent power-of-two area of physical memory
- **EPC**: Enclave Page Cache = pages allocated by SGX for enclaves
	- Allocated by kernel or hypervisor, encrypted in hardware with keys generated at boot time
- **EPCM**: Enclave Page Cache Map = metadata of each EPC page such as valid, owner, etc.
	- Inside the processor, fixed size, limits the maximum EPC size (e.g. 128MB)

When an enclave tries to access memory the following two checks are tested:
- Is the address in the EPC? -> "Address in the page table?"
- Does it match the metadata in the EPCM -> "Check page table metadata"

Nothing is really surprising, this is the classic job of the OS or hypervisor that is now done at the physical page level by the hardware (through special instructions). The OS code is replaced by *processor firmware*, which is a smaller TCB, but it still follows [[Security by Obscurity]].

## EPC Management Pages
- Most pages in EPC are code or data of the enclaves
- Some pages are reserved for SGX management
	- **SGX Enclave Control Structures (SEC)** pages: enclave attributes, hashes for attestations, etc.
	- **Thread Control Structures (TCS)** pages: allow multiple threads to execute the enclave code concurrently
- These pages are neither accessible to hypervisors, OSs, etc. nor to the enclave code itself, only to SGX

> [!danger] Exceptions
> On an exception during the execution of enclave code, **SGX dumps the state in EPC pages linked to the TCS** and restores the application state (thus hiding the enclave state to the application), before executing the exception handler.
> Again, a bit more of the classic kernel job shifted into the processor (a pre-handler part of SGX).

## Memory Encryption and Integrity
![[Intel SGX Architecture.svg|400]]

- PC pages are **encrypted** by a hardware **Memory Encryption Engine (MEE)** so that no snooping or [[Architectural Support for Security#Cold Boot Attack]] can succeed
- MEE works at the resolution of *cache lines (512 bits)*
- MEE encrypts every piece of data in a protected region of untrusted memory but also spontaneously *maintains an integrity tree in untrusted memory*
- The root of the integrity tree is stored in protected memory inside the hardware TBC (processor chip)
## EPC Swapping
EPC is only a limited-size cache: *enclave pages* may need to be *swapped out to non-EPC memory* (Non-EPC memory is *unsecure* and replay attacks my happen). On EPC swap out we, decrypt the page, re-encrypt again with *versioning* information to ensure freshness and *sign* it to check for integrity, and save it to non-EPC memory together with *128 bytes of metadata*. Versioning information is saved in *dedicated EPC management pages*.

When storing the hashed payloads, we make use of an *integrity tree*:
- `Tagx` is the digest of data D0 together with nonce `n0x`
- `Tag0x` is the digest of nonces n00-n04 together with `n1x`
- The tree root nonce is secure because stored in memory internal to the processor
- Read and verify:
	1. All tags can be computed independently and in any order
	2. If any check failed, integrity compromised 
- Write and update:
	1. Preemptive check to avoid replay attacks
	2. Update, increment nonces, and recompute tags
	3. Write tags

In the end, we can use 3/4 of our memory for data, the rest is for metadata. The overall performance overhead of MME has been measured to be 2-14%.

## Root of Trust
- Security requires many *different cryptographic keys* for multiple purposes
	- Private and public keys for authentication
	- Secret keys for confidentiatliy
	- Keys for integrity checks
- Some can be *random and ephemeral* (e.g. for encrypting data into DRAM) -> generated at boot
- Most need to depend on a something *unique and persistent*: a root of trust key stored in the processor and accessible only to the TCB
	- **Root Provisioning Key (RPK)**, stored by Intel
	- **Root Sealing Key (RSK)**, that Intel declares to erase after manufacturing
- Classic security issues: [[Public Key Infrastructure]], [[WebPKI Certificate Authority.svg|Certificate Authority]], revocation

# ARM TrustZone
- A very different system from Intel SGX
- The basic business model and market is very different: **ARM does not build chips but licenses Intellectual Property**; many customers only license the architecture and build the processor themselves (e.g., Apple)
- TrustZone is a collection of hardware mechanisms which conceptually **partition a system and its resources in a secure and a nonsecure world**
## Extended Privilege and Memory Isolation

- Essentially, introduces a secure/nonsecure partition mode orthogonal to the classic privilege levels (Thread/Handler)
	- Fairly classic register banking and duplications—e.g., four copies of register R13 (stack pointer) instead of only two
- Memory split in three classes
	- **Secure** and **Nonsecure**
	- **Secure but callable from nonsecure** code (a special API is responsible of the return to the nonsecure world)
- Hardware **Attribution Units** and **Protection Controllers** intercept addresses to memory and raise exceptions in case of violation
	- Much simpler than SGX enclave accesses, but conceptually similar
	- Limited number of secure regions (e.g., eight)

## Calling Restrictions
- Branching into nonsecure callable region *checks that the first instruction at the destination* is a *Secure Gateway (SG)* instruction which sets the processor in secure mode; *special branch instructions* to return from secure to nonsecure
- The nonsecure callable region is a *bridge to call secure code*, not callable directly
- Alternatively, the more classic privileged instruction *Secure Monitor Call* (SMC) jumps into the monitor

![[Pasted image 20260618172904.png|500]]

- System components receive a special AXI bit as a sort of extension of the address and can thus be TrustZone-aware (in *red* on the figure)
- Some busses omit the TrustZone secure bit in the bus address (in *orange* on the figure)
- Since there is no ARM chip but only some architectural definitions, the security *properties depend on the actual system design* on the ARM licensee and on the details of all components
- The **secure world** contains several necessary components
	- A trusted boot loader (hardware reset -> secure mode)
	- A small trusted OS
	- A monitor to switch back and forth from the nonsecure world
	- Security critical applications
- The secure monitor has unrestricted access to the nonsecure world
- ARM provides *reference implementations* of secure firmware inclusive of secure boot services and the secure monitor
- **No security by obscurity** on the software side (at last not from ARM, but probably quite a bit by the system designers e.g. Apple)

# Conclusions
- There is definitely no magic one-stop solution for all security troubles
- Rather, we see an *enormous variety* on what TEEs are and what they are expected to protect from (= huge variety of quickly evolving business needs)
- The sole clear and sound motto appears to be "reduce the attack surface to the bare minimum"
- Focus appears to be mostly in *glorified versions of classic isolation mechanisms and classic security protocols* (e.g. attestation schemes), but also in some new unconventional features (e.g. memory encryption and integrity in SGX)
- The apparent *complexity* of some of these solutions seems alone and by itself a form of *fragility*
- Commercial systems show *very little or no protection* from advanced microarchitectural and physical *side-channel attacks*
- Still a new and quickly evolving aspect of computer architecture which will *need some time to reach maturity* and some form of standardization

---
References:
- [[Trusted Execution Environments]]
- Costan, Victor and Srinivas Devadas. “Intel SGX Explained.” _IACR Cryptol. ePrint Arch._ 2016 (2016): 86.