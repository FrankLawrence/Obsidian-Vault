---
Tags: 
Created: 2024-10-22 22:09:30
cssclass: svg-embed
---
(Links:: [[Advanced Network Programming]])
> [!info] Objectives
> - Network Congestion  
> 	– What is network congestion? Why and when does it happen?
> - Challenges and goals?  
> 	– What are the key challenges and objectives for congestion control?
> - Key ideas  
> 	– How should we go about approaching congestion control?

![[Building a reliable transport.png|500]]
- **Round-trip** time estimation
	- single most important feature
	- estimate RTT and its *variance*
	- variance in RTT increases with load

> [!question] What is network congestion? Why and when does it happen?
> System has finite **capacity**.
> Offered load is at or below capacity.
> ![[Congestion 1.png|400]]
> Congestion:
> ![[Congestion 2.png|400]]
> Offered load *exceeds* capacity. To fight this, routers use queues to buffer the data.
> ![[Congestion 3.png|400]]
> # System performance
> ![[System performance congestion.png|500]]
> 1. Congestion Avoidance: Stay left of "knee"
> 2. Congestion Control: Stay left of "cliff"
> 3. Congestion Collapse: Avoid at all costs

> [!question] What are the key challenges and objectives for congestion control?
> - Use network resources "efficiently"
> - Ensure "fair" resource allocation
> - Adapt "quickly"
> - No "explicit coordination"

# Window-based Control
Congestion window (*cwnd*)
- Bytes awaiting acknowledgement
- **Throttles** sender
- `wnd <- min(rwnd, floor(cwnd))`
- If there's no congestion -> *increase* cwnd
- On detecting congestion -> *decrease* cwnd

![[Window-based control.png|500]]

> [!question] How should we go about approaching congestion control?
> ![[Files/Congestion control|500]]
> - **Slow start**: Send data, doubling every time if you don't detect congestion, begin at the very bottom

# Slow Start
For every ACK, increment cwnd by 1. cwnd is doubled every round-trip time (RTT).
Stop on encountering *loss*. Use last cwnd to limit "slow" start.

![["Slow" Start 1.png|300]]
![["Slow" Start 2.png|300]]
# Congestion Avoidance and Control
- Cautiously **probe** for additional throughput (*additive increase*)
- Aggressively **backoff** on loss (*multiplicative decrease*)
- For **every RTT**, increment cwnd by 1. For every ACK, cwnd += 1/cwnd
![[Pasted image 20241022232529.png|400]]

[[Lecture 10 Transport Layer Part 2#^7d4bba|Additive Increase and Multiplicative Decrease (AIMD)]]: gentle increase, rapid decrease; drives towards "fairness"
# Delay-based Approach
Try to estimate the delay that a packet experiences when waiting in the buffer of a router.
![[Delay-based approach.png|500]]
![[Delay-based Approach 2.png|500]]

---
References: