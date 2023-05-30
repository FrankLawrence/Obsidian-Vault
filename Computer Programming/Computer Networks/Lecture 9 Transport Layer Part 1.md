---
Tags: lecture
Created: 2023-05-16 22:53:04
---
(Links:: [[Computer Networks]])
- Physical layer moves bits over a phisical channel
- Data link layer translates frames to and from bit/byte streams, provides error detection/correction and flow control
- Network layer transmits packets across the network from a source host to a destination host and provides congestion control with transport layer
# Transport layer responsibilities and challenges
- Provides a **reliable** data stream over an **unreliable** network
- Provides communication between **processes**
- The topology of the network is not visible -> Transport layer only present at source and destination
## Berkeley Socket primitives
Interface exposed to the application layer:
1. Socket - create a new communication **endpoint**
2. Bind - assign a **local address** to an endpoint
3. Accept - passively establish an incoming connection
- UDP doesn't use bind since it is connection-less
- Used by TCP
- Applications can make a socket and bind to a port which it communicate with
- Incoming connection specifies on which port it wants to connect
## Addressing
- Internet uses 
	- IP addresses for **Network Service Access Point**
	- ports for **Transport Service Access Point**
## Process server
- Listens for incoming requests on different ports
- Starts a new application to handle the incoming data and shuts it down when not in use
- Server ports typically are hardcoded

> [!question] How do incoming connections know the correct port?
> Server ports are typically hardcoded

- clients setup the connection to the server and are dynamically assigned a port number for their application
- A port can be connected to two NSAPs at the same time (e.g. two IP addresses)
# Connection establishment and release
## Connection establishment
- Sequence numbers used to distinguish duplicates
- **hop limit** removes old packets
	- After time $T$, sequence numbers are safe to wrap around
- **time-of-day clock** decides which sequence number to choose (works when host crashes)
	- low-order $k$-bits of the clock are used as the $k$-bit initial sequence number
- Sequence Number Limits
	- Sequence numbers that reappear within $T$ seconds are a retransmission
	- Maximum sending rate: $\frac{2^x}{T}$ Bps
		- $x$: length of sequence number
		- $T$: Maximum time for packets
- By the time sequence numbers wrap around, old segments with the same sequence number are long gone
### Three-way Handshake
- Send connection request and agree on starting sequence number
- receive an acknowledgement of the previously sent sequence number
- Duplicates: when receiving an acknowledgement of packet that was not intended, send a *Reject* message back
## Connection release
- Asymmetric disconnect
	- Connection ended by either participant without agreement -> may result in data loss
- Symmetric disconnect
	- Two armies problem: Participants must send an acknowledgement for the acknowledgement -> "infinite" ack must be sent -> no solution
	- The last party to send a message cannot know if it arrived

---
References: