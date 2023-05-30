---
Tags: lecture
Created: 2023-05-30 14:59:16
---
(Links:: [[Computer Networks]])
# Revisiting reliable delivery and flow control
## Reliable delivery
- The transport layer is responsible for providing a **reliable** data stream over an unreliable network

> [!question] Did we not take care of this in the data link layer?
> No - We have to have reliability **across** networks and to the **destination process** as well, not just individual links
- Transport layer checks the end-to-end correctness of data

> [!question] Why not do error control only at the transport layer?
> Link layer retransmission is faster since routers can retransmit the message, without the host having to be contacted 

- Machines can receive data and pass it onto the next layer
	- If they crash the do not send an acknowledgement
	- The packet is retransmitted and the receiver can interpret this as new data -> unreliable 
- Recovery from a layer $k$ crash can **only be done by layer $k+1$**
## Flow control
- Needed to slow down the sender if the **receiver** cannot handle the data rate
- Sliding window protocols used 
	- Send multiple frames at the same time before waiting for an acknowledgement
- Received packets are buffered at receiver before they are read by the application
	- Available buffer space is used as receiver window size
# Congestion control and bandwidth allocation
- Needed to slow down the sender if the **network** cannot handle the data rate
- The available network resources must be evenly distributed 
- Max-min fairness: Maximises minimum bandwidth, then uses excess bandwidth where possible
	- bandwidth given to one flow cannot be increased without decreasing the bandwidth given to another flow with an allocation that is no larger
	- ![[Max-min bandwidth allocation for four flows.png|500]]
## Convergence
- When new connections enter the network, the bandwidth needs to be reallocated
- we need to dynamically adjust bandwidth usage using trial and error
- Keep trying to increase bandwidth usage and slow down when congestion signal is received
- **Additive increase, multiplicative decrease** (**AIMD**)
	- ![[Additive and multiplicative bandwidth adjustments.png|500]]
	- Additive increase/decrease moves the point at a 45° angle since both bandwidth's are increased the same amount
	- Multiplicative increase/decrease moves the point at an angle proportional to the current bandwidth
		- Users with a higher bandwidth get a larger increase
	- ![[Additive Increase Multiplicative Decrease control law.png|500]]
# TCP and UDP
- 

---
References: