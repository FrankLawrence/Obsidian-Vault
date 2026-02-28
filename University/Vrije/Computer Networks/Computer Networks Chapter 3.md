---
Tags: 
Created: 2023-04-27 11:01:31
---
(Links:: [[Computer Networks]])
# 6 `fas:CheckCircle`
You receive the following data fragment: 0110 0111 1100 1111 0111 1101. You know that the protocol uses bit stuffing. Show the data after destuffing.

Byte stuffing adds a 0 after 5 consecutive 1's
0110 0**111 11**00 1111 0**111 11**01
0110 0111 1101 1110 1111 11
# 12 `fas:CheckCircle` 
Sixteen-bit messages are transmitted using a Hamming code. How many check bits are needed to ensure that the receiver can detect and correct single-bit errors? Show the bit pattern transmitted for the message 1101001100110101. Assume that even parity is used in the Hamming code.

5 check bits are required since $\log_2(16)=5$
00010011010
# 18 `fas:CheckCircle` 
Using the convolutional coder of Fig. 3-7, what is the output sequence when the input sequence is 10101010 (left to right) and the internal state is initially all zero?

| Internal state | Output bits |
| -------------- | ----------- |
| 100000         | 11          |
| 010000         | 01          |
| 101000         | 00          |
| 010100         | 10          |
| 101010         | 00          |
| 010101         | 00          |
| 101010         | 11          |
| 010101         | 00          |

Output sequence: 1101001000001100
# 24 `fas:CheckCircle`
A channel has a bit rate of 4 kbps and a propagation delay of 20 msec. For what range of frame sizes does stop-and-wait give an efficiency of at least 50%?

For a one bit sliding window we have: $0,5=\frac{1}{1+2*4kbps/f*0,02sec}$
$0,5+4kpbs/f*0,01sec=1$
$4kpbs/f*0,01sec=0,5$
$4kpbs/f=50$
$4000=50f$
$80=f$
This means we need a frame size of 80 bits to have an efficiency of 50%
# 30 `fas:CheckCircle` 
Suppose that the three-statement while loop near the end of protocol 6 was removed from the code. Would this affect the correctness of the protocol or just the performance? Explain your answer.

The while loop implements the advance of the edges of the windows. This affects the correctness of the protocol, since whithout you're not able to find the size of the windows.
# 36 `fas:CheckCircle`
Frames of 1000 bits are sent over a 1-Mbps channel using a geostationary satellite whose propagation time from the earth is 270 msec. Acknowledgements are always piggybacked onto data frames. The headers are very short. Three-bit sequence numbers are used. What is the maximum achievable channel utilization for

(a) Stop-and-wait?
It takes 0,001 seconds to load the 1000 bits onto the channel. Another 0,540 seconds are needed to send the data to the satellite and for the acknowledgement to come back.

(b) Protocol 5?
(c) Protocol 6?
# 42 `fas:CheckCircle`
What is the minimum overhead to send an IP packet using PPP? Count only the overhead introduced by PPP itself, not the IP header overhead. What is the maximum overhead?

PPP uses one escape byte (0x78), a one byte address field, a one byte control field (00000011 for an unnumbered frame), a 2 byte field dictating the protocol (can be 1 byte), a payload field with default length of 1500 bytes, a minimum 2 byte checksum field using CRC and one last escape byte. 
This means in total there are at least 1507 bytes with payload and 7 bytes for just the header information.

---
References: