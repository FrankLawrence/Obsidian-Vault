---
Tags: #lecture 
Created: 2026-01-08 15:01:42
---
(Links:: [[System Security]])

Some system security mechanisms to mention:
- Hardware roots (TPM/SE/[[Trusted Execution Environments|TEE]])
- Secure Boot
- Kernel hardening
- Process/app sandboxing
- Runtime exploit mitigations
- User-facing protections (permissions, biometrics, encrypted storage)

Note: No single control is sufficient - defence-in-depth (compartmentalisation) is the rule!

> [!summary]- System Security Mechanisms - ChatGPT
> - **User vs kernel separation (privilege rings):**  
>     Keeps user apps isolated from the kernel so buggy/malicious apps can’t directly modify core OS state.  
>     All desktop/mobile OSes (Windows kernel mode vs user mode; iOS/Android use Linux/Unix style privilege separation).
> - **Process isolation / sandboxing:**  
>     Each app runs in its own process space with limited resources and IPC rules to reduce blast radius.  
>     Examples: iOS app sandbox + entitlements; Android app sandbox (unique UID + SELinux); Windows AppContainer.
> - **Least privilege / permission model**  
>     Apps request only the permissions they need (runtime permissions on mobile; UAC / app manifests on desktop). Examples: Android runtime permissions, iOS privacy permissions, Windows UAC prompts.
> - **Code signing & executable integrity / App store vetting / runtime monitoring**  
>     OSes require/sign/verify signed code before loading to prevent tampering and untrusted binaries. Examples: iOS requires App Store signing, Android (Play-signed or OEM-signed), Windows driver signing and SmartScreen/Code Integrity.  
>     Server-side vetting plus on-device scanning to block known malicious apps.  
>     Examples: Apple App Store review, Google Play Protect, Microsoft Store scanning.
> - **Secure boot / measured boot / chain of trust**  
>     Boot firmware verifies the OS loader and kernel so an attacker can’t persist below the OS. Examples: UEFI Secure Boot + TPM on Windows/Linux; Apple Secure Boot on iPhones.
> - **Hardware root of trust / secure enclave / TEE**  
>     A hardware-protected environment stores keys and performs cryptographic ops outside the main OS. Examples: Intel SGX/TDX, ARM SEV/SNP, TPM / TPM2.0 + Windows Hello; Apple Secure Enclave (iPhone); ARM TrustZone + Android Keystore.
> - **Full-disk / file-based encryption**  
>     Protects data at rest so disk theft doesn’t expose user data.  
>     Examples: BitLocker (Windows), FileVault (macOS), File-Based Encryption (Android), iOS Data Protection.
> - **Protected key storage / credential protection**  
>     OS has specialized APIs for storing secrets (never expose raw keys to apps). (APIs similar to HSMs / smartcards) Examples: iOS Keychain, Android Keystore, Windows DPAPI / Credential Guard.
> - **Runtime sandbox/containers & micro-VMs**  
>     Lightweight isolation for untrusted code (e.g., browser tabs, Office macros).  
>     Examples: Windows Defender Application Guard, Chrome sandbox, Android isolated processes.
> - **Secure update mechanism / signed updates / App/driver/module signing & load restrictions**  
>     OS and app updates are cryptographically signed to prevent malicious updates and rollback attacks.  
>     OS blocks loading unsigned drivers or kernel modules to prevent persistent compromise (driver signing, SIP, kernel module signing).
> - **Kernel hardening / mandatory access control**  
>     Extra enforcement inside kernel to limit what processes can do.  
>     Examples: SELinux (Android, many Linux distributions), AppArmor (some Linux), System Integrity Protection (macOS), Windows PatchGuard / Virtualization-based security (VBS).
> - **Authentication + multi-factor & biometric auth**  
>     Support for stronger auth methods and hardware-backed biometrics.  
>     Examples: Windows Hello, Face ID / Touch ID, Android biometric API, FIDO2/WebAuthn support.
> - **Network protections (firewall, VPN, TLS enforcement)**  
>     Built-in firewalls, system-wide VPN frameworks, and platform enforcement for TLS and secure sockets.
> - **System auditing & secure logging**  
>     Audit trails, event logs, and integrity protections for logs so security events can be analyzed.
> - **Exploit detection / anti-malware**  
>     On-device AV, behavior monitoring, heuristics, and cloud lookups to detect and quarantine malware. Examples: Windows Defender, Google Play Protect.
> - **Privacy controls & data minimization**  
>     APIs and system prompts that limit access to sensors, camera, microphone, and location. Example: Android Permissions ...
> - **Address-space layout randomization ([[ASLR]])**  
>     Randomizes memory layout to make memory corruption exploits harder.
> - **Data Execution Prevention / NX (DEP)**  
>     Marks memory pages non-executable so injected code cannot run from writable pages.
> - **Control-flow integrity & related mitigations**  
>     Techniques like Control Flow Guard (Windows), Pointer Authentication (ARM PAC on modern iPhones/Android phones), and similar reduce code-reuse and control-flow attacks.
> - **[[Stack Canary|Stack canaries]] / safe stacks**  
>     Runtime checks that detect stack-smashing / buffer-overflow attempts.

# Basics of System Architecture
Applications are managed by an Operating System. The operating system has the responsibility of managing hardware resources for each application, such as CPU, memory, and peripherals (network, etc.). Application security is concerned with the following requirements:
1. **Launch-time integrity**: "correct" application was started or loaded
2. **Run-time isolation**: No interference from malicious software & peripherals
3. **Secure persistent storage**

For one, having the OS manage security, is *flexible and scalable*; however, operating systems are *complex* and are *prone to bugs*.

**Intel Platform Overview**:
- *Processor*: contains one or more CPU cores
- *Memory*: stores runtime data and code
- *Chipset*: connects the processor to peripherals
- *Peripherals*: connect via various bus-interfaces to the chipset
# Memory
**Segmentation** is a mostly legacy feature, where memory references are composed of a *segment selector* and an *offset*. Each segment has a **segment descriptor** which defines the base address, limit (size), access rights and privilege level. To improve [[Memory Management#Design Issues for Paging Systems|performance]] for running processes, segment descriptors are ordered into one of two tables:
- Local descriptor table (LDT): Holds descriptors specific to one process or task
- Global descriptor table (GDT): Holds descriptors that apply system-wide (shared by all processes)

In 64-bit mode segmentation is *mostly disabled*. Only filesystem and GS segments are still used & repurposed for fast per-thread and per-CPU access.
Segment descriptors and page tables include bits that specify whether a memory page is accessible from *user mode* (Ring 3) or only from *supervisor mode* (Ring 0-2).

![[Permission Levels Ring.svg|600]]

On top of privileges, each process has its own [[Memory Management#Virtual Memory|Virtual Address Space]]. On context switch, the control register containing the physical base address of the page directory table (`CR3`) is changed. The kernel address space is protected with the *supervisor bit*.
![[Four-Level Page Tables.canvas|Four-Level Page Tables]]
## OS Security
As well as bringing performance improvements, paging has the security benefit of *process isolation & protection*, since **each process gets its own page tables**, giving each one a private virtual address space. 
Additionally, when translating the path from the top level page table into the physical frame number, the *[[Memory Management Unit]] performs the walk automatically*. The *combination* of the frame base and the offset makes the final physical address.

Security relevant data in page table entries:
- **Supervisor bit**: If set, this page is accessible only in ring 0 (*isolates OS from applications*)
- **RW bits**: to distinguish between read-only and writeable pages
- **Non executable bit** (NX): If set, the page is not executable (*prevents run-time code injection*)

Application misbehavior:
- Writing to protected register (e.g. `CR3`)
	- CPU throws exception
	- Exception handler in the kernel is called
- Accessing protected memory (e.g. supervisor bit set in page tables)
	- CPU throws exception
	- Exception handler in the kernel is called
- Never calling the kernel (attempted DoS)
	- Kernel registers frequent timer interrupts
	- Timer fires an interrupt every $X$ ms
	- Kernel interrupt handler is called
	- Kernel saves current execution state
	- Scheduler in the Kernel decides what to execute next
## Virtual Address Spaces
Due to the performance benefits of [[Memory Management#Virtual Memory|virtual memory]], the *entire kernel's virtual address space* was *permanently mapped into its address space*. Even though user-mode code can't access kernel pages, with *speculative execution*, we can transiently [[Meltdown|read data from those pages]].
The solution is **Kernel Page-Table Isolation** (KPTI): when running user-mode code, remove kernel's virtual mappings from process's active page table.
When a user thread makes a [[system call|syscall]] (or an interrupt occurs):
- The CPU traps to the kernel entry point (in ring 0)
- The OS immediately switches `CR3` to another page table, the kernel-mode `CR3`, that includes:
	- The process's user-space mappings
	- The full kernel address space
- So there are now two `CR3`s per process:
	- User-mode `CR3` -> only user pages (no kernel mapping)
	- Kernel-mode `CR3` -> user + kernel mappings
- After the syscall finishes, the kernel switches back to the user-mode `CR3` before returning to user space.
## Memory: Peripherals
- Firewire uses [[DMA]] for faster speeds -> attacker can attack locked PC issuing DMA request to fetch RAM contents (DMA attack) -> leak keys and passwords
	- High-speed serial bus, designed for bus-mastering, isochronous data exchange for real-time applications
- *Mitigations*
	- Do not have Firewire ports
	- Use [[IOMMU]]
- **BadUSB**: change USB device controller to mimic another device class
	- Storage device acts as keyboard
	- Mouse acts as network card
- Thunderbolt can also be attacked using firewire adapter
- IOMMU
	- Converts device addresses (device-visible virtual addresses) into physical addresses
	- Controls DMA access to physical memory
	- Attempts to access unreserved memory addresses blocked

> [!example]
> 1. User requests OS to run ubs.exe
> 2. OS fetches ubs.exe, checks its hash and proceeds only if the has matches the expected value. It stores the hash on the HDD as a log.
> 3. OS maps ubs.exe to memory and sets up its page tables such that no other application can access its memory
> 4. OS sets up the IOMMU to protect against DMA-access from the malicious NIC


# Launch Time Integrity
- Privilege Rings
	- *CPU tracks current privilege level (CPL) using 2 register bits*
	- Main uses: limiting access to privileged instructions, I/O-ports

![[Permission Levels Ring.svg|500]]

[[system call|System Call]] Interface:
1. Before syscall, put arguments into registers 
2. Issue syscall -> CPU changes privilege level and calls *Hook*
3. *Hook* performs access policy checks and validates arguments
4. Kernel performs the requests action (e.g. read a file from disk)
5. Return to the application (via special return instruction)

```asmatmel
hook:
	/* validate arguments */
	...
	/* perform access policy checks */
	...
	/* Call the syscall_handler */
	call syscall_handler
syscall_handler:
	/* Print to stdout */
	...
	/* return to application */
	sysretq
```
The OS kernel does not necessarily have the lowest privilege:

| Level   | Example Use                          |
| ------- | ------------------------------------ |
| Ring 3  | User applications                    |
| Ring 0  | OS kernel                            |
| Ring -1 | Hypervisor (virtual machine monitor) |
| Ring -2 | SMM (Low-level firmware)             |
| Ring -3 | Management Engine (firmware)         |

## Intel Platform Overview
- **SMM** (System Management Mode) runs *inside the CPU* during runtime for power control, firmware tasks, and hardware events, invisible to the OS
	- Executes from SMRAM on SMI
	- Full RAM access, full kernel manipulation possible (if invoked)
- **Intel ME** (Management Engine) is a *separate microcontroller* inside the chipset that enforces security, implements firmware TPM (PTT) and provides remote management
	- Intel CSME is a management engine that runs on power-on, separate from the MCU
	- It configures boot guard and ensures the CPU cannot run unsigned firmware
	- DMA access to memory (full compromise possible)
- **BMC** (Baseboard Management Controller) is an *external management controller* mainly on servers, allowing remote power control, KVM, firmware updates, and monitoring even if the CPU is off
- [[UEFI]] (Unified Extensible Firmware Interface) is a modern replacement for legacy [[BIOS]] and serves as the **firmware-OS interface** (the first software that runs on power-on)
	- Runs at [[Intel Boot Process|boot]], initialized hardware, sets SMM handlers
	- Can compromise at boot
	- UEFI stays active after boot
-	TPM (Trusted Platform Module)
	-	Measurements, Crypto
	-	No direct access to memory/no direct way to compromise

| Component                 | Executes On   | Layer       | Always Running   |
| ------------------------- | ------------- | ----------- | ---------------- |
| Intel CSME / ME           | MCU in PCH    | Ring -3     | Yes              |
| Firmware TPM (PTT)        | MCU in PCH    | Ring -3     | Yes              |
| UEFI firmware             | Main CPU      | Ring -2     | Only during boot |
| SMM                       | Main CPU      | Ring -2     | Only on SMI      |
| Hypervisor                | Main CPU      | Ring -1     | If present       |
| OS Kernel                 | Main CPU      | Ring 0      | Yes              |
| User Space                | Main CPU      | Ring 3      | Yes              |
| Discrete TPM              | External chip | Out-of-band | If present       |
| Embedded Controller / BMC | External MCU  | Out-of-band | Yes              |

## TPM Support
- Main uses
	- *Secure Boot*: OS boots only if the chain of trust is valid
	- *Authenticated boot*: System records chain of trust but OS boots even if the chain is invalid
	![[Chain of trust.svg|500]]
	- Crypto. co-processor: e.g. key generation, encryption, decryption, hashing
	- Secure storage: e.g. non-migratable keys
	- Trusted log: hash-chain based log in the PCRs
	- Attestation: proving system state to another party using the trusted log, AIK

![[Root of Trust.svg|800]]

- TPM **Binding**:
	- Binding encrypts data using the TPM's *public key*
	- Ensures only the specific TPM can decrypt the data
	- *Ties data to the TPM hardware*, not to the system state
	- *Purpose*: Protects sensitive data on a specific device
- TPM **Sealing**:
	- Sealing encrypts data with system state conditions, defined by PCR values
	- Data can only be decrypted when the system is in a the same *trusted state* (same components and configurations when sealed)
	- Uses PCRs to measure and verify the system's configuration
	- Ensures data access only in a *trusted and predetermined system state*
	- *Use cases*
		- Disk encryption key protection
		- Secure credential storage
		- Software attestation (only verified system configuration can access certain secrets)

> [!example]
> 1. System boots
> 2. TPM measures BIOS, bootloader, OS, etc. and records hashes into PCRs
> 3. Application asks TPM to seal data with a key and set of PCR conditions
> 4. TPM encrypts data internally and stores sealed blob externally
> 5. When unsealing, TPM checks PCR and decrypts

# Below the OS: Some More Details

## Intel Management Engine (ME)
- Runs even when system is turned off (but connected to power)
- Runs on its own microcontroller within the Platform Controller Hub (PCH)
- Main functionality:
	- Remote out-of-band management (using Intel Active Management Technology; powerful remote access that circumvents OS)
	- Boot guard (integrity of the boot process)
	- Provision of cryptographic functions
	- System power management

*Physical access attacks* are the hardest to defend for an OS, since the bad actor could remove the hard drive, or boot from a USB key and copy data out/change the passwords. Some trivial solutions might be:
- Prevent booting from external sources
- Protect the [[BIOS]] with a password -> you can easily reset the BIOS using reset jumper, or removing the battery
- Partial/full disk encryption
- Data hiding

An alternative, is to use the newer [[UEFI]] which, on top of supporting larger drives and 64-bit, uses *Secure Boot*:
- Only allow booting trusted, signed bootloaders
- Minimal bootloaders were developed that bridge the gap -> Trust additional keys

# Runtime Integrity (Intel SGX)
## Isolation
Intel SGX provides isolation even against the compromised OS, with remote and local attestation and sealing using the TPM. These are known as **Trusted Isolation Environments**.
![[TEEs Trust Model.svg|800]]

- Create isolated environments at the application level (ring 3), called **enclaves**
- Enclaves are isolated from all the other software in the system OS/Hypervisor (ring < 3)
- Resists a physical attacker (cold boot attacks, bus tapping, etc.) by *only trusting the CPU die* and nothing else
- OS in charge of memory management ([[Memory Management Unit|MMU]]) and interrupts
- CPU tracks whether in *enclave-mode* and *which* enclave is executing through special structures inaccessible to software, but residing in memory
- **Memory reads to enclave memory when not in enclave mode, or from another enclave fail** with code `0xFF`; writes fail silently
- Enclaves can read/write the memory of their untrusted app (shared & untrusted memory)
- New component on the CPU takes care of securing memory the **Memory Encryption Engine** (MEE)
    − This is only needed against physical attacks (otherwise the other memory protection mechanisms are enough)
- The CPU is trusted, so data resides in the cache in plain-text
![[Intel SGX Architecture.svg|400]]
- [[Merkle Tree]] used to enforce integrity checks
	- CPU stores root of Merkle tree
	- Root hash is enough to enforce integrity of the whole tree
	![[Intel SGX Merkle Tree.svg|700]]


## Trusted Execution Environments
- [[Trusted Execution Environments]] are used to have some form of primitive trust
- Before using the environment, we need to prove that it not some malicious OS using either *Secure Boot*, or *Remote attestation*
	- Secure Boot checks that each boot step only runs trusted software -> chain of trust to TEE
	- With remote attestation, the *CPU measures the enclave and signs its hash*
	- Enclave invokes measurement, OS is suspended, CPU measures and 'returns' the signed hash
		- creates secure channel between client and enclave (e.g. [[Transport Layer Security|TLS]])

![[SGX Remote Attestation.svg|700]]
- Enclave called by "company app" that calls the enclave, which then executes and returns
- Enclave memory is encrypted (and integrity protected) at the processor boundary
- Enclaves have no direct access to disk or I/O or to trusted clock; limited support for counters
- Can do sealing: store encrypted confidential data on disk
	- This data is encrypted and MACed with enclave and processor specific keys
# Disk Encryption
- Attackers ...
	- cannot (easily) recover data from the disk
	- cannot boot from USB and change data (encryption + authentication)
	- can wipe the disk
	- may find encryption key and recover data

The simplest approach at achieving disk encryption is by only using a password. The OS asks a password from the user on boot, using it to unlock the encryption key, putting it in memory to decrypt the disk.

> [!danger] An attacker can brute-force the password

A better approach is to *store the encryption key in secure element* (TPM chip). This requires more hardware support, and migrating platform is harder. Disk encryption is common on most platforms (BitLocker on Windows, FileVault on MacOS and dm-crypt on Linux).

> [!example] dm-crypt
> - Works over different media (HDD, USB,...)
> - Encrypt disk as whole or single partitions (block-layer encryption)
> - cryptsetup to setup encryption
> - *File system is completely unware*

- Regardless of the unlocking technique, the encryption key must be kept in memory
	- Decrypt and encrypt files on disk as they are read and written
- Recall: Attacker with physical access
	- Extracting disk content is easy, but recovering data hard
	- Assumption: Attacker *cannot read secret from memory*

## Cold Boot Attack
Without refreshing DRAM, there is no immediate decay of the data, and data persists longer when cooled down.
![[95179.jpeg|600]]
1. Unlocking disk stores key on computer RAM when locked
2. Attacker powers down computer and removes RAM chips
3. Cool down RAM modules using compressed air or liquid nitrogen
4. Plugin RAM module into another platform and recover key

A less invasive manoeuvre is to boot an attack tool from external media, copying the memory content, and then using another platform for key recovery.

**Countermeasures**:
- **Erase key** from memory on every (controlled) suspend
	- user needs to type in password often
	- does not help sudden power loss
- **Prevent booting** from external media
	- does not prevent DRAM component transfers
- **Physical protection**
	- components that respond to enclosure opening or low temperatures
	- expensive for commodity systems
- **Avoid placing the key in memory**
	- Performs encryption in disk controller
	- Requires architectural changes
# Case Study
- We have explained disk encryption on x86 laptops
- What about smartphones?  
	- Similar storage encryption typically available (Android, iOS, ...) 
	- But certain details are different
- We examine smartphone protection through case study 
	- San Bernardino terrorist suspect from 2015 (FBI, Apple)
- Key points  
	- How was storage protection implemented? 
	- How was it (probably) broken?
## Storage protection on smartphones
- Users can enable storage encryption 
	- Typically optional feature
- Simple approach: encrypt with user-provided PIN 
	- Not secure against brute-forcing (obviously)
- On laptops TPM can be used for better protection 
	- But smartphones don’t have TPMs
- Solution: leverage hardware-support in the platform 
	- Processor-specific keys (ARM TrustZone)
- Processor chip (or SoC) has device-specific encryption keys  
	- Extracting such keys not easy (risk of breaking chip and loosing keys)
- Better protection approach  
	- At boot the OS asks PIN code from user  
	- PIN is given to the CPU  
	- CPU derives storage encryption key from PIN and processor key
- Prevents brute-forcing of extracted storage  
	- storage must be decrypted on the same device where the CPU is

Smartphone PIN codes are usually 4 digits long, but hard to brute-force due to *throttling* when attempts fail.

![[PIN code attempts.svg|500]]
## NAND Mirroring
- Create copy of NAND chip and copy data over
- After each boot and PIN attempt, power down device and restore modified parts from the backup NAND chip
- This bypasses the exponential time backoff implemented, allowing much faster brute-forcing

      

---
References: 
- https://tldp.org/LDP/tlk/mm/memory.html