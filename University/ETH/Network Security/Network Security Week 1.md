---
Tags: exercise
Created: 2026-01-21 16:49:32
---
(Links:: [[Network Security]])
## Network refresher

### Question 1

> [!question] Explain what bandwidth allocation is, and what requirements we pose on a bandwidth allocation algorithm.
> Bandwidth allocation is the way the bandwidth available on a link is split among different traffic flows. We want it to be efficient (use as much bw as possible) and fair (no one gets too much bw), while avoiding congestions.
One example of bandwidth allocation is congestion control.

> [!question] How does UDP deal with bandwidth allocation?
> The UDP protocol has no congestion control scheme.

> [!question] How does TCP deal with bandwidth allocation?
> Slow start and Additive Increase Multiplicative Decrease.

> [!question] The simpler nature of UDP is not necessarily a disadvantage. When is UDP preferred over TCP?
> When most of the features of TCP are not required, like media streaming services. In these cases, the loss of a limited number of packets is often acceptable.

> [!question] TCP is more complex than UDP. This complexity results in overheads, both in time and storage space. Let's focus on storage: the server has to store the state of each connection in its memory. Do you see a potential problem with this?
> An attacker can open a high number of TCP connections and keep them open, filling the server memory and preventing it from accepting legitimate connections. This attack is called [[DDoS Attack#SYN/ACK Flood]].

### Question 2
> [!question] Explain why congestion is detrimental to networks.
> ![[Congestion Control Goodput.svg|600]]
> Network links work a bit like roads. If there are many cars on a road, traffic doesn't just flow smoothly at maximum capacity. Any minimal disruption causes endless lines of vehicles, and the road could stop being usable. In a congested network routers often can't cope, and the usable bandwidth on some links could even drop to zero. This results in dropped packets. Packet losses, in turn, cause retransmissions in TCP, which make the situation even worse. This situation is called *congestion collapse*.

### Question 3
Routing algorithms are at the heart of routers. [[Lecture 7 Network Layer Part 1#Open Shortest Path First (OSPF)|Open Shortest Path First (OSPF)]], is a link-state protocol used in many real world scenarios. In OSPF, every link has a specific cost and the goal is to find the best possible route. Here is a simple explanation of how it works: <https://www.auvik.com/franklymsp/blog/ospf-protocol-explained/>.

> [!question] Consider the following network. The square nodes are local-area networks (LANs), while the round nodes are routers. LANs can only receive and generate packets, they can't route incoming traffic. Fill in the routing tables below.
> 
> ![OSPF network](assets/ospf-network.png)
> 
> R1:
> 
> | target | next hop |
> | :----: | :------: |
> |   N1   |   ...    |
> |   N2   |   ...    |
> |   N3   |   ...    |
> 
> R2:
> 
> | target | next hop |
> | :----: | :------: |
> |   N1   |   ...    |
> |   N2   |   ...    |
> |   N3   |   ...    |
> 
> R3:
> 
> | target | next hop |
> | :----: | :------: |
> |   N1   |   ...    |
> |   N2   |   ...    |
> |   N3   |   ...    |
> 
> 
> R4:
> 
> | target | next hop |
> | :----: | :------: |
> |   N1   |   ...    |
> |   N2   |   ...    |
> |   N3   |   ...    |
> 
> R5:
> 
> | target | next hop |
> | :----: | :------: |
> |   N1   |   ...    |
> |   N2   |   ...    |
> |   N3   |   ...    |
> 
> 
> N1:
> 
> | target | next hop |
> | :----: | :------: |
> |   N2   |   ...    |
> |   N3   |   ...    |
> 
> N2:
> 
> | target | next hop |
> | :----: | :------: |
> |   N1   |   ...    |
> |   N3   |   ...    |

_Solution_:
R1:

| target | next hop |
| :----: | :------: |
|   N1   |    N1    |
|   N2   |    R3    |
|   N3   |    R2    |

R2:

| target | next hop |
| :----: | :------: |
|   N1   |    R1    |
|   N2   |    R3    |
|   N3   |    R4    |

R3:

| target | next hop |
| :----: | :------: |
|   N1   |    R1    |
|   N2   |    R5    |
|   N3   |    R2    |


R4:

| target | next hop |
| :----: | :------: |
|   N1   |    R2    |
|   N2   |    N2    |
|   N3   |    N3    |

R5:

| target | next hop |
| :----: | :------: |
|   N1   |    R3    |
|   N2   |    N2    |
|   N3   |    R3    |

N1:

| target | next hop |
| :----: | :------: |
|   N2   |    R1    |
|   N3   |    R1    |

N2:

| target | next hop |
| :----: | :------: |
|   N1   |    R5    |
|   N3   |    R4    |

> [!question] You want to isolate the flows N1 $`\rightarrow`$ N3 and N1 $`\rightarrow`$ N2. Does this configuration satisfy this property? If not, how would you change the costs so that it does?
> Path 1: N1 $`\rightarrow`$ R1 $`\rightarrow`$ R3 $`\rightarrow`$ R5 $`\rightarrow`$ N2.
> 
> Path 2: N1 $`\rightarrow`$ R1 $`\rightarrow`$ R2 $`\rightarrow`$ R4 $`\rightarrow`$ N3.
> 
> The property is not respected as the first link is shared. The cost between N1 and R3 must be lower than 8, so that N1 will route any packet with target N2 through R3 instead.

> [!question] An external authority forces you to add a static route (with maximum priority) that sends any packet that reaches R5 to the link R5 $`\rightarrow`$ R4. Ignoring the longer routes and the unused links, what problems can arise?
> The danger of congestion increases significantly. Traffic from N1 to N2 will now go through R4, as well as traffic from N2 to N1. Basically, all traffic goes through R4, which probably needs to be upgraded in order to avoid problems.


---
References: