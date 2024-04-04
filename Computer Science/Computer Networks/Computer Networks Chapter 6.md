---
Tags: 
Created: 2023-06-02 13:46:07
---
(Links:: [[Computer Networks]])
# 6.6
In the Internet File Server example (Fig. 6-6), can the connect( ) system call on the client fail for any reason other than listen queue being full on the server? Assume that the network is perfect.

Maybe the IP address given has a wrong format, doesn’t exist, or is a duplicate of an already existing entry (to connections on the same machine).
# 6.12
Imagine that a two-way handshake rather than a three-way handshake were used to set up connections. In other words, the third message was not required. Are deadlocks now possible? Give an example or show that none exist.

Deadlocks are possible. Deadlocks are however still possible, even with the three-way handshake. The three-way handshake is used to start a connection and prevent a connection from starting when duplicate messages arrive at a machine, and it does not help with preventing deadlocks. Other measures are put in place like using a timer so the machine doesn’t wait indefinitely.
# 6.18
Some other policies for fairness in congestion control are Additive Increase Additive Decrease (AIAD), Multiplicative Increase Additive Decrease (MIAD), and Multiplicative Increase Multiplicative Decrease (MIMD). Discuss these three policies in terms of convergence and stability.

AIAD would will only help with getting 100% of the bandwidth, since it moves at a 45% angle. MIAD is opposite to AIMD and will guide the bandwidth distribution away from the optimal point, making it the least fair. MIMD can just like AIAD only help guide the distribution towards 100% efficiency
# 6.24
Several RPC implementations provide an option to the client to use RPC implemented over UDP or RPC implemented over TCP. Under what conditions will a client prefer to use RPC over UDP and under what conditions will he prefer to use RPC over TCP?

If the programmer wanted the call to be abstracted from view and to work "out of the box" then TCP would be the best option, since it also provides a bit stream. Sometimes however the programmer needs more control like what to do if the procedure result doesn't arrive. Sometimes package loss is already accounted for on higher levels.
# 6.30
A process on host 1 has been assigned port p, and a process on host 2 has been assigned port q. Is it possible for there to be two or more TCP connections between these two ports at the same time?

This notion is called inverse multiplexing and is not supported by TCP since it uses only a single network endpoint. If you need inverse multiplexing, you can use SCTP.
# 6.36
Consider the effect of using slow start on a line with a 10 msec round-trip time and no congestion. The receive window is 24 KB and the maximum segment size is 2 KB. How long does it take before the first full window can be sent?

After sending packets out for 4 rounds (40 msec), 32KB is sent on round 5 and the receiver sends a message back with the congestion flag set (10 msec). The threshold is then set to 16KB, and using TCP tahoe it takes another 4 rounds (40 msec) to get back to 16KB packet sizes. It then switches to using additive increase for 24-16=8 rounds (80 msec). In total 170 msec pass.
# 6.42
In a network whose max segment is 128 bytes, max segment lifetime is 30 sec, and has 8-bit sequence numbers, what is the maximum data rate per connection?

Since each segment has 128 bytes and each segment is given a sequence number, we can send $128\times2^{8}=128\times256=32$ MiB with an 8-bit sequence number. They can't show up in the next 30 seconds which means the maximum data rate is $\frac{32 \text{ MiB}}{32\text{ sec}}=1\text{MiB/s}$
# 6.48 
For a 1-Gbps network operating over 4000 km, the delay is the limiting factor, not the bandwidth. Consider a MAN with the average source and destination 20 km apart. At what data rate does the round-trip delay due to the speed of light equal the transmission delay for a 1-KB packet?

If we assume that the data runs on a fiber optical cable, then the speed of light in this medium is roughly 200 000 km/s. The round-trip delay for a package is $2\times20km/200000\text{ km/s}=40km/200000\text{ km/s}=0,2\text{ msec}$
The data rate must be so fast that 1-KB is loaded onto the fiber in 0,2 msec: $\frac{1\text{ KB}}{0,2 \text{ msec}}=5\text{ MB/s}$

---
References: