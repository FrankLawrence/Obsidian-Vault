---
Tags: #lecture 
Created: 2026-01-03 21:14:45
---
(Links:: [[Network Security]])
> [!question] What are distributed denial-of-service attacks?
> - DoS attacks try to *make a service or network resource unavailable* to its intended/legitimate users
> - Typically achieved by *exhausting available resources* by sending an excessive amount of traffic/packets/requests
> - DDoS attacks can simultaneously use many different sources, often by creating and using [[#(IoT) Botnets|botnets]]
> - DDoS attacks are often used to extort companies: "Pay XX bitcoin to stop the attack"

|                                | Network links                                         | Network devices / <br> networking stack                                                                         | Applications / <br> end system                                 |
| ------------------------------ | ----------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| Description                    | [[#Volumetric Attacks]]                               | [[#Protocol Attacks]]                                                                                           | [[#Application-layer attacks]]                                 |
| Unit of measurement            | Bits per second (bps)                                 | Packets per second (pps)                                                                                        | Requests per second (rps)                                      |
| Used mechanism / <br> examples | [[#Reflection and Amplification]], [[#Shrew Attack]]  | [[#Reflection and Amplification\|Reflection]], [[#Session State exhaustion]], [[#SYN/ACK Flood]], Fragmentation | [[#Computational Complexity]], Hash collisions, [[#Slowloris]] |
| Defenses                       | Filtering and traffic scrubbing, Black-hole filtering | Cookies, rate-limiting                                                                                          | randomized/keyed hash functions                                |

# General DoS attack techniques
- Attacker *controls significantly more* resources than victim
- Attacker needs to *expend significantly less* resources than victim
- Attacker can *hide its identity* or continually change it (like with reflection attacks)
- Victim needs to *expend a significant amount of resources* before being able to assess the legitimacy of requests
- Attack can *instruct/trick other entities to send traffic* on its behalf (amplification attack)
## Main Attack Types
### Volumetric Attacks
- Consume the bandwidth either *within the target network/service, or between the target network/service* and the rest of the internet
- These attacks are simply about causing congestion
- Typical vectors of attack:
	- [[Lecture 8 Network Layer Part 2#Internet Control Message Protocol (ICMP)|ICMP]] packet floods (including all ICMP message types)
	- UDP packet floods
	- Malformed ICMP, UDP, IP packet floods
	- [[#Reflection and Amplification]]
	- Examples: [[DNS Amplification Attack|DNS]], NTP, memcached reflection
- Link flooding causes congestion & high loss rates for incoming traffic
- Note: very low throughput for >5% loss rates
- **Result**: Few legitimate clients served during DDoS attack

![[Congestion Control Goodput.svg|500]]

- Recent measurements demonstrate loss impact on currently used Protocols:
	- Deadline-aware Multipath Transport Protocol (DMTP) automatically selects best path and error correction to satisfy the arrival deadline for each packet, while attempting to reduce cost
	- In contrast, Multipath TCP and Multipath [[QUIC]] **deteriorate quickly as loss rates increase**

### Protocol Attacks
- Designed to exhaust resources available on the target or on a network device on the path to the target
  e.g. routers, load balancers, or security devices
- Once resources are consumed (e.g. TCP state table), no new connections can be opened
- *Protocol DDoS attacks need not consume all of the target's available bandwidth to make it inaccessible*
- They can take down even high-capacity devices capable of maintaining state for millions of connections
- Typical attack vectors: 
	- SYN/ACK floods
	- RST attacks
	- TCP connection floods
	- Fragmentation attacks
	- DNS, NTP reflection
### Application-layer Attacks

- Target various resources of an application or service at [[The Application Layer|Layer 7]]
- Most *sophisticated and stealthy attacks (low-and-slow attacks) as they can be very effective with as few as one attacking machine* generating traffic at a low rate
- Attacks very difficult to proactively detect with traditional flow-based monitoring solutions
- Typical attack vectors:
	- Layer 7 protocols, HTTP, [[SMTP]], [[DNS]], SNMP, [[FTP]], SIP, etc.
	- Application request floods
	- Database connection pool exhaustion
	- [[#Reflection and Amplification]]
	- Slowloris, slow post/read
	- SIP invite flood
## (IoT) Botnets

> [!question] What is a Botnet?
> - A large set of compromised machines connected to the Internet
> - Execute malicious code and can be controlled via command and control (C&C) systems
> - Often geographically dispersed

- Internet of Things devices are perfect for constructing botnets...
	- Many devices with uniform configuration
	- Often very poorly secured, e.g. hardcoded credentials
		- Enables automatic scanning
	- Often no security updates in years
		- In particular when manufacturer goes out of business
	- Often connected to the Internet without bandwidth limitations
	- Example: [[Mirai botnet]]
		- Mostly consisted of vulnerable webcams

- **Possible mitigations**
	- *Patch*
		- Manufacturers should provide automatic security updates -> mallory could bypass mechanism and compromise device which is updated
		- Provide patches for the full lifetime of devices -> may go out of business
	- *Credentials*
		- No hardcoded credentials
		- Force users to change default passwords -> users may forget password
	- *Monitoring*
		- ISPs should actively monitor their network for suspicious traffic (costly)
		  -> balance between false positives and true positives

## Reflection and Amplification
- Address spoofing
	- Source address in IP header can be set by sender
	- In a connectionless protocol (UDP), server cannot confirm actual sender
- **Defenses**
	- Ingress filtering by ISPs: ensure that hosts only use IP source addresses from their network's address prefix
		- Needs to be globally deployed
		- Poor incentives for ISPs to deploy it (only customers of other ISPs profit)
		- Vulnerable to in-network packet injection (e.g. by malicious routers)
	- Use connection-based protocols (e.g. TCP, QUIC)
		- Additional latency, memory, and (esp. in case of [[Transport Layer Security|TLS]]) computation
		- Potentially additional DoS attack vector (state exhaustion)
	- Cryptographic source authentication
		- Additional DoS attack vector if built on (expensive) asymmetric cryptography
		- Requires symmetric key distribution of [[PKI]]s

Reflection and amplification attacks require the ability to spoof the source address, of publicly accessible servers, where ideally the amplification factor is large. Typical reflectors (and their amplification factor) include
- DNS (up to ~180)
- NTP (up to ~500)
- Memcached (up to ~50 000); UDP disabled by default in version 1.5.6

> [!info]- How reflection and amplification work
> 1. Choose open service (e.g. open DNS resolver) as reflector
> 2. Craft request that triggers (much) larger response
> 3. Send packet where source address is set to victim's address
> 4. Reflector sends reply to victim

### Mitigations
- Prevent address spoofing
- Perform [[Security Engineering Model Driven Security#Role-Based Access Control (RBAC)|access control]] 
	- e.g. DNS servers deployed within an organization or ISP should only serve clients from same organization
- Implement response rate limiting (RRl) 
	- Limit the number of responses to a client IP
- Ensure small amplification factors (ideally < 1)
	- Example: WireGuard ensures that the responder's first message is *smaller* than the initiator's
# Specific Attack Examples
## Volumetric Attacks
### Shrew Attack
Conventional bandwidth-based DoS requires sending *high-rate* attack traffic. Shrew DoS exploits [[University/Advanced Network Programming/Congestion Control|TCP congestion control]] feature to send periodic short burst to the target link/router. This denies legitimate TCP flows, as it believes there is a *long-term congestion*. Since TCP retransmission uses exponential backoff timeout, the attacker forces TCP into *retransmission timeout state*.

![[Congestion Control.svg|600]]

Alternatively, the attacker can use *temporal lensing*: send traffic to reflectors, which have varying delay when arriving at the victim. This concentrates the flow in time, allowing an attack using a low-bandwidth channel.

![[Temporal lensing.svg|400]]
### Coremelt
- Adversary controls many bots distributed across the internet which send packets *between each other*, exhausting the bandwidth on the victim link. 
- More suited for attacking core infrastructure of certain target areas
- There is no *target* in the usual sense to identify unwanted traffic

### Crossfire
- Observation; due to route optimization, few links are actually used to connect a target region to rest of internet (common BGP routes)
- Adversary controls distributed bot army, targeting servers inside *target region* to disconnect it from rest of internet
## Protocol Attacks
### DNS Flooding (NXDOMAIN Attack)
- Goal: overwhelm victim's authoritative name servers
- Idea: query non-existent subdomains of victim domain
- Resolver queries all authoritative name servers in turn
- Can use multiple DNS resolvers
- Result: name server can no longer reply to legitimate requests

> [!danger] Why is this a problem?
> Many internet services rely on DNS to map domain names to IP addresses (even the DNS infrastructure itself), which leads many different services to be unavailable.
### Session State exhaustion
In a two-way communication, a channel between peers is identified with a unique session number. The session number must be known at the server to match subsequent requests to the right session. By *exhausting* the **session state table** of the server, new connections are no longer accepted, and existing connections are dropped.
### SYN/ACK Flood
TCP uses a [[Lecture 9 Transport Layer Part 1#Three-way Handshake|three-way handshake]] to establish a connection:
1. Client sends a SYN packet with random sequence number $A$
2. Server replies with a SYN+ACK packet with acknowledge number $A+1$ and a random sequence number $B$
3. Server keeps state $\{A+1,B\}$
4. Server waits for an ACK with sequence number $A+1$ and acknowledge number $B+1$
5. Connection established

![[TCP handshake.svg|300]]

If we send *many SYN packets* with *spoofed source addresses*, eventually the server state table overflows, which prevents new connections.

We can mitigate this, by using SYN cookies, allowing for connection without state table:
1. Client sends a SYN packet with a random sequence number $A$
2. Server replies with a SYN+ACK packet with acknowledgement $A+1$ and sequence number $B=F(\text{time}, \text{IP address}, \text{port},...)$
3. Server checks if expected/valid acknowledge number is returned by applying $F(...)$
4. Connection established
- Encode state in a unique but determined way (by using cryptographic hash function with secret salt) that allows the server to validate the state in the reply -> no need to store session state at the server
## Application-Layer Attacks
### Computational Complexity
- Induce worst-case behavior in a vulnerable algorithm
- The larger the difference between the worst case and average case, the more vulnerable the algorithm

**Hash Collisions**:
- Collisions are common due to small table size; table resized when load factor > threshold
- If you know how the hash algorithm works, you can create entries, such that it always results in collisions -> linear time instead of constant
- *Mitigation*: hash randomization use a secret for each hash table

Another example includes *regular expression denial of service*, where a regex might take exponentially longer to compute for specific inputs.
### Slowloris

Slowloris allows a single machine to take down another machine's web server with minimal bandwidth, by continuously occupying connections, and creating *computationally expensive* operations for the server:
- sends partial requests
- periodically, will send subsequent HTTP headers, adding to - but never completing - the request
- fills connection pool

Mitigation:
- Increase connection pool size
- Limit simultaneous IP address connections
- Require minimum transfer speed per connection -> people with slow speed can't access
- Restrict client connection time
- *Use deep packet inspection and heuristic measurements on traffic patterns to discern the legitimate from illegitimate traffic* -> balance with false positive rate
# Countermeasures
Commonly Deployed DDoS Defense Mechanisms:
- **Ingress filtering**: Removes packets with illegitimate source IP addresses
- **Cloud- or ISP-based filtering**: Delegates defense to cloud or ISP to apply heuristics to filter out malicious packets
- **Computational puzzles**: Slows down attacks, achieves per-computation fairness (e.g. Anubis)

Most systems aim for 99.999% uptime (<5min down/year) by using:
- **Redundancy**
	- No single point of failure
	- N+2 systems running under normal operation (service still available if two systems fail)
	- More than two geographically diverse locations
	- More than two independent internet connections
- **Monitoring & Rapid detection**
	- Rapid detection and automatic failover
	- Evict failed nodes/systems immediately
- **Failure resilience**
	- System can tolerate various temporary component failures
	- Graceful degradation upon failures
- Perform *long-term monitoring* to assess periodicity and peak periods/loads (like holidays)
- Plan bandwidth and resources to cover the *majority of extreme peak* loads (*over provisioning*)
## In-network and cloud-based DDoS Mitigation Services
Cloud-based DDoS mitigation includes [[BGP, Anycast, CDN#CDN|CDN]], where host might be charged more for attack traffic. By changing [[BGP, Anycast, CDN#BGP Mechanics|BGP]] prefix announcement or DNS entry for web server, traffic can also be redirected to cloud provider using filtering.

ISP-based DDoS mitigation commonly refers to **scrubbing centers**, where *Deep packet inspection* (DPI) is employed and *connection patterns* identified to filter malicious traffic.

Downsides:
- Cloud-based security provider can be easily bypassed
	- Because most clouds use DNS to redirect traffic, attackers can easily bypass the proxies if the victim’s IP is exposed
- Heuristic filtering, adapts slowly to attack
    - Heuristics suffer from false positives and false negatives
    - Adversary can rapidly cycle through different attack strategies
- Filtering typically only observes traffic destined to victim, does not see replies
	- Adversary can send encrypted traffic claiming to have an open connection, filtering system cannot see returned error packets, or detect absence of return packets
- Privacy violation
    - E.g., Radware decrypts HTTPS and injects CAPTCHAs to client
    - An untrusted or compromised cloud could expose users’ sensitive data
- Very limited destination traffic control
- High setup cost to install system, e.g., in some cases leased line needed to carry filtered traffic
	- Once set up, high barrier to switch to different provider, followed by price increases 
- Requires continuous subscription for fetching updated attack signatures
## Remotely Triggered Black Hole Filtering (RTBH)
- Idea: simply block all requests from a specific ip prefix in border routers (using BGP updates) to protect ISP infrastructure
- Many false positives, and can be exploited to block organization using [[#Reflection and Amplification|address spoofing]]

# DDoS Defense with the SCION

[[SCION#EPIC Every Packet is Checked|EPIC]] is a feature of SCION, which allows for source address validation at line-rate speeds. With this, and its *hidden paths* feature, we can defend against DDoS attacks:
- SCION paths segments do not need to be globally announced, but can be selectively distributed as a "hidden path"
- Without a valid cryptographic authenticator (secret key) in the hop field, a packet cannot pass onto the protected link

Another DDoS defense makes use of *Hummingbird*, a scalable global Quality of Service (QoS), which allows *purchase of AS traversal reservations*. Stable paths ensure reservations are not affected by routing changes. No per-flow state on routers enables forwarding scalability.

We can now enable fundamental DDoS defenses, and distinguish between two cases:
- Case 1: communication **among trusted entities** can be guaranteed
	- Hidden path system enables access control of last link
	- Hummingbird guarantees bandwidth for entire communication path
- Case 2: public service: initially no distinction possible between bot and legitimate user
	- During DDoS attack, *after user authentication*, legitimate users can be migrated to hidden path or Hummingbird-protected connection
	- Keep performing user authentication, rescue legitimate users by migrating them as well
		- question: how do we protect the initial connection to perform user authentication?

> [!question] How do we protect the initial connection to perform user authentication?
> - Attempt 1: If users have a shared secret with the service, hidden path or Hummingbird connection can be bootstrapped through a DDoS-resilient service (e.g. cloud-replicated service to authenticate users and provide path information)
> 
> > [!bug] Solving DDoS by using DDoS-resilient servie ;-)
> > Slightly better, since authentication portal easier to scale than full web service
> 
> - Attempt 2: Better approach: use SCION packet authentication at routers and at destination to efficiently prioritize legitimate users over bots
> 	- Observation: If legitimate user population is differently distributed than the bot population, then we can use SCION/EPIC to prioritize legitimate users' traffic
> 	- SCION/EPIC can provide concrete approaches to implement diverse prioritizations
> 	- Sample prioritization based on AS-level authentication: per AS history fair sharing, which measures historical number of users/packets per AS
> 	- Powerful: *any host in an AS without attacker nodes will obtain at least their historical fair share*

> [!example]- Web Store
> - Set up separate server instances for IP (regular Internet) and SCION traffic
> 	- Isolates SCION server from IP Internet attacks
> - Set up hidden paths to SCION server, prioritizing traffic from hidden paths and dropping remaining traffic that server cannot handle
> - Premium users can continuously obtain hidden path information, so that they always have access to a valid hidden path
> - During DDoS attack, web store migrates legitimate users to use a hidden path 
> - System properties
> 	- Premium users will be served throughout the attack
> 	- Legitimate users will be able to use service without interruption after migration to hidden path
> - Note: Hummingbird needed to remove hidden path assumptions (congestion is on the last hop link and no legitimate users are malicious). Hummingbird planned to become operational in 2026