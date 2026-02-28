---
Tags: exercise
Created: 2026-01-21 17:32:16
---
(Links:: [[Network Security]])
# Exercise sheet 03: SCION

### Question 1
One of the principal aims of SCION is to provide users with transparency
and better control over the forwarding paths of network packets. Answer
concisely the following questions.

> [!question]- In the context of routing architectures, what is the control plane? What does it do?
> The control plane is the part of the architecture that deals with network topology, making routing decisions. It is responsible for the discovery of network paths, route establishment, switching to failover paths in case of failures, and policing.

> [!question]- What is the data (or “forwarding”) plane? What does it do?
> Data plane is responsible for forwarding data packets that end hosts have injected into the network. Packets are forwarded according to the routes established in the control plane.

### Question 2
**SCION vs IP**

> [!question]- How is an IP packet forwarded when it reaches a border router? What happens instead in SCION? A sentence or two for each is enough.
> The router looks up a routing table and (usually based on longest-prefix matching of the destination address of the packet). The lookup returns an exit interface, to which the packet is forwarded. In SCION none of that happens: the packet already contains the forwarding information, so a router only needs to check the next hop information in the SCION packet header.

> [!question]- How do those different routing architectures influence the size of a packet?
> A SCION packet needs to contain all of its routing information (the full path it will be forwarded through). Consequently, the header overhead is higher compared to IP (where routing information is distributed on the various hops in the path).

> [!question]- Consider this scenario: network failure interrupts an inter-AS link, and BGP eventually reconverges to start using a backup link. Besides the connectivity interruption, how is a host using that link affected by a change in the network topology? Will they be aware of it?  (Think about what information about the routing is available to the end-hosts.)
> The topology change will be almost completely transparent to end-hosts running on the network: the routing is opaque for them. The end-host will have no information on how a packet is routed, and cannot influence the path they are forwarded on.

> [!question]- Consider the same setting as above in SCION: what are the differences? Can you think of an advantage of this approach?
> The end-host will detect that the traffic on a certain path is not being forwarded correctly (or the path will be revoked), and explicitly failover to another path. In this case the end-hosts have full information on the routing, and full power on its forwarding path. 
> This allows for fast failovers.

### Question 3
**SCION vs BGP**

> [!question]- How are available paths determined in BGP? How does it compare to SCION? (A sentence or two for each is enough.)
> BGP: routing announcements are globally propagated. SCION: paths are disseminated through beaconing, which is divided into core beaconing and intra-ISD beaconing.

> [!question]- What happens when a link fails in BGP and SCION?
> In BGP, packets will queue up or be lost until the network converges to a new states (self-healing). The time required for this process may be on the order of minutes.
> 
> SCION is not self-healing: the failed paths will be revoked, but end-hosts will need to be path-aware and select a new path for communication. The time required for this process is in the order of a few RTTs/beaconing intervals.

> [!question]- One key property of BGP is that ASes influence how traffic is routed based on BGP policies. In SCION, how do ASes control how packets are routed?
> ASes can choose to which customers to forward which beacons (PCBs). This provides end-hosts with a set of possible paths that are already conforming to policy, balancing expressiveness with economic incentives.

### Question 4
In this exercise, we take a closer look at the control plane
implementation of SCION.

> [!question]- What is an ISolation Domain (ISD)? What is a Core AS?
> The architecture of SCION mandates that the Internet is partitioned into ISDs, that are independently organized groupings of ASes. In each ISD, part of the ASes form the core, which is responsible for managing the whole ISD and has some special functions. 
> An ISD usually represents an area of common trust or of common legislation (e.g. countries, multinational federations).

> [!question]- Explain what is beaconing and how the distribution of PCBs (Path-segment Construction Beacon) works.
> Beaconing is the process through which paths are found in SCION. The Core ASes in an ISD (Isolation Domain) periodically flood the ISD with PCBs, by sending them in an anycast fashion (dubbed service anycast in SCION). Any AS receiving this packet will send it to its beacon server, that will add the current AS info and send the PCB to the ASes downstream. When beacons reach leaf nodes in the AS graph, the process is completed.

> [!question]- What is a path segment? Why is it called that way? What is the difference between up, down and core path segments?
> A path segment is any contiguous subsequence of ASes contained in a PCB, provided that at least one of the extremes is a core AS. They owe their name to the fact that they represent different segments of a whole path. Each path is in fact comprised of an up-path segment (from source AS to a core AS), possibly a core-path segment (from core AS to core AS, possibly on a different ISD), and a down-path segment(from core to destination AS). Cross-overs are possible, allowing packets to go from an up- to a down-segment without passing through the core.

> [!question]- Each AS has its own path server. There are regular ASes and core ASes. After beaconing, what type of path segments do each of them store? (excluding caches at various levels)
> -   Regular AS: contains up-path segments to reach the core ASes
> -   Core AS: contains down-path segments and core-path segments

### Question 5
**About the Project**

In the SCION project that was just released, you are to implement a
SCION client application (end-host). We provide a "SCION daemon", which
gives you an API to retrieve available paths from your AS to a remote AS.
The remote AS contains the grading/verifier host. Your task is to send
packets along specifically chosen paths to the remote verifier.

Notably, the SCION daemon runs on your end host together with the client,
and is separate from (but talks to) your AS's path server. It's a sort of
local cache of path segments.

> [!question]- Is a SCION daemon absolutely necessary for SCION to function, or do you think one can build an end host client application without using one? 
> No, it is not necessary. The SCION daemon provides a local cache of the paths, but client apps can (with enough logic) talk to the path server directly.

> [!question]- If you can build a client without the daemon, why is a daemon still useful?
> The daemon is useful for:
> - abstraction of complex protocol-level details
> - caching path segments
> - making clients explicitly aware of hidden paths by not giving it to them unless requested (of course, if the daemon does not have it cached, it won't know about any hidden paths either until clients request it)

> [!question]- In order to grade your choices (on which path you chose to send a packet along), the verifier host somehow knows about the path which a SCION packet traversed. How can they do that? Can client apps do the same?
> They inspect the SCION packet header, which contains the info fields and hop fields. Logically, the byte array of hop fields can be split into three segments: up, core, and down. The indices to split at are indicated by another field.
> 
> The reverse is possible as well (we can find out which path the verifier's response used), which is exactly what your AS Finder Test is.

In one of the later questions, you will be asked to use FABRID to select your path to reach the remote verifier. FABRID is an extension to SCION, where ASes that have it enabled can choose to expose some of its forwarding policies for end hosts to use (alongside path segments).

For example, an AS may make available the policy: "If you want, we support forwarding only over devices produced by manufacturer A". Then, FABRID-enabled clients can use these as constraints when querying for paths, and may choose to apply them to packetes it sends out (e.g. by encoding information like "For this packet, at hop 2, apply policy L100; at hop 3, apply policy L200.")

> [!question]- What are some realistic settings where clients may want to use the additional flexibility of FABRID to select paths?
> FABRID improves the transparency and flexibility of path selection.
> 
> Clients may want to use FABRID when there are imposed requirements, for example a state or company security policy to always use devices from a certain manufacturer. It can even be international, and an AS may announce a policy that it won't route over a certain country if the client requests it.

> [!question]- One can argue FABRID exposes internal details of intermediary AS topologies, which could be bad for security. Why is this an ill-formed argument to make?
> The AS does not reveal internal routing decisions; it just reveals the existence of a path that fulfills that policy. How the intra-AS routing is configured or whether there are multiple paths that fulfill that policy is not revealed.
> 
> The ASes also have full control over what policies they choose to announce, and if they don't want to reveal their router suppliers, they can just choose not to reveal such information.
> 
> Finally, it is also possible to use FABRID without announcing any policies and just make use of the source authentication + path validation properties it gives. You could also not use FABRID, as it is an optional component of SCION.
