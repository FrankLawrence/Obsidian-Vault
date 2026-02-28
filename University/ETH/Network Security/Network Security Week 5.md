---
Tags: exercise
Created: 2026-01-21 20:56:17
---
(Links:: [[Network Security]])
# Exercise sheet 05: Denial of Service Part 2 & Crypto Refresher (37 points)

### Question 1 (10 points)
Remotely Triggered Black Hole Filtering (**RTBH** in short) is a generic
technique that can be used to mitigate volumetric DoS attacks – the
offending traffic is simply dropped (black-holed) at the border routers
of an autonomous system (AS). RTBH comes in two flavours, source-based
and target-based.

This information is enough to solve the exercise questions below, but for
those who are curious, below is a link to the white paper.

[1]: (assets/RTBHF_whitepaper.pdf)

**1.1.** (4 points)
In source-based RTBH, all the traffic from the attacker’s subnets is
dropped by the target’s ISP. (Note for clarification that the red
"dropped" icon in the diagram is labelled on the arrows going from PEs
to the Target).

PEs stand for Provider Edges (essentially just edge routers), which are
routers at the edge of ISP networks that connect to external
users/customers.

![Source-based RTBH<sup>1</sup>](assets/source_based.png)

**1.1.1.** (2 points)
> [!question] Are the ISP network (incl. links) and the target device protected from
> DoS? Why or why not?

_Solution_:
In this case, both the network and the target are protected from the
DoS. The ISP absorbs all of the attack at its border routers. If the
attack is particularly large, there could be problems at the upstream
provider of our ISP.

**1.1.2.** (2 points)
> [!question] Why is this technique hard to implement, in the case of a DDoS attack?
> Can you imagine which unintended consequences it may have?

_Solution_:
During a DDoS attack, the distributed nature makes it difficult for
ISPs to create a complete list of malicious source IPs. This means
that source-based RTBH can still let in attack traffic (false negative).
Moreover, any benign flows coming from the same subnets where infected
machines reside are often dropped as collateral damage (false positives).
If an amplification attack was used, there would be lots more false
positives as well, where legitimate requests get dropped.

**1.2.** (6 points)
In destination-based RTBH, all the traffic to the target’s subnets is
dropped by the target’s ISP. This is a quicker solution than
source-based RTBH, but has its own problems.

![Destination-based RTBH<sup>1</sup>](assets/dest_based.png)

**1.2.1.** (2 points)
> [!question] What do you think are the problems with source-based RTBH? Would you
> say that the identified problem undermines its effectiveness as a DoS
> mitigation strategy?

_Solution_:
It doesn’t solve any problem for the target host. That machine will be
effectively incapable of receiving any traffic, because the ISP is
dropping it. As a result, the attacker will have practically succeeded
in its intent to DoS.

On the other hand, if the target host has itself a network to protect,
it gains the same benefits as the ISP by sacrificing just one device
under attack.

**1.2.2.** (3 points)
> [!question] Let us now delve into the technical details of this solution. In our
> setup, the trigger router (manually configured by the admin) "installs"
> some configuration to the edge routers (PEs). How does this
> configuration work, and what does it change on the edge routers
> (explain to as much depth as you can)?
> 
> Although the diagram says "BGP updates" for the black arrow, you can
> assume iBGP (e.g. OSPF) is used within the ISP infrastructure.

_Solution_:
The ideal place to drop the traffic is indeed the border routers, so
that the traffic does not even enter the AS.

The traffic is 'sinked' by adding a null route in the forwarding table
(also known as FIB, forwarding information base):
they receive an iBGP update which sets the 'next hop' for the target
IP to the IP address of a blackhole interface.

In particular, let's assume `192.0.2.1` is the IP of the `null0`
blackhole interface (the subnet `192.0.2.0/24` is reserved for testing
purposes), and the target of the DDoS attack is `1.1.1.1`.
The border router would receive an iBGP update telling it to use
`192.0.2.1` as a next hop for `1.1.1.1`, and therefore it would discard
the traffict by sending it to the `null0` interface.

**1.2.3.** (1 points)
> [!question] Normally, whenever a packet is dropped, an ICMP message would be sent
> back to inform the sender. Is this behaviour desirable here? Why?

_Solution_:
No, it is not desirable. The ICMP messages would just increase the
volume of traffic on the attack path. Moreover, we do not want the
attacker to know which DoS countermeasures are in place.

### Question 2 (12 points)
(D)DoS attacks work by exhausting critical resources. For each attack:
1. identify the targeted critical resource;
2. discuss how the attack prevents the normal operation;
3. propose a mitigation.

**2.1.** (3 points)
> [!question] [Slowloris attack](https://www.incapsula.com/ddos/attack-glossary/slowloris.html)

_Solution_:
Every web server has a limit on the number of HTTP connections that can
be open at the same time. Slowloris monopolizes them, preventing the
server from accepting any new ones. This can be partially fixed by
limiting the number of connections from the same IP, increasing the
maximum number of connections, or detecting the attack and killing a
connection (eg. slow transfer speeds).

**2.2.** (3 points)
> [!question] Flooding the embedded devices that use public key cryptography for
> message authentication with random messages.

_Solution_:
Asymmetric cryptography is slow and consumes a lot of power. This attack
can either take all of CPU time on the device, preventing it from
processing legitimate requests, or drain the battery, forcing it to shut
down. If possible, the device should rely on symmetric cryptography. You
could also use rate limiting to reduce the number of requests coming
from the same source, but that doesn’t solve the problem completely.
Another approach is to limit the number of messages for which the costly
operations have to be performed. This could for example mean filtering
incoming messages based on estimated message origin or message
structure.

**2.3.** (3 points)
> [!question] Sending packets crafted specifically to trigger the worst-case scenario in
> a hash-map.

_Solution_:
The worst-case scenario in a hash-map is mapping all N objects to a
single index. Searching this hash-map takes O(N) times, instead of
expected O(1). This takes a lot of CPU time, prolonging the time needed
to serve a request, and causing the to pile up in the queue. The
solution would be to use a data-structure that is resistent to an attack
like this, while still providing sublinear search times (eg. trees).

**2.4.** (3 points)
> [!question] [TCP SYN flood attack](https://www.incapsula.com/ddos/attack-glossary/syn-flood.html): sending many TCP SYN requests to the victim
> server.

_Solution_:
Every time a TCP handshake is initiated, the receiving machine needs to
store the data about it in kernel memory. In order to prevent memory
exhaustion, the number of half-open TCP ports is limited. TCP SYN flood
tries to keep as many TCP ports as possible in the half-open state, thus
forcing the server to reject new connections. This is done by sending as
many TCP SYN packets as possible (hence the name SYN FLOOD). We can
protect against the SYN flood by not keeping the state on our machine,
until the other side proves that they are reachable. This can be
accomplished by [SYN Cookies](https://cr.yp.to/syncookies.html)
which encode the data on the connection.

## Crypto refresher

### Question 1 (8 points)
On Thursday 16.10 you received a refresher lecture on cryptography. The following are some questions to help test your knowledge.
Concisely answer the following questions:

**1.1.** (2 points)
> [!question] Edward wants to prove to Laura he really is the sender of a message. What security property(s) is he trying to achieve?  Which cryptographic primitive could he use?

_Solution_:
(Data Origin) Authentication - they could use digital signatures or HMACs.

**1.2.** (2 points)
> [!question] Edward wants to send a secret message to Glenn---they cannot meet to exchange a key, but they deem it unlikely for the NSA to tamper with their messages on the fly ([1]). What security property(s) are Edward and Glenn trying to achieve? Which cryptographic primitives could they use?
> 
> [1]: Not historically accurate

_Solution_:
Secrecy, Confidentiality - they could use asymmetric encryption.

**1.3.** (2 points)
> [!question] Edward wants to store records of his job assignments at NSA on his NAS. Which property(s) ensures that nobody will alter the record before it reaches the NAS? Can you name a network protocol which provides this guarantee? (assume a fair use of the protocol)

_Solution_:
Integrity. e.g. BGPsec / TLS (level 4) / IPsec (level 3).

**1.4.** (2 points)
> [!question] Chelsea wants to share some documents with a journalist, without risking to be identified.  What security property is she trying to achieve?  Can you name a technology that could, in principle, protect her?

_Solution_:
Anonymity. The TOR network could help Chelsea, but she needs to be careful---correlation and other statistical attacks could expose her even if she is using an up-to-date TOR client.

### Question 2 (2 points)
> [!question] Challenge question (we did not reach this part during the crypto refresher session. If you are interested, we encourage you to read a little bit about 'Merkle Trees', as they might appear later in the course):
> Given a Merkle Hash Tree with n leaves, how many nodes need to be recomputed if a leaf is updated? (Assume a binary balanced fully populated tree).

_Solution_:

You can imagine a binary-like structure, where its parents get recomputed
when a single leaf is changed, recursively traversing up until the root node
gets updated. Thus, the number of nodes that need to be changed is the depth of
such tree:

`log_2(n)`

### Question 3 (2 points)
> [!question] You are using AES in ECB mode to encrypt an uncompressed high-resolution image. What could go wrong?

_Solution_:
![image](assets/exctuxes.jpg)

[More details here](https://blog.filippo.io/the-ecb-penguin/)

ECB mode will encrypt the same plaintext block to the same ciphertext. AES block size is 128 bits, which is just 16 bytes. Some features of the plaintext will still be visible in the ciphertext---in the case of an image, edges and large uniformly colored areas will be particularly noticeable.

### Question 4 (3 points)
Challenge Question:
You are writing a network protocol to control your server remotely---you decide to simply use AES in CBC mode to encrypt communication.
Unfortunately, CBC mode is susceptible to replay attacks and command manipulation, since modifying the ciphertext changes the plaintext in predictable ways.
You decide to add a MAC to prevent manipulation. Specifically, you add a MAC to all commands, and pad the combination of plaintext+MAC to a proper multiple of the block size before encrypting them. You might be using a padding scheme like PKCS7.
Finally, You also modify the server to now report an error if the padding is invalid, and a different error if the padding was valid but the MAC verification failed.

**4.1.** (3 points)
> [!question] What could still go wrong?

_Solution_:
Without adding a counter or something similar, the threat of a replay attack persists. The attacker could mount a _padding oracle attack_ (https://en.wikipedia.org/wiki/Padding_oracle_attack) against the server. In short: the attacker can distinguish between an invalid padding error (no reply) and the MAC error, and can use this to brute-force the plaintext byte by byte.

For example, let's again assume that there is a single ciphertext block. The attacker can brute-force the last byte of the plaintext in a block $`pt_{15}`$, manipulating the last byte of the IV, $`iv'_{15} = iv_{15} \oplus n_{15}`$, until she obtains a valid padding. Since

```math
pt_{15} = dec(ct)_{15} \oplus iv_{15}
```
```math
pt'_{15} = dec(ct)_{15} \oplus (iv_{15} \oplus n_{15})
```

the PKCS7 ([RFC 2315](https://tools.ietf.org/html/rfc2315)) padding will be valid for $`n_{15} = 0, pt'_{15} = pt_{15}`$ and for $`n_{15} \ne 0, pt'_{15} = PKCS7(1) = \mathtt{0x01}`$. It follows from the latter that:

```math
pt_{15} = dec(ct)_{15} \oplus iv_{15} = pt'_{15} \oplus n_{15} = PKCS(1) \oplus n_{15} = \mathtt{0x01} \oplus n_{15}
```

At most 255 (0x00 is valid as the original message is assumed to be valid) tries are necessary to brute-force the last byte $`n_{15}`$.
We learn the last plaintext byte and thus directly all the other padding bytes (again assuming a valid original message).
Each of the other bytes can be found after at most 256 tries.

To read more about padding oracle attacks, see for example [this blog
post](https://blog.skullsecurity.org/2013/padding-oracle-attacks-in-depth) by
Ron Bowes.


---
References: