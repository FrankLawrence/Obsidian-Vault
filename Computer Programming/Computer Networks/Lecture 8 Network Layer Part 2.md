---
Tags: lecture
Created: 2023-05-17 01:24:34
---
(Links:: [[Computer Networks]])
# The Internet Protocol (IP)
## IPv4 Frame header
- Version: IPv4
- Time to live: How long the packets can travel before they should get dropped (counted by hops)
- Total length (16 bits): packet size can reach 64KiB
- [[Lecture 7 Network Layer Part 1#Packet fragmentation|Identification and Fragment offset]]: Which fragment of the packet and the offset in the data
- Checksum field computed by adding all 16-bit half-words
- Source and Destination address (32-bit each): IPv4 address for Network Interface Card
- You cannot just use a MAC address since they are geographically independent and topologically independent of the network
## Network Address Translation (NAT)
- Devices have their own internal IP address
- Devices on Home/Company LAN are connected to ISP via a **Network Address Translation Box** -> House/company has one external IP address
	- NAT receives the source address and port and the destination address and port when a devices sends a packet
	- NAT box address is fixed and switched for source address
	- NAT replaces the source port with a new port (randomly generated) mapped to the source address and port
	- NAT saves the mapped ports in a *table index*
## IPv6
- Time to live renamed to Hop limit
- Next header: specifies transport layer protocol or extension header
- No fragmentation support or error detection
## Addressing the Problem of Too Many Addresses
### Internet Protocol Prefixes and Subnets
- Routing algorithms can calculate routes to prefixes, instead of to every individual address
- Prefixes handed out by single organisation ICANN
- Organisations can further subdivide their prefix to create **subnets**

> [!example] IP Prefix
> Example address: 37.60.194.64
> $$\overbrace{00100101.00111100}^{\text{Network}} . \overbrace{11000010.01000000}^{\text{Host}}$$
> 16 bits used by network -> Prefix: 37.60.0.0/**16**
> Subnet mask: 11111111.11111111.00000000.00000000
### Classless InterDomain Routing (CIDR)
- Group different prefixes into supernets 

> [!example]
> ```mermaid
> graph TD;
> B --- C
> B --- D
> B --- E
> A --- B
> ```
> - C: 37.62.0.0/15 (**00100101.001111**|*1*0.00000000.00000000)
> - D: 37.60.0.0/16 (**00100101.001111**|*00*.00000000.00000000)
> - E: 37.61.0.0/16 (**00100101.001111**|*01*.00000000.00000000)
> - B is connected to three routers where all first 14 bits are equal
> 	- B sends a routing a table of size 1 instead of all 3
## Internet Control Message Protocol (ICMP)
- If something goes wrong, **routers** send these messages to **senders** 
- used by `traceroute` and `ping`
## Dynamic Host Configuration (DHCP)
- IP addresses are assigned dynamically by DHCP
- Can also configure other settings such as:
	- Network mask
	- Addresses of default gateway
	- DNS
	- Time servers
- DHCP server receives a request and offers an available address
	- Sends the IP address to the MAC address of the machine
## Address Resolution Protocol (ARP)
- Find the MAC address associated with an Network layer address (IPv4) address
- ARP packet is broadcast to all devices requesting MAC address from device with the IP address
- Request broadcast over Ethernet
- Station replies with ARP packet (IP and MAC address)
## Network-Layer Resource Allocation
- Connectionless Service: **Datagrams**
	- Routers use routing algorithm to decide where to send each packet **individually**
	- No setup required
	- Router failures have low impact
- Connection-Oriented Service: **Virtual Circuits**
	- Decide **fixed route** during connection setup
	- All packets part of the connection follow the route
	- Easy congestion control
	- Easy Quality of Service guarantees
# Congestion Control
- Make sure the sender does not send information faster than a receiver can accept
- Combined responsibility of the **network** and **transport** layers
- ![[Congestion control Goodput.png|500]]
- simplest approach: **resource over-provisioning** (installing more bandwidth)
## Traffic-aware routing
- Links between routers display available bandwidth on the network
- Using dynamic cost calculation can prevent congestion (pick a route with more bandwidth)
## Admission control
- check if network has sufficient capacity before loading new traffic
- keep on checking for new paths if links are overloaded
## Traffic throttling
- Send messages in the opposite direction to indicate network congestion
- End-to-End (TCP via ECN): Send back "choke" to the *source* to slow down transmission
- Link-by-Link: Send back "choke" to *every router*
	- works quicker but routers must buffer data in memory
# Traffic Shaping
Regulating Network Resource Usage
- devices don't send data at a constant rate
- limit data rate used by senders but allow bursty traffic
- Regulates **rate** and **burstiness** of data entering the network
## Token bucket
- $R$: Incoming token rate
- $M$: Maximum sending rate
- $B$: Storage size 
- Sending $n$ bytes requires $n$ tokens
- Maximum burst duration: $\frac{B}{M-R}$ seconds
## Load Shedding
- random early detection: Drop packets randomly if buffer space is almost full
- Works if transmission errors are unlikely cause of packet loss (wired links)
- Wireless channels need to solve transmission errors on the data link layer to 
## Quality of Service
- Bandwidth: Maximum data rate measured in bits per second
- Delay: Time it takes to get from source to destination
- Jitter: Variation in packet delay. 0 jitter means delay is constant
- Packet loss: Probability of packets being dropped
- different applications have different requirements

---
References: