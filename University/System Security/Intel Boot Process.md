1. Power-On / Reset
   CPU starts from Reset Vector (0xFFFFFFF0), mapped from the top of [[#SPI Flash Layout (simplified)|SPI Flash]]
2. CPU reads Boot Guard Policy
   From Field Programmable Fuses (FPFs) and the Boot Policy Manifest (BPM) in SPI flash
3. Boot Guard Verifies IBB
	1. Calculate a cryptographic hash over the IBB region (defined in BPM)
	2. Checks its digital signature using the OEM public key hash fused in hardware
4. If verification passes: CPU begins executing the firmware inside the IBB - the UEFI SEC phase (Security phase)
5. If verification fails, boot process halts or enters recovery mode

# SPI Flash Layout (simplified)

```
┌───────────────────────────────┐  0x00000000
│ Flash Descriptor              │
├───────────────────────────────┤
│ Intel Management Engine (ME)  │
├───────────────────────────────┤
│ GbE Region / OEM Data         │
├───────────────────────────────┤
│ BIOS / UEFI Firmware Region   │  <- Reset Vector (0xFFFFFFF0)
│  ├── Initial Boot Block (IBB) │  <- Verified by Boot Guard
│  ├── PEI / DXE code           │
│  └── BOS & Boot Manager       │
└───────────────────────────────┘  0xFFFFFFFF
```

# UEFI in Boot
1. Power on
2. *Initial ME firmware executes first*
	1. Loads from ME region in SPI flash
	2. Initializes chipset (PCH), memory, security features
	3. Loads Boot Guard policy and keys from fused configuration
	4. Looks regions of SPI flash (ME) which sets security policy
3. CPU held in reset
	1. *ME configures boot guard enforcement before releasing CPU*
	2. Boot Guard policy: Verification Mode (enforce) or Measurement Mode
4. CPU released from reset -> fetches reset vector from SPI flash (first instruction)
	1. CPU attempts to execute the **first instruction of UEFI** (SEC phase)
	2. **Boot Guard intercepts and verifies the Boot Policy Manifest (BPM) and Initial Boot Block signature (IBB)**
	3. Signature is checked against key fused into CPU
	4. If verification passes -> continue
	5. If verification fails -> halt (Verification Mode) or measure (Measurement Mode)
5. **UEFI firmware executes** (SEC -> PEI -> DXE)
	1. Hardware initialization continues
	2. TPM measurements (PCRs) extended with hashes of firmware components
	3. Secure Bot keys loaded
6. Boot Manager loads OS Loader
	1. OS loader signature checked (Secure Boot)
	2. TPM PCRs reflect full measured boot path
7. Operating System Booted
	1. ME continues running in background
	2. TPM PCRs available for remote attestation

# Initial Boot Block
| Component                       | Purpose                                                                    | Notes                                         |
| ------------------------------- | -------------------------------------------------------------------------- | --------------------------------------------- |
| Reset Vector Stub               | The first instruction CPU fetches                                          | Often a far jump to real firmware entry point |
| *SEC Phase (Security Phase)*    | *Minimal assembly/C code that sets up CPU state and verifies next phase.*  | Must be self-contained, small, and verified.  |
| Early chipset init routines     | Very basic initialization (e.g. SPI controller, memory controller handoff) | Just enough to allow PEI stage to load        |
| Temporary stack setup           | Enables early C code execution                                             | Often uses CPU cache as RAM                   |
| *PEI Core entry point*          | Transition point into Pre-EFI initialization                               | PEI code itself may or may not be part of IBB |
| Optional early Boot Guard hooks | Code that extends measurements into TPM or performs platform checks        | Optional but common on enterprise boards                                              |

# Boot Sequence