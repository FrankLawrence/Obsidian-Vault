---
Tags: exercise
Created: 2026-01-21 18:37:03
---
(Links:: [[Network Security]])
# Exercise sheet 04: Denial of Service Part 1 (30 points)

### Question 1
**DDoS: Main Attack Types.**

**1.1.** (3 points)
> [!question] Name the three main types of DDoS attacks. What part of the network
> infrastructure do each one attack, and in what units do we measure their
> severity?

_Solution_:
1. Volumetric Attacks: Consume the bandwidth in a link, either within
the target network/service, or between the target network/service and
the rest of the Internet. (Units: bits/sec)
2. Protocol Attacks: Designed to exhaust resources available on the
target or on a specific device between the target and the Internet.
Protocol DDoS attacks need not consume all of the target's available
bandwidth to make it inaccessble.
(Units: packets/sec)
3. Application-Layer Attacks: Target various aspects of an application
or service at Layer 7. These are the most sophisticated and stealthy
attacks as they can be very effective with as few as one attacking
machine generating traffic at a low rate (Units: requests/sec)

**1.2.** (2 points)
> [!question] Name some representative attack vectors for each of the 3 types of DDoS.

_Solution_:
1. Volumetric Attacks: ICMP/UDP packet floods, Reflection/Amplification, etc.
2. Protocol Attacks: SYN/ACK floods, RST attacks, TCP connection floods, etc.
3. Application-Layer Attacks: Application request floods, Slowloris, etc.

**1.3.** (2 points)
> [!question] What kind of attacks are the most difficult to detect? Why do you think so?

_Solution_:
Application-layer attacks are often considered more sophisticated and challenging 
to mitigate compared to volumetric or protocol attacks. The reasons are as follows: 
- Application-layer attacks can be more difficult to detect because they mimic 
legitimate traffic and often target specific weaknesses in applications. 
This makes them potentially more dangerous as they can go unnoticed for a longer time.
- Application-layer attacks focus on exhausting specific resources within the application or service. 
This approach can be highly effective in disrupting targeted services.
- Application-layer attacks may not require massive amounts of traffic to cause 
disruption. Even a relatively low rate of malicious requests can impact 
the targeted application or service.
- Mitigating application-layer attacks requires more sophisticated 
techniques, such as behavior analysis and traffic profiling. This complexity can make it 
challenging for defenders to quickly identify and block malicious traffic.

### Question 2 (4 points)
**Volumetric Attacks: Shrew Attack**

The lectures briefly mentioned Shrew Attacks as a potential attack vector
for volumetric DDoS attacks. Let's learn about it in a little bit more
depth.

Shrew Attacks can cause TCP traffic on a link to degrade significantly,
potentially even down to zero. But unlike typical attacks that try to fill
the bandwidth by sending high-rate attack traffic (and is thus easily
detected), shrew attacks cleverly sends packets at sufficiently low
average rate to elude detection by counter-DoS measures.

This information is sufficient to solve the questions below, but for those
who are curious, the paper below explains the mechanism in depth (and
may make it easier to answer questions):

[1] <https://dl.acm.org/doi/pdf/10.1145/863955.863966>

**2.1.** (2 points)
> [!question] Shrew attacks cleverly abuse TCP's resending mechanism to perform a
> DoS attack while maintaining low amounts of __average__ attack traffic.
> Can you think of how they might do this?

_Solution_:
By sending short bursts of data at a high rate, an attacker can deliberately prompt a TCP flow to consistently 
enter retransmission timeout states. These bursts are designed to align with the Round-Trip Time (RTT) scale, 
and the attacker periodically repeats them at a slower pace, synchronized with Retransmission Timeout (RTO) timescales. 
The objective is to significantly diminish the victim's throughput, bringing it close to zero. 
The attacker keeps the average transmission rate low, making it difficult for counter Denial-of-Service 
(DoS) mechanisms to efficiently identify and counteract the attack.

**2.2.** (2 points)
> [!question] The diagram in the figure below provides parameters `l`, `R`, and `T`.
> Can you describe how an attacker would choose these values, with respect
> to the Round Trip Time of the link, the victim's Retransmission Timeout,
> and the link's bandwidth capacity?
> 
> <img src="assets/Square-wave_DoS_stream.png" alt="image" />

_Solution_:
The attacker employs a deterministic on-off pattern, transmitting bursts with a duration (l) and rate (R) 
in a periodic fashion with a set period (T). For a shrew attack to be successful, the rate (R) must be sufficiently high 
to cause loss—meaning the aggregated rate (considering existing traffic) exceeds the link capacity. 
The burst duration (l) should be of the order of Round-Trip Time (RTT), striking a balance between 
inducing timeouts and avoiding detection. Additionally, the period (T) should be on the scale of Retransmission Timeout (RTO), 
strategically chosen to subject flows attempting to exit timeout to successive loss events.

### Question 3 (12 points)
**DDoS: DNS.**

DDoS attacks on links are always very tricky to mitigate, and local
countermeasures are futile. This is because regardless of what you do with
the high amounts of incoming traffic (drop them, handle them, etc), your
outgoing connection to the internet is often also unavailable because you
use the same exhausted link there.

In this question we'll look at a specific type of attack that causes such
scenario: DNS reflection attacks.

On March 18, 2013 an anti-spam-mail organization, Spamhaus, experienced a
DDoS of bandwidth unheard of before then – 75 Gbps[1]. Skim through the
article below (especially the paragraph on "How to Generate a 75Gbps DDoS")
and answer the questions below:

<https://blog.cloudflare.com/the-ddos-that-knocked-spamhaus-offline-and-ho/>

[1] Note that more modern DDoS have reached several Tbps.

**3.1.** (1 points)
> [!question] How does a DNS reflection attack work?

_Solution_:
An attacker sends a request to a DNS server with spoofed IP address. The
DNS will reply to the machine that actually has the spoofed IP address,
which will not be expecting traffic coming to it.

**3.2.** (2 points)
> [!question] Can you give an example of a DNS query which would trigger a big response?

_Solution_:
Same as before, but using a request that triggers a much larger response.
Examples of such queries are the ANY queries, which return all the DNS records
of a domain, including the lengthy SOA records. (`dig @X.X.X.X ANY ripe.net
+edns=0 +bufsize=4096`)

Note that most DNS recursive resolver and authoritative servers will nowadays
refuse to respond ANY queries, or will greatly limit the size of their
responses, in order to limit the amplification factor.

**3.3.** (1 points)
> [!question] In this case attackers probably used a request that is \~30 bytes long,
> which resulted in a \~3,000 bytes response. What is the amplification
> factor?

_Solution_:
```math
amp\_fac=\frac{3000b}{30b} = 100
```

**3.4.** (3 points)
> [!question] Write down your local internet's speed, measured with https://fast.com/.
> (1 point)
> 
> Using the amplification factor calculated above, calculate the scale of
> DDoS attack (in Gbps) that you can cause on a victim DNS server.
> (2 points)

_Solution_:
```math
attack\_mbps = local\_mbps \times 100
attack\_gbps = \frac{attack\_mbps}{1000}
```

**3.5.** (3 points)
> [!question] DNS (mainly) uses UDP port 53.
> 
> Can you still perform a reflection attack with a TCP-based protocol? If
> you want, you can assume a Dolev-Yao attacker who can also
> intercept packets on the links between open DNS servers and the victim.

_Solution_:
UDP is a fire-and-forget protocol, with no added overhead. So, the DNS
server just replies to the spoofed IP address with no further checks.

On TCP the reflection attack would not work as-is, as the data sent by
the server towards the victim would be the SYN/ACK message in the TCP
handshake.

However, there are two ways you can still make it work:
1. You could make it an ACK reflection attack, which the folks attacking
   Spamhaus also did. This works just by sending a lot of SYNs to
   various servers, with the source IP spoofed as the victim. The result
   is a lot of SYNACKs sent to the victim. But unlike DNS reflection,
   the amplification factor is low. It's also easy to block by checking
   if ACKs are matching sent SYNs.
2. Assuming Dolev-Yao, the attacker could intercept the SYNACK, drop it,
   and reply with the ACK + DNS request, which would cause the amplified
   DNS response to be sent to the victim similar to the UDP attack.

**3.6.** (2 points)
> [!question] What is an open recursive DNS resolver? How can you configure it to
> mitigate DNS reflection & amplification attacks?

_Solution_:
An open DNS recursive resolver replies to any DNS query, coming from any
device on the Internet. This can be a problem, given the amplification
factor of a DNS query. A solution would be to rate-limit resolved
requests by IP, so that no machine would be overwhelmed with data. But
usually attacks do not use one single server. Another solution would be
to limit the IP subnets that can access the server to the ISPs known to
prevent source address spoofing.

### Question 4 (7 points)
Consider an attacker that wants to perform a DDoS attack on a victim server.
The attacker has created a botnet with 10,000,000 compromised IoT devices but
is _unable to spoof source addresses_.

**4.1.** (2 points)
> [!question] The attacker uses the botnet devices to directly flood the victim server.
> If each IoT bot device simply sends 10 TCP SYN packets per second
> towards the victim, what is the approximate aggregate bandwidth that
> reaches the victim? Reference the TCP header size, IPv4/6 header size,
> and give your answer in the unit of bits per second.

_Solution_:
TCP has a minimum header size of 20 bytes, IPv4 a minimum of 20 bytes and
IPv6 a minimum of 40 bytes. Thus we have

$$
10^1 × 10^7 × 8 × 40 = 32 × 10^9 bps
$$

for IPv4, or $(48 × 10^9 bps)$ for IPv6.

**4.2.** (3 points)
> [!question] Consider a scenario where the victim server uses an address filtering approach.
> Botnet nodes are blacklisted (based on their pattern of connection requests),
> and the associated packets are rejected.
> 
> Clearly, the advantage is that malicious SYN requests would be dropped.
> 
> But can you think of disadvantages or outstanding issues with this approach?

_Solution_:
If the attack is exhausting the link, then the regular packets still cannot
reach the server.

Also, given the IoT context, there are probably many NATed devices.
Blacklisting based on addresses can lead to collateral damage to benign
devices.

**4.3.** (2 points)
> [!question] Now assume that botnets are capable of spoofing addresses. Consider a
> service which publishes a list of misbehaving hosts, categorized as
> those performing 20 consecutive SYN requests to one of the servers in
> the network controlled by the service. Any server may periodically
> download the blacklist and refuse connections by listed hosts. Describe
> any two disadvantages of this approach.

_Solution_:
Disadvantages include:
1. The list service can be DDoSed
2. An attacker knowing the scheme can avoid being placed on the list
3. Change of IP address after being blacklisted
4. Framing of legitimate users by spoofing their addresses.


---
References: