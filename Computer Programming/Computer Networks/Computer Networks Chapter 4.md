---
Tags: 
Created: 2023-06-02 13:40:58
---
(Links:: [[Computer Networks]])
# 4.6
What is the length of a contention slot in CSMA/CD for 
(a) a 2-km twin-lead cable (where signal propagation speed is 82% of the signal propagation speed in vacuum)?, and 
(b) a 40-km multimode fiber optic cable (signal propagation speed is 65% of the signal propagation speed in vacuum)?

Speed of light in vacuum $=299.792.458 \;m/s$
(a)
Speed of light in lead cable $299.792.458\cdot 0.82\approx 245.829\;km/s$
$\tau\approx \frac{2km}{245.829\;\text{km/s}}=8,13\mu$sec
Slot width is $2\tau=16,26\mu$sec
(b)
Speed of light in fiber optic cable $299.792.458\cdot 0.65\approx 194.865\;km/s$
$\tau\approx \frac{40km}{194.865\;\text{km/s}}=205\mu$sec
Slot width is $2\tau=410\mu$sec
# 4.12
Consider five wireless stations, A, B, C, D, and E. Station A can communicate with all other stations. B can communicate with A, C and E . C can communicate with A, B and D. D can communicate with A, C and E. E can communicate A, D and B.

(a) When A is sending to B, what other communications are possible? 
(b) When B is sending to A, what other communications are possible? 
(c) When B is sending to C, what other communications are possible?

(a) Since A can communicate with all, all other stations will hear A's messages. Signals wouldn't be able to be told apart so no other communications are possible.
(b) When be is sending, then C and E receive these signals as well. If D want's to send something to C or E, the signals would overlap with those coming from B AND A would also receive these signals and wouldn't be able to distinguish the messages from B. If C or E wanted to send something to D, then A would also be receiving these signals, so no other communications are possible.
(c) This is the same as in the previous case.
If fact when two stations are communicating with each other, then no other communications would be possible since the signals would overlap with each other.
# 4.18
Two CSMA/CD stations are each trying to transmit a frame. They both contend for the channel, using the binary exponential backoff algorithm after a collision. What is the probability that the contention ends on round k, and what is the mean number of rounds per contention period?

On round $k$ there are $2^{k-1}$ different slots to choose from.
The probability that both stations choose the same number on round $k$ is $\frac{1}{2^{k-1}}$. For example if a third collision occurs, the probability of this happening is 0.25.
This also means that the probability of both stations not choosing the same number and therefore ending on round $k$ is $1-\frac{1}{2^{k-1}}$
# 4.24
In Fig. 4-27, four stations, A, B, C, and D, are shown. Which of the last two stations do you think is closest to A and why?

D doesn't pick up the signals from A whilst C does so C is closer to A.
# 4.30
Fig. 4-28 shows different wait times in 802.11 for frames with different priorities. This approach prevents high-priority traffic, such as frames carrying real-time data, from getting stuck behind regular traffic. What is a disadvantage of this approach?

Frames of low priority might only be able to send their data after a long time, or maybe even never. In addition applications must first have to agree on whose data has a higher priority.
# 4.36
What is the maximum size of the data field for a 3-slot Bluetooth frame at basic rate? Explain your answer.

For a 1-slot frame there is a 126 bit frame header and a 240 bit data field. Between two slots there is an additional $260\mu$ sec delay (to allow the inexpensive radio circuits to become stable). For frames that use more slots, another 126 bits can be used for data, since they would be used for the frame header. For a 3-slot frame we then have the beginning 240 bits of the first original data field, plus two times 126+240 bits and the additional two times 260 bits (since at a speed of 1Mbps in $260\mu$ seconds we get 260 bits) that would be used as padding between frames:
$240+(240+126)*2+(260*2)=1492$ bits
The same can be done for 5-slot frames, that's why we get $240+(240+126)*4+(260*4)=2744$ bits
# 4.42
Consider the extended LAN connected using bridges B1 and B2 in Fig. 4-33(b). Suppose the hash tables in the two bridges are empty. Which of these data transmissions leads to a broadcast:  

(a) A sends a frame to C.
(b) B sends a frame to E.  
(c) C sends a frame to B.  
(d) G sends a frame to C.  
(e) E sends a frame to F .  
(f) D sends a packet to C.  
Assume that every frame is sent after the previous frame has been received.

Transmissions (a) and (b) are broadcast since we don't yet know where C and E are.
(c) is not broadcast since we know from (b) where B is.
(d) is broadcast since only B1 knows where C is, B2 doesn't know since C's transmission wasn't broadcast before.
(e) is broadcast since we don't yet know where F is.
(f) is broadcast since B2 still hasn't received a message that came from C
# 4.48
Store-and-forward switches have an advantage over cut-through switches with respect to damaged frames. Explain what it is.

Store-and-forward switches wait for the entire message to arrive, before sending it on. This means that at each switch the frame can be checked for errors. The message can be stopped to not waste bandwidth and the error can be reported back to the sender.

---
References: