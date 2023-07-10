---
Tags: 
Created: 2023-04-21 03:14:19
---
(Links:: [[Computer Networks]])
 Algorithms for achieving reliable and efficient communication of frames between two adjacent machines
# Data Link Layer Design Issues
- the data link layer has to:
	- provide a well-defined service interface to the network layer
	- frame sequences of bytes as self-contained segments
	- detect and correct transmission errors
	- regulate flow of data for slow receivers
- packets from the network layer encapsulated into **frames** for transmission
- frames consist of a Header, Payload field and Trailer
## Services Provided to the Network Layer
- data link layer designed to offer various services:
	- **Unacknowledged connectionless service**: independent frames sent to destination machine without having the destination machine acknowledge them (e.g. Ethernet)
	- **Acknowledged connectionless service**: no connections used, but each frame sent is individually acknowledged -> sender knows if frame was sent correctly or not (e.g. ureliable channels like WiFi)
	- **Acknowledged connection-oriented service**: source and destination machines establish a connection before any data are transferred
		- frames are numbered for guaranteed arrival and correct order
		- used for long unreliable links such as satellite channels (previous services could cause a frame to be sent and received multiple times)
## Framing
- data link layer breaks up bit stream into frames
- for each frame a checksum is calculated and included in the frame when transmitted
- destination machine recalculates checksum and compares
- methods of breaking up the bit stream:
	- Byte count
	- Flag bytes with byte stuffing
	- Flag bits with bit stuffing
	- Physical layer coding violations
- **Byte count**: field in the header specifies the number of bytes in the frame
	- ![[Byte stream.png|500]]
	- can be easily garbled on transmission error -> unreliable on it's own
- **Byte stuffing**: each frame starts and ends with special flag byte
	- two consecutive flag bytes indicate new frame -> used for resynchronization
	- escape byte allows the flag byte to be used as data
	- escape bytes must also be escaped to be used as a data byte
	- escape bytes removed before being passed onto the network layer
	- ![[Byte stuffing.png|500]]
- **Bit stuffing**: each frame begins and ends with special bit pattern $01111110$ or $0x7E$
	- when sender's data link layer encounters five 1s (bit pattern) in the data, a 0 bit is automatically stuffed
	- receiver removes 0 bit automatically if five consecutive 1s are detected
- length of frame depends on contents of data
	- byte stuffing will maximally increase the size by 100%
	- bit stuffing will never exceed an increase of 12,5% 
- used reserved signals to indicate start and end of frames with [[4B-5B|4B/5B]]
	- reserved signals are easy to find 
	- no need to stuff data
## Error Control
- receiver sends back special control frames with positive (arrived safely) or negative (retransmit frame) acknowledgement about incoming frames
- sender waits for acknowledgement of a frame it sent -> delay if frame never received or acknowledgement lost
- timer set to expire after an interval long enough to transmit frame and receive acknowledgement
	- timer will go off, alerting the sender to a potential problem -> resend frame
	- outgoing frames are assinged sequence numbers so no frame is received twice
## Flow Control
- **[[#Adding Flow Control: Stop-and-Wait|feedback-based flow control]]**: receiver sends back info to sender giving it permission to send more data
- **rate-based flow control**: built-in limit to rate at which sender can transmit data
# Error Detection and Correction
- Error-correcting codes: enough redundant information to deduce whwat the transmitted data must have been (used on noisy channels)
- Error-detecting codes: enough redundancy to allow receiver to deduce that an error has occured (but not which)
- redundant bits are treated the same as data bits and can be corrupted aswell
- thermal noise can cause single-bit errors
- errors might come in bursts
## Error-Correcting Codes
### Hamming codes
- $m$ is data bits and $r$ is redundant bits
- **codeword**: $n$-bit unit containing data and check bits
- **code rate**: fraction of codeword that is not redundancy, $m/n$
- **Hamming distance**: number of bit positions in which two codewords differ
	- a hamming distance of $d$ means $d$ single-bit errors are needed to convert one to the other
- to detect $d$ errors, you need a distance of $d+1$ so that $d$ single-bit errors can't result in another valid codeword
- to correct $d$ errors, you need a distance of $2d+1$ so the legal codewords are far enough apart from eachother that the original codeword is still closest to the invalid codeword
- **Hamming codes**: bits of the codeword are numbered consecutively, starting with bit 1 at the left end, bit 2 to its immediate right, and so on
	- bits at powers of 2 are check bits, the rest is filled up with the $m$ data bits
	- which check bits contribute to the data bit in position $k$ -> rewrite $k$ as a sum of powers of 2
	- a bit is just checked by those bits that it's made of (11=8+2+1)
	- the modulo 2 sum (parity) should be even -> check bit is one if there is an uneven amount of 1s in the checked bits, zero otherwise

|     | 1    | 2    | 3    | 4    | 5    | 6    | 7    | 8    | 9    | 10   | 11   | 12   |
| --- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
|     | 0001 | 0010 | 0011 | 0100 | 0101 | 0110 | 0111 | 1000 | 1001 | 1010 | 1011 | 1100 |
| 1   | x    |      | x    |      | x    |      | x    |      | x    |      | x    |      |
| 2   |      | x    | x    |      |      | x    | x    |      |      | x    | x    |      |
| 4   |      |      |      | x    | x    | x    | x    |      |      |      |      | x    |
| 8   |      |      |      |      |      |      |      | x    | x    | x    | x    | x    |
- [[Hamming codes table]]

> [!example] Hamming codes
> Message: 11010101
> Codeword: \_\_1\_101\_0101
> Positions:**12**3**4**567**8**9...
> 
> Codeword: **1**\_**1**\_**1**0**1**\_**0**1**0**1
> Positions **1**23456789...
> 
> Codeword: 1**11**\_1**01**\_0**10**1
> Positions 1**2**3456789...
> 
> Codeword: 111**1101**\_010**1**
> Positions 123**4**56789...
> 
> Codeword: 1111101**00101**
> Positions 1234567**8**9...

- for even parity sums, each check result should be zero
- the set of check results form the **error sydrome** that tells us where the error is

> [!example] Hamming codes error correction
> Codeword: 1111101001**1**1
> Check bit 1: **1**1**1**1**1**0**1**0**0**1**1**1 = 1
> Check bit 2: 1**11**11**01**00**11**1 = 1
> Check bit 4: 111**1101**0011**1** = 0
> Check bit 8: 1111101**00111** = 1
> -> Single-bit error at location 1011 (11)

### Binary convolutional codes
- encoder processes a sequence of input bits and generates a sequence of output bits
- output depends on the current and previous input bits (encoder has memory)
- **constraint length**: number of previous bits on which the output depends
- ![[The binary convolutional code.png|500]]
	- convolutional code of $r=1/2$ and $k=7$
	- one input bit on the left-hand side produces two output bits on the right-hand side that are XOR sums of the input and internal state
	- every input shifts the values in the registers to the right
### Reed-Solomon codes
### Low-Density Parity Check codes
## Error-Detecting Codes
### Parity Bit
- parity bit appended to the data (chosen so that number of 1 bits is even)
- 1011010->10110100
- to detect a block with a single 1-bit error, one parity bit per block will suffice
- error rate is $10^{-6}$ and 1000 bits per block -> one every 1000 blocks is false -> extra block (1001 bits) must be sent again
- **Interleaving**: consider each block a matrix of $n$ bits wide and $k$ bits high -> compute parity bits for each row -> can more easily detect burst errors
### Checksum
- checksum placed at the end of message as a complement of the sum function -> sum of entire codeword shows error if result is not 0
- one's complement sum derived from taking sum modulo $2^{16}$ and adding high-order overflow bits to low-order bits

> [!warning] 
> Cannot detect deletion or addition of zero data, nor swapping parts of the message, and provides weak protection against message splices
- **Fletcher's checksum**: product of data and *position of data* added to sum
### Cyclic Redundancy Check
- bit strings are representated by polynomials with coefficients of 0 and 1 only
	- 110001 represents a six-term polynomial with coefficients 1,1,0,0,0 and 1: $1x^5+1x^4+0x^3+0x^2+0x^1+1x^0$
- sender and receiver agree upon **generator polynomial** $G(x)$ in advance
- can detect double bit errors, odd bit errors, burst errors < r (degree of polynomial) bits

> [!info] CRC algorithm
> 1. Let $r$ be the degree of $G(x)$. Append $r$ zero bits to the lower-order end of the frame so it now contains $m+r$ bits and corresponds to the polynomial $x^rM(x)$
> 2. Divide the bit string corresponding to $G(x)$ into the bit string corresponding to $x^rM(x)$, using modulo 2 division
> 3. Subtract the remainder (which is always $r$ or fewer bits) from the bit string corresponding to $x^rM(x)$ using modulo 2 subtraction. The result is the checksummed frame to be transmitted. Call its polynomial $T(x)$
# Elemetary Data Link Protocols
## Basic Transmission and Receipt
- procedure call `wait_for_event(&event)` returns when something happens (frame arrives)
- receiver computes checksum and data link layer is informed if incorrect (`event=cksum_err`) or correct (`event=frame_arrival`)
- frame header isn't passed onto network layer -> network and data link protocols are separate
- sequence numbers assigned to each frame run from 0 up to MAX_SEQ, defined by each protocol
- **Frame header** consists of four control fields: *kind, seq, ack, and info*
	- kind: does the frame contain data
	- seq and ack: used for sequence numbers and acknowledgements
	- info: contains a single packet (data frame) or nothing (control frame)
- Protocols assume unreliable channels, so data link layers use timers to recover from lost frames by timing out and receiving an interrupt signal if no reply is received within a set time
## Simplex Link-Layer Protocols
### Utopia: No Flow Control or Error Correction
- sender continously fetches packet's from network layer and constructs outbound frame
- *info* frame only used since there are no errors or flow control in this perfect world
- receiver waits for undamaged frame to arrive
- frame removed from hardware buffer and data passed on to network layer 
### Adding Flow Control: Stop-and-Wait
> [!warning] 
> Sender can flood receiver with frames faster than the latter is able to process them
- **Stop-and-wait**: receiver provides feedback (little dummy) to the sender -> permission to transmit next frame
- sender must wait for little dummy frame arrives
	- no need to put information on dummy frame since receiver only expects an acknowledgement
- [[The Physical Layer#^c0e815|half-duplex]] channel needed to send and receive data 
### Adding Error Correction: Sequence Numbers and ARQ
- frames may be damaged or lost completely
	- checksum detects if frame is damaged

> [!warning] 
> If the acknowledgement frame is lost, then the sender sends a duplicate frame after the timer has run out -> receiver interprets duplicate frame as a new frame
> 
> Receivers distinguishes first frame from retransmission with sequence number

- a 1-bit sequence is sufficient 
	- after acknowledgement the sequence number is incremented modulo 2 (0->1, 1->0)
	- if acknowledgement is lost the sequence number stays the same
- **Automatic Repeat reQuest**: sender waits for positive acknowledgement before advancing to next data item
# Improving Efficiency
## Goal: Bidirectional Transmission, Multiple Frames in Flight
### Bidirectional Transmission: Piggybacking
- *kind* field in the header of an incoming frame indicates whether the frame is data or an acknowledgement to the receiver
- acknowledgements are attached to next outgoing data frames -> temporary delay in outgoing acknowledgements
- better use of available channel bandwidth
- acknowledgement waits a fixed number of miliseconds -> data link layer sends separate acknowledgement frame if no packet arrives in time
### Sliding Windows
- sender maintains a set of sequence numbers corresponding to frames it is permitted to send (**sending window**)
- receiver maintains **receiving window** correspoding to set of frames permitted to accept
- packet is fetched from network layer and upper edge of window is advaced by one
- acknowledgement advances lower edge by one
- sender needs $n$ buffers to hold unacknowledged frames
## Examples of Full-Duplex, Sliding Window Protocols
### One-Bit Sliding Window
- acknowledgement field contains the number of the last frame received without error
- If receiver waits for sender's first frame before sending one of its own, every frame is accepted

> [!Failure]
> If $A$ and $B$ simultaneously initiate communication, their first frames cross. [[Sliding Windows Abnormal case.canvas]]
### Go-Back-N
- sender can transmit up to $w$ frames before blocking, instead of just 1

> [!info] Link utilization
> - Frame size: $f$
> - Window size (in frames): $w$
> - Bandwidth (data rate of channel): $B_p$
> - Bandwidth (Frames per second): $B=B_p/f$
> - Propagation delay: $D$
> $$\text{Link utilization}=\frac{w}{1+2BD}$$
> - "+1": acknowledgement frame will not be sent until after a complete frame is received

- If a frame arrives damaged, the receiver should discard all subsequent frames
### Selective Repeat
- receiver accepts and buffers correct frames received following a damaged or lost one
- receiver sends back a negative acknowledgement that stimulate retransmission before the timer expires

---
References: