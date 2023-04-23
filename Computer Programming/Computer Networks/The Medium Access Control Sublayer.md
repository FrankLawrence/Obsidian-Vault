---
Tags: 
Created: 2023-04-21 20:32:40
---
(Links:: [[Computer Networks]])
Protocol used to determine who goes next on a multiaccess channel
# The Channel Allocation Problem
## Static Channel Allocation
## Assumptions for Dynamic Channel Allocation
- N independent stations each generate frames for **transmission independently**
- All stations transmit and receive over a **single channel**
- two frames transmitted at the same time can **collide**, which can be detected by all stations
- frames can be transmitted can be begin at **any time** or in certain intervals (**slots**)
- Stations can or cannot tell if the channel is in use (**Carrier Sense**)
- 
# Multiple Access Protocols
## ALOHA
- users transmit to central computer whenever they have data to be sent
- central computer resends frame to all stations to make sure that incoming frames haven't collided
- sender waits random amount of time if frame was destroyed
- frame size is uniform

> [!danger] Collision
> If the first bit of a new frame overlaps with just the last bit of a frame that has almost finished, both frames will be totally destroyed and will have to be retransmitted

- **Slotted ALOHA**: time divided into discrete intervals called **slots** each as long as a frame
	- halves the vulenrable period
## Carrier Sense Multiple Access Protocols
- **1-Persistent CSMA**: station listens to channel to see if anyone else is transmitting -> send data if idle; otherwise wait
	- wait random amount of time if collision occurs

  > [!danger] Collision
  > If two stations become ready while a third is sending, both will send after the third station finishes resulting in a collision
  > 
  > If the bandwidth-delay product is too large, a station will not sense any signal on the channel and begin transmitting even when a second station has already begun sending
  
- **Non-persistent CSMA**: stations are less "greedy"
	- if the channnel is in use the station will wait a random period of time and then sense again
	- longer delays than 1-persistent
- **P-persistent CSMA**: sense channel and send transmit (if idle) with a probability $p$
- ![[Comparison of the channel utilisation versus load fro various random access protocols.png|500]]
- **CSMA/Collision Detection**: stations attempt to transmit and abort transmission when collision is detected -> wait random time until trying again
	- ![[CSMA-CD.png|500]]
  > [!danger] Worst-case collision
  > $\tau$: time for signal to propagate between two furthest stations
  > - start transmission at $t_0$
  > - second station starts transmitting at $t_0+\tau-\varepsilon$
  > - original station detects collision at $2\tau-\varepsilon$
  - CSMA/CD is ALOHA with slot width of $2\tau$
  - reducing contention period duration improves throughput
  - The longer the frame the higher the throughput but lower the latency
## Collision-Free Protocols
- **Bit-Map Protocol**:
	- contention period has $n$ slots
	- station $i$ transmits 1 in slot $i$ if it has a frame to send
	- stations begin transmitting in numerical order after contention period is over
	- ![[The basic bit-map protocol.png|500]]
	- **Never** any collisions
	- high-numbered stations rarely have to wait for the next scan
	- channel efficiency: $\frac d{d+n}$
- **Token Passing**: 
	- pass token from one station to another (in predefined order) indicating who has permission to send
	- stations can send a frame (if available) before passing it on
	- no bias for low- or high-numbered stations
	- each station must wait for all N-1 stations to send the token around
- **Binary Countdown**: stations broadcast their address as a binary bit string, starting with the high-order bits
	- bits in address from different stations are ORed together by the channel
	- as soon as a station sees that a high-order bit position with 0 in its address has been overwritten with a 1, it gives up
	- high-numbered stations have higher priority
	- ![[Binary Countdown Protocol.png|400]]
	- Channel efficiency: $$\frac d{d+\log_2n}$$
## Wireless LAN Protocols
> [!danger] No Collision Detection
> A station on a wireless LAN may not be able reachable from all other stations because of the limited radio range

- ![[Wireless LAN Protocols.png]]
	- If A and C send to B, both would conclude that they can transmit since they are out of each other's range
	- (a) A and C are **hidden terminals** when transmitting to B
	- B is transmitting to A at the same time that C wants to transmit to D -> C thinks the channel is in use and cannot send to D (dashed line)
	- (b) B and C are **exposed terminals** when transmitting to A and D
- **Multiple Access with Collision Avoidance** (**MACA**): 
# Ethernet
## Classical Ethernet MAC Sublayer Protocol
## Switched Ethernet
## Fast Ethernet
## Gigabit Ethernet
## 10-Gigabit Ethernet
## 40- and 100-Gigabit Ethernet
## Retrospective on Ethernet
# Wireless LANs
## The 802.11 Architecture and Protocol Stack
## The 802.11 MAC Sublayer Protocol
## The 802.11 Frame Structure
## Services
# Bluetooth
## Bluetooth Architecture
## Bluetooth Applications
## The Bluetooth Protocol Stack
## The Bluetooth Link Layers
## The Bluetooth Frame Structure
# DOCSIS
# Data Link Layer Switching


---
References: