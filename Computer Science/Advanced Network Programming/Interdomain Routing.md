---
Tags: 
Created: 2024-10-03 15:33:42
---
(Links:: [[Advanced Network Programming]])
# Basics
- Transport: End-to-End communication (TCP, UDP,...)
- Network: Best-effort global packet delivery (IP, ICMP, ...)
- Application: Border Gateway Protocol -> For exchanging routing and reachability information among autonomous systems
## Packet Switching
![[App. data packet.png|200]]
- Source splits application data into *packets*
- Each packet contains a *header*; header specifies destination
- **Encapsulation**: Each layer has its own header
![[Packet switching.png|400]]
- Packets travel *separately*
- Routers **forward** packets using destination address
- Routers may **buffer** packets
- Receiver *re-assembles* packets into application data
- **Statistical multiplexing**
	- Traffic is bursty
	- Maximize utilization
## IP: Best effort delivery
- Packets may be *lost*, *corrupted*, or *delivered out-of-order*
- Simple
	- Per-packet decisions; little complexity
	- Easy failure recovery
	- Supports heterogenous links/networks
## The network of networks
Autonomous system (AS) (often network providers like KPN) are a bunch of routers, where *single* entity or organization decides how the packets within are transmitted. Each AS is assigned an **AS number (ASN)**
# BGP Role & Router Internals
Routers make **per-hop** decisions (no control after forwarding) on how to route packets *toward* their destinations. Routers must thus learn the path to destinations to be able to decide where to forward next.
## Router Internals
![[Router Internals.png|300]]
- Input ports
	- terminates incoming physical link to a router
		- receives datagrams
		- decapsulation
		- lookup (a function which maps an input address to output address; is sent along Output ports)
	- "adapter"
- Output ports
	- Opposite of input ports
		- sends datagrams on the "wire"
		- queuing
		- encapsulation
- Switching fabric
	- "Heart" of the router
	- Connects input ports to output ports
- Routing processor
	- Executes routing protocols (e.g. BGP)
- **Control plane**: Routing
	- Computing paths to destinations
	- Exchanging path information with other routers
	- Creating forwarding tables
- **Data plane**: Forwarding
	- Directing packet to an outgoing link
	- Using forwarding tables
# Forwarding
- Each router has a **forwarding table**  
	- Maps destination address to outgoing interface
- After receiving a datagram ...
	- retrieve destination address from the header 
	- lookup address in the table
		- determine the outgoing interface  
	- forward the packet out via that interface

Routers use Longest Prefix Match to decide which interface to send an incoming packet on:
![[Longest Prefix Match.png|500]]

Example: `130.37.0.2` is contained in two prefixes. Choose the **most specific** route.
# Routing
- Allows a *subnet* to advertise its existence
- **Policy-based** (money 💰), path vector, complex, ...
- BGP's functions
	- Obtain subnet **reachability information** from neighbouring AS
	- Propagate this knowledge to all routers in the AS
	- Pick *"good"* routes based on *reachability information* and *routing policy*

> [!question] How do packets reach different networks?
> Networks are represented by *prefixes* or *subnets*

![[BGP reachability information.png|500]]
- Learning reachability information:
	- Facilitates routers or **BPG peers** to exchange routing information over TCP connections or **BGP sessions** (does not mean physical link between two routers)
	- Consider routers $R_{1}$ and $R_{2}$
	- $A$ **advertises** *prefix* to $B$
		- $A$ says to $B$, "I promise to deliver all datagrams destined to the prefix `130.37.0.0/16` towards that prefix"
	- Traffic flows in the *opposite* direction of the BGP connection
	- sessions between $A$ and $B$ are *eBGP* sessions
	- sessions between routers within $B$ are *iBGP* sessions
- BGP configuration
	- **Routing policies** configure BGP's behavior such as
		- Choosing paths from multiple alternatives
		- Controlling advertisements to other ASes

![[BGP Announcements.png|500]]
- BGP Announcements
	- $C$ has two choices for `130.37.0.0/16`
		- Send to $B$ (which will then send to $A$)
		- Send to $A$ -> Will choose this because "Choose best path, with lowest cost"
	- $D$ can decide to not broadcast to $E$ if they don't want to have to route their traffic
# Policies
> It's all about business relationships

![[Policy-base routing, customer provider.png|200]]
- Customers $x$ and $y$ **pay** their respective provider for internet access (and to be able to communicate with each other) -> $A$ provides **transit** for $x$
- **Settlement-free peering**: $A$ & $B$ are *peers* (equals) -> they decide to not have to pay each other since "roughly" the same amount of data goes to the other

![[86115.png|200]]
- $x$ advertises its prefix $P$
- $A$ (re)advertises prefix $P$ to $C$ and $B$
	- $A$ *is paid by* $x$
	- Traffic destined to $P$ (belonging to $x$) from anywhere *must* reach $x$
- $B$ advertises $P$ to $y$
	- $y$ pays $B$
- $C$ does **not** advertise $P$ to $B$ (No revenue for $C$)
- $x$ advertises $P$ to $A$ and $C$
	- $x$ pays $A$ and $C$
- $A$ and $C$ both *re-advertise* $P$ to $B$
	- What does $B$ choose?

> [!question] If $x$ has $A$ and $C$ as providers, and both advertise $P$ to $B$, which will $B$ choose?
> It depends ...

---
References: