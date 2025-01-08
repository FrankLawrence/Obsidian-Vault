---
Tags: 
Created: 2024-10-23 02:28:14
---
(Links:: [[Advanced Network Programming]])
# BGP Mechanics
![[BGP Path Advertisement.png|400]]
- Assume some peerings between ASes
- $A$ advertises its prefix to its *neighbours*. **Traffic** flows in a direction *opposite* to that of the advertisements
- We can control traffic **ingest** using what and to whom we advertise
- If $C$ accepts the advertisement and its policy allows re-advertising to $B$ and $D$, then $C$ **prepends** itself to the AS path and re-advertises
- BGP is known as a **path vector** protocol: it chooses decisions based on the distance vector + path information
	- Path length is represented in terms of AS-level hops
	- Instead of just distance, entire path is provided
- If $C$ receives an advertisements with $C$ in the AS path, then discard it (disallow **cycles** in routing path)
![[Shortest AS path BGP path advertisement.png|400]]
- If $D$ receives two advertisements with the same length, which will it choose? -> based on $D$'s policy
	- ASes can assign certain scores (*local preference*) to different ASes, usually based on "measured" speeds
# Anycast
- **Idea**: Advertise same prefix from multiple locations
- How to server content from an anycast IP address?
  -> Announce the same prefix *from multiple locations* in BGP
- IP packets are sent to a specific destination IP address
- How to send a packet to an **anycast** destination IP address?
	- No difference to regular unicast IP address
- Anycast challenges?
	- Many factors influence BGP routing decision: AS path, local preference
	- Chosen anycast replica can change repeatedly, over time
- Connectionless services are best suited for IP anycast
# CDN
When an internet user requests content (say from Youtube), the server providing the resources could be far away, increasing **latency**. The throughput of TCP depends on latency. 
One server is also a **single point of failure**, meaning that a rapid increase in users could throttle/overwhelm the system. Bandwidth and storage cost might be prohibitive, based on the location.
With platforms having to host multiple servers around the world, there also needs to be an efficient was of deciding, for a given user, *which server is closest*.

> [!info] Approaches to content delivery
> - Centralized hosting
> - Peer-to-peer networks
> 	- In theory, **infinite** scalability
> 	- In practice, download capacity is *limited* by upload capacity of peers
> - Content delivery networks
> 	- **Offload** content delivery to a large number of content servers
> 	- Place servers "near" users, in **diverse** locations and networks

- Benefits: 
	- Content Consumer: Fast download
	- Content provider: Reduce infrastructure cost; quick and easy deployment of network services
- **Redirection**
	- DNS-based
	- Anycast-based
	- Hybrid...

> [!example] DNS-based redirection
> - `GET /`
> 	- HTML contains an image URl
> 	- www.abc.com/world.jpg
> 1. User tries to resolve the domain
> ![[CDN DNS resolution.png|500]]
> 2. When returning response to domain lookup, give the ip address of the CDN server closest to the user

CDNs take care of:
- Service type - ability to satisfy the request (e.g., QuickTime)
- Server health - up and running?
- Server load - CPU, disk, and network utilization
- Network condition - Minimal packet loss, sufficient bandwidth
- Client location - proximity to client, e.g., in terms of RTT

> [!summary]
> - BGP is a path-vector protocol  
> 	- Policies: What is advertised and to whom?
> 	- Policies are typically based on business relationships, not performance!
> 
> - Anycast  
> 	- Announce same prefix from multiple locations
> 	- Leave BGP to determine best path for (customers in) different networks
> 
> - CDNs
> 	- Deliver content “efficiently” to end users  
> 	- Massive fleet of servers around the globed 
> 	- Serve data to users from “closest” servers
> 		- Using Anycast, DNS, ...

---
References: