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
- **CSMA/Collision Detection**: stations attempt to transmit and abort transmission when collision is detected -> wait random time until trying again ^71a35b
	- stations try to claim the channel for use
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

- ![[Wireless LAN Protocols.png|600]]
	- If A and C send to B, both would conclude that they can transmit since they are out of each other's range
	- (a) A and C are **hidden terminals** when transmitting to B
	- B is transmitting to A at the same time that C wants to transmit to D -> C thinks the channel is in use and cannot send to D (dashed line)
	- (b) B and C are **exposed terminals** when transmitting to A and D
- **Multiple Access with Collision Avoidance** (**MACA**): sender stimulates receiver to broadcast short frame so other stations dont transmit for duration of the upcoming (large) data frame
	- ![[MACA Protocol.png|500]]
	- A sends Request to Send (RTS) frame containing length of data frame to B -> B replies with Clear to Send (CTS) frame with copy of data frame length
	- E hears RTS but not CTS so it can send since it knows the station receiving data is not in range
# Ethernet
## Classical Ethernet MAC Sublayer Protocol
- Frame format:
	- 8 byte preamble: contains bit pattern 10101010 with last byte (*frame delimeter*) being 10101011
	- 6 byte destination and source addresses: 
		- first bit indicates if the destination is ordinanry (0 bit) or for a group (1 bit)
		- address with all 1s used for broadcasting
		- source addresses are globally unique and assigned by IEEE and during manufacture process
	- 2 byte type/length field: 
		- specifies which process (based on network-layer protocol) to give the frame to (greater than 1536)
		- specifies the length of the frame (less than or equal to 1536)
	- 1500 byte data: cannot by smaller than 46 bytes for collision detection -> Pad field fills out the frame to minimum size
		- If a short frame is sent and a collision occurs at $\tau$, the sender will detect the collision at $2\tau$
		- Sender thinks the frame was sent successfully
		- The frame must be long enough so that the sender can detect collision and stop transmitting before end of frame -> time to send frame must take longer than $2\tau$
		- minimum packet size for reliable detection: $2\tau\times R\text{ (R = data rate)}$
	- 4 byte Checksum: 32-bit CRC
- CSMA/CD with Binary Exponential Backoff
	- after $i$ collisions, a random nnumber between 0 and $2^i-1$ is chosen and that number of slots is skipped
## Switched Ethernet
- ![[Hub.png|400]]
	- each station has a dedicated cable running to a central hub where all wires are attached electrically
- ![[Switch.png|400]]
	- Switch contains high-speed backplane that connects all of the ports
- switches output frames to the ports for which those frames are destined
- on [[The Physical Layer#^ce1a72|full-duplex cables]], both the station and port can send a frame on the cable at the same time -> CSMA/CD is not needed 
- multiple frames can be sent to the switch simultaneously
	- if the output port is the same, the switch must use a buffer for a frame
- every computer connected to a hub can see the traffic sent between all other computers
# Wireless LANs
## The 802.11 Architecture and Protocol Stack
## The 802.11 MAC Sublayer Protocol
- receiver cannot transmit while listening for noise burst -> relies on `ACK` message from receiver -> *retransmits* frame
- **CSMA with Collision Avoidance** (**CSMA/CA**): 
	- if channel is being used, wait until idle then start random backoff
	- pause backoff countdown when channel is in use
	- send data once counter reaches 0 and wait for acknowledgement
	- if acknowledgement wasn't received, double backoff period (exponential) and try again
	- ![[CSMA-CA.png|500]]
	- Virtual sensing: each station keeps a logical record of when the channel is in use by tracking the Network Allocation Vector (length of the frame) of each frame
- Automatic Power Save Delivery: Send data to client after client sends data to AP (good for applications with frequent traffic in both directions) -> Client can go to sleep until theres more traffic to send or receive
## The 802.11 Frame Structure
- Frame control field (2): 11 subfields
	- Protocol version (2): set to allow future versions of 802.11 to operate at the same time in the same cell
	- Type (2): data, control or management frame
	- Subtype (4): e.g. ACK, RTS or CTS
	- To DS and From DS (1): frame going to or coming from network connected to AP
	- More fragments (1): more fragments will follow
	- Retry (1): retransmission of earlier frame
	- Power management (1): sender is going into power-save mode
	- More data (1): sender has additional frames
	- Protected (1): frame body has been encrypted for security
	- Order (1): higher layer expects the sequence of frames to arrive in order
- Duration (2): how long the frame and acknowledgement will occupy the channel in microseconds (used to manage NAV)
- Address 1 (6): Recipient (AP)
- Address 2 (6): Transmitter
- Address 3 (6): distant endpoint (possibly internet)
- Sequence (2): numbers frames for duplicates
- Data (2312): first bytes are Logical Link Control (identifies high-layer protocol) where payloads should be passed
- Check sequence (4): 32-bit CRC
# Data Link Layer Switching
## Learning Bridges
- ![[Bridges and a hub connecting to point-to-point stations.png|500]]
	- bridge decides whether to forward (e.g. A --> B1 --> C) or discard (e.g. E --> F) each frame
- if there is more tha one station the [[#^71a35b|CSMA/CD]] protocol is used
- bridges manage big hash tables which lists each possible destination and their output port
- **Flooding algorithm** used to fill entries of table: every incoming frame for an unkown destination is output on all the ports connected to the bridge except the one it arrived on
- **Backwards learning**: bridges look at source address of incoming frame to tell which port it's associated with

> [!summary]- Routing for an incoming frame
> 1. If the port for the destination address is the same as the source port, discard the frame.
> 2. If the port for the destination address and the source port are different forward the frame on to the destination port
> 3. If the destination port is unknown, use flooding and send the frame on all ports except the source port

- frame is forwarded as soon as the destination header field has come in, before the rest of the frame has arrived
## Spanning-Tree Bridges
- bridges can be connected by two cables for higher reliability -> redundant frames will be sent over multiple lines to the other bridge
- forwarding to an unknown address creates an infinite loop
- bridges build a spanning tree where there is a unique path from each source to each destination
## Virtual LANs
- By keeping LANs no larger than they need to be, the impact of broadcast traffic is reduced
- network administrator decides how many VLANs there will be, which computer swill be on which VLAN, and what the VLANs will be called
- bridges have configuartion table which tells which VLANs are accessible via which ports
- ![[VLANs on a bridged LAN.png|500]]
- IEEE 802.1Q Standard
	- ethernet format contains VLAN tag
	- bridges have to be VLAN aware
	- all machines on a port must belong to the same VLAN
	- VLAN protocol ID (2 bytes): value 0x8100
	- Subfields (2 bytes): VLAN identifier (12b), priority field (3b), Canonical format indicator

---
References: