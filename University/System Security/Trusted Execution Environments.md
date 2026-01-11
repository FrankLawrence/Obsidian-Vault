---
Tags: #lecture
Created: 2026-01-09 23:33:55
aliases: TEE
---
(Links:: [[System Security]])

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


---
References: