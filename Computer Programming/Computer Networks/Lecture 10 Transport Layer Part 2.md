---
Tags: lecture
Created: 2023-05-30 14:59:16
---
(Links:: [[Lecture 9 Transport Layer Part 1|Lecture 9]] <- [[Computer Networks]] -> [[Lecture 11 Application Layer Part 1|Lecture 11]])
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
## UDP
- **Very thin** layer on top of IP
- Header provides **ports** needed to connect to remote applications
- UDP does not do:
	- Flow control
	- Congestion control
	- Retransmissions
- UDP has a length field
- UDP checksum includes fields from the IP header
## TCP
- many mechanisms for reliable delivery and error detection/correction
- many RFCs (Request For Comment) define TCP
- Provides a **reliable end-to-end byte stream** over an unreliable network
- Sequence numbers and acknowledgements allow reliable, in-order delivery and enable sliding window protocols
- TCP checksum uses same IP-header fields as the UDP checksum
- Window size used for flow control
- ACK, SYN and FIN flags used to establish/release connections
- Header length enables 0 or more 32-bit word options
- Three-way handshake: 
	- Connection request with SYN
	- Acknowledgment number is the previously received sequence number + 1
	- Initial sequence numbers are randomly generated
- Use seq. numbers and timestamp to detect duplicates
- Every **data byte** has its own sequence number
- duplicate acknowledgments indicate packet loss
- send acknowledgment via piggy-backing, otherwise wait up to 500ms
### AIMD
- **Congestion window**: Tracked on sender and specifies how many segments can be transmitted
- Look at the rate at which acknowledgements come back to determine the slowest link in the network
- "Slow Start": Increase the congestion window based on ack. rate
- Arbitrary threshold switches from "slow" start to **additive increase**
- Packet loss results in multiplicative decrease and return to additive increase
- Tahoe: Reset congestion window and set threshold to $\frac{1}{2}\times \text{window}$
- Reno (Tahoe + fast recovery): set congestion window to threshold $\frac{1}{2}\times \text{window}$

---
(Links:: [[Lecture 9 Transport Layer Part 1|Lecture 9]] <- [[Computer Networks]] -> [[Lecture 11 Application Layer Part 1|Lecture 11]])
References: