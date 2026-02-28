---
Tags: 
Created: 2025-09-30 10:15:59
---
(Links:: [[Network Security]])
**Vision**: high available, efficient communication on global networks with malicious adversaries through formal verification

> [!attention] Non-scalability of Trust
> The internet as evolved and expanded -> **There is no single entity that is trusted by everyone**
## Fault Isolation Domains

- Each local grouping (ISD) of ASes has their own local trust roots
	- Prevents ASes in other groups from hijacking their ASes
	- Organic forming structure
	- ISDs interact through well-defined borders
- Decentralized trust with no global single point of failure (except for Cloudflare ;) )

![[Fault Isolation Domains.svg|700]]

## SCION Path-based Network Architecture
- **Control Plane**: Construct and disseminates *path segments*
- SCION creates path segments at the higher level through the ISD cores
- **Data Plane**: Combine Path Segments to *Path*
	- Packets contain Path
	- Routers forward packets on Path
- **The endpoint chooses the path**, by controlling which paths it advertises

![[SCION Path-based Network Architecture.svg|700]]

SCION has many features such as:
- *Geofencing*: restrict paths to certain countries to keep traffic within jurisdiction
- *Resilience* via fast-path failover
- *Multipath*; allowing for higher bandwidth by utilizing all paths
	- Given a *high latency, high bandwidth* and a *low latency, low bandwidth* path, [[BGP, Anycast, CDN|BGP]] picks the cheapest path (low latency)
	- The preferred path is application dependent -> SCION offers both paths

### Multipath
- Inter-domain multipath is a necessity to achieve high availability
- Rapid failover without routing system convergence
- Enable higher network capacity
- Enables higher communication efficiency
- Helps defend against [[DDoS Attack]], as adversary needs to congest all links

# SCION Details: Deep Dive
## Intra-ISD Beaconing
- Core Ases K, L, M initiate **Path-segment Construction Beacons** (**PCB**s)
- PCBs traverse ISD as a flood to reach downstream ASes
- Each AS receives multiple PCBs representing path segments to a core AS
	- PCBs collect path information as they traverse network

![[PCB Contents.svg|900]]

- PCBs contain path segments that can be used as communication paths to the core AS that initiated it
	- **Down-path segment**: from core AS to AS
	- **Up-path segment**: from AS to core AS

## Segment Registration
### Up-path
- **ASs select** the path segments to announce as *up-path segments* for *local hosts*
- Each AS runs their own **local path servers**
	- Used to register *up-path segments*
	- Sent to core path server, which can be queried globally, from other ISDs
- **Path servers provide a lookup service for segments to an AS**
### Down-path
- **ASes select** the path segments to announce as *down-path segments* for *others* to use to communicate with it
- Down-path segments are uploaded to **core path server** in **core AS**
## Inter-ISD Path Exploration
- **Core beaconing**: advertise routes to other ISDs
## Communication Overview
> [!example] Communication within ISD
> - Client obtains path segments
> 	- Up-path segments to local ISD core ASes
> 	- Down-path segments to destination
> 	- Core-path segments as needed to connect up-path and down-path segments
> - *Client combines* path segments to obtain end-to-end paths

> [!example] Communication to remote ISD
> - Host (F) contacts local path server requesting `<ISD,AS>`
> - If path segments are not cached, local path server will contact core path server (A or B)
> - If core path server does not have path segments cached, it will contact remote core path server (other ISDs)
> - Finally, host receives <b style="color:#B46504">up-</b>, <b style="color:#0E8088">core-</b>, and <b style="color:#56517E">down-</b>segments
> ![[SCION Path-based Network Architecture.svg|500]]

## Path Combination
![[Path Combination.svg|900]]
## Data Plane
- Control Plane explores and disseminates path segments
- Forwarding information from path segment is encoded in the packet header
- (Stateless) routers only verify the authenticity of the information
  -> two [[AES]] cryptographic operations replace (expensive) longest-prefix match in [[BGP, Anycast, CDN|BGP]]

> [!info] Observation
> - SCION is scalable compared to BGP
> 	- AS-level instead of IP prefix based routing
> 	- Leaf ASes only receive but do not forward any beacons (only core ASes initiate beacons)
> 	- Beaconing does not require iterative convergence (which can take very long for BGP)
> - Consequences:
> 	- Rapid *path exploration* between all pairs of core ASes
> 	- Near "**instantaneous**" path exploration within ISD
> 	- Simulations suggest that gloval connectivity after "Internet cold boot" can be achieved within seconds

--- 
> [!summary]
> - Network paths are discovered through **beaconing**
> - Paths are **registered** at **path servers**
> - **Senders query path servers** for path segments, which can be used to add forwarding information to packets
> - SCION **Border routers peer** with other networks and **forward** based on **forwarding information in packet header**

# Benefits
- Security: Authenticated control plane and resilience against path hijacks
- Stability: Native multipath capability at the network level with rapid path failover ensures high stability despite link failures at the physical layer
- Control: Path-awareness for end hosts enables application-specific path control and optimization
- Protection: Hidden paths and sender-based path selection increase protection agains [[DDoS Attack]]
- Performance: SCION applications can select the best paths based on latency, bandwidth, loss, or jitter

# SCION in the field
## SCION for end-users
![[SCION Field Deployment.svg|1400]]

- Studies show that RTT median is 6.9% lower compared to standard IP (using ICMP and SCMP pings)
# SCION Defense mechanisms
- **High-speed packet processing**
	- High speed links (400Gb/s) require 64-byte packet processing every $1.3ns$
	- High-speed asymmetric signature implementation take too long: $\sim 100\mu s$ per signature
	- AES-NI instruction only requires 30 cycles ($\sim 10ns$), but hardware implementations can be performed sub $1 ns$
		- Reference: Memory lookup from DRAM requires $\sim 200$ cycles ($\sim 70ns$)
	- *Symmetric crypto enables high-speed processing through parallel processing and pipelining*
- **DRKey & Control-Plane PKI**
	- SCION offers a global framework for authentication and key establishment for secure network operations
	- Control-plane [[PKI]]
		- Sovereign operation thanks to ISD concept
		- Every AS has a public-key certificate, enabling AS authentication
	- Dynamically Recreatable Keys (DRKey)
		- High-speed local key derivation (within $\sim 20ns$)
	- *PISKES: Pragmatic Internet-Scale Key-Establishment System* 
## Dynamically Recreatable Key
- **Idea**: Use a per-AS secret value to derive keys with an efficient Pseudo-Random Function (PRF)

- AS $X$ *creates* a key for AS $Y$ using secret value $SV_{X}$: $$K_{X\to Y} = PRF_{SV_{X}}(\text{"Y"})$$
	- Intel AES-NI instructions compute $PRF$ within 30 cycles
	  Key computation is $\sim 7$ times faster than DRAM key lookup!
	- Any entity in AS $X$ knowing secret value $SV_{X}$ can derive $K_{X\to*}$ very quickly
- Notation: $K_{X\to Y}$ 
	- Arrow indicates direction of key derivation, *not* the direction of communication
- Asymmetric key derivation
	- Entities in AS $X$ can derive key based on a single local key (nanosecond scale, stateless) -> great for verifying message origin at high traffic areas
	- Entities in AS $Y$ need to contact local key server to fetch key (microsecond scale, stateful)
### Key Server Infrastructure
- Key servers deployed in each AS build backbone
	- Responsible for key exchange, local key establishment and key management
- After AS-level keys are established, symmetric keys for end hosts can be provided using key derivation
- Keys can be used to provide source authenticity of packet without costly key exchange between communicating parties
- Each host is required to contact their *local* key server
### Key Hierarchy and Exchange
- AS $A$ creates key hierarchy from secret value $A$ ($SV_{A}$) using a pseudo-random function
	- 0th level: per-AS local secret key $SV_{A}$
		- *renewed frequently*
		- distributed to trusted infrastructure nodes inside same AS
	- 1st level: $K_{A\to B} = RPF_{SV_A}(\text{"B"})$
		- AS uses 0th level key to derive symmetric key for other ASes
	- 2nd level: $K_{A\to B:Hb} = RPF_{K_{A\to B}}(\text{"Hb"})$
- Key servers exchange keys through secure channel set up with AS certificates
![[DRKey Key Hierarchy.svg|800]]

- Second-level key exchange: apply $PRF$ once more on AS-to-AS key to derive AS-to-Host symmetric key

> [!example] 
> AS $X$ creates a key for Host $H$ in AS $Y$
> - $K_{X\to Y} = PRF_{SV_{X}}(\text{"Y"})$
> - $K_{X\to Y:H} = PRF_{K_{X\to Y}}(\text{"H"})$

- Any entity in AS $X$ can derive $K_{X\to *:*}$ because the know secret value $SV_{X}$ 
- Key server in AS $Y$ can derive $K_{X\to Y:*}$ from stored $K_{X\to Y}$
- Host-to-Host keys $K_{X:*\to Y:*}$ can be similarly derived
## Use Case: SCMP Authentication
- AS $B$ must decline packets intended for AS $C$
- Border router in AS $B$ can derive key $K^{scmp}_{B\to A:\text{Source}}$ from $SV_{B}$ (done *quickly*!)
- Host "Source" in AS $A$ can fetch key from local key server $KS_{A}$ to authenticate SCMP message

![[SCMP Authentication.svg|800]]
## Use Case: LightningFilter
- Goal: Super fast firewall for incoming traffic
- Sender locally fetches remote LightningFilter's DRKey key from local key server
- Remote LightningFilter can derive key within a few nanoseconds and can authenticate packet
- Advantage: cryptographic packet verification possible at much lower overhead than heuristic rule-based firewalls

- **Bonus**: History-based filtering
	- Filtering service that is deployed upstream of protected end server
	- Gather traffic volume data from each AS to determine resource allocation during DoS when an AS is compromised and goes rogue -> limited to specific number of requets/sec
	- Collateral damage only for hosts within attacker-infested AS
## EPIC: Every Packet is Checked
- Properties
	- Level 1: improved path authorization based on additional secret key
	- Level 2: line-rate packet source authentication by routers and destination
	- Level 3: path validation by destination
- Assumption: global time synchronization (+/- 100ms)
- Attacks prevented:
	- Malicious router replays packets or increases packet size
	- Hop field MAC is brute forced and destination attacked until expiration time
### EPIC Level 1: Used in Hidden Paths
Currently, a SCION hop field MAC $MAC_{K_{i}}$ consists of the following:
- $TS$: time beacon creation
- $ExpT$: Expiration time
- $IgIF$/$EgIF$: Ingress/Egress Interface
- $T$: packet creation time
- $\sigma_{i-1}$: previous hop field MAC

Additionally 
- $K_{H} = MAC_{K_{i}}(TS \parallel IgIF \parallel EgIF \parallel ExpT \parallel S_{i-1})$  
- $S_{i} = H(K_{H})$ (Host of the current key)
- Hop field MAC: $MAC_{K_{H}} (T \parallel H(P) \parallel len(P))$

Hop key $K_{H}$ is distributed via [[#Intra-ISD Beaconing|beacon]] (PCB) and $S_{i}$ is included in packet. The result is *every packet has unique hop field MAC*, foiling brute force attacks on hop field MAC.

==TODO: do not fully understand==
### EPIC Level 2
- Goal: line-speed source authentication for every packet on every router
- Approach: include DRKey $K_{X\to Y:H}$ in hop field MAC
- Hop field MAC: $MAC_{\color{orange} K_{\Large X\to E:e}} (T \parallel H(P) \parallel len(P)\parallel K_{H})$
  For host $e$ in AS $E$, traversing AS $X$
- Router in AS $X$ can efficiently derive $K_{ X\to E:e}$ (2 AES operations)
- Host $e$ needs to fetch one key per AS traversed from local key server
- Result: efficient per-packet in-network source authentication (5 AES ops)

> [!summary] SCION enables efficient source address verification
> - ISD, AS number and host address can now be checked through DRKey-based MAC verification within nanoseconds
> - In-network checking on routers within EPIC header
> - At destination checking through SCION Packet Authentication Option (SPAO) header, e.g. with LightningFilter
> - Fundamental defense against source address spoofing

---
References: