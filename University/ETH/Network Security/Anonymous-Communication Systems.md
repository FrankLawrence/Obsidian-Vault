---
Tags: #lecture 
Created: 2026-01-14 01:27:03
---
(Links:: [[Network Security]])

Even with our traffic being *encrypted*, there is still a lot of **metadata** which can be gathered and may reveal identifying information. This makes metadata much more powerful than most thought it to be.

> [!question] Why could anonymity be desirable?
> - **Military applications**:
> 	- Covert intelligence gathering
> 	- Covert attacks
> 	- Penetration testing on own infrastructure
> 	- Undercover agents communicating out of a monitored country
> - **Trade, industrial R&D**
> 	- Detect price discrimination
> 	- Hide revealing patient searches in an untrusted database
> - **Anonymous reporting**  
> 	- Tips regarding criminal activity, but also accidents
> - **Human rights**
> 	- Free speech, whistleblowing, censorship avoidance
> - Building block for other technologies:  
> 	- Crypto-currencies ([[BitCoin]], Ethereum)
> 	- **Electronic Voting**

# Threat Model
- There are various types of adversaries that can be considered
- Degree of control: *local* or *global*
- Type of control: *network* or *compromised infrastructure*
	- Various combinations are possible
	- Assumption: the infrastructure is never *fully* compromised
- Type of behavior: *passive* or *active*
- Often not clearly specified -> unclear guarantees

> [!example] Harvard bomb threat
> - Student logged into Uni wifi sends bomb threat over [[TOR]]
> - University can see which individuals were connected to TOR network at time of message
> 
> -> **You cannot be anonymous on your own**

# Basics of anonymous communication
- Wireless communication *broadcasts* data
	- guarantees (not always) receiver anonymity
	- Sender can be de-anonymized through triangulation
	- Sender can hijack connection by using 'burner phone', or masking their sender ID
- Using *proxy* or *VPN* for anonymization
	- Use *layered encryption* to hide content from proxy; e.g. TLS on VPN
	> [!danger] Problem
	> Proxy can see (and record) metadata -> single point of failure if compromised
	- Using *multiple proxies* mitigates attack as long as *at least one honest proxy* is traversed
	![[Onion Routing.svg|600]]
	- Messages and forwarding information is encrypted multiple times (onion) -> *all keys* necessary to decrypt

# Mix-Nets
- Intended for sending anonymous emails (or other individual messages)
	- Latency is not a big concern
	- No connection setup, only individual messages
- Build on asymmetric cryptography
- Each mix has a public/private key pair $K_{i} / K_{i}^{-1}$
- Public keys $K_{i}$ and addresses $I_{i}$ are known to the sender
- **Sending messages**:
	- Alice ($A$) sends $M$ to Bob ($B$) through mixes $1$,$2$, and $3$
	- Bob does not know who the message cam from

$$A \to 1: \biggl\{{\color{orange}I_{2}}, \Bigl\{ {\color{orange}I_{3}}, \bigl\{ {\color{#58F}I_{B}}, \{ M \}_{K_{B}} \bigr\}_{K_{3}} \Bigr\}_{K_{2}} \biggr\}_{K_{1}}$$
$$1 \to 2: \Bigl\{ {\color{orange}I_{3}}, \bigl\{ {\color{#58F}I_{B}}, \{ M \}_{K_{B}} \bigr\}_{K_{3}} \Bigr\}_{K_{2}}$$
$$2 \to 3: \bigl\{ {\color{#58F}I_{B}}, \{ M \}_{K_{B}} \bigr\}_{K_{3}}$$
$$3 \to B: \{ M \}_{K_{B}}$$

> [!danger]+ Problem
> Messages have varying lengths which may show at which point the packet is from the mix identifiers $I_{n}$ -> Original paper did not include mix addresses, but uses a fixed cascade of mixes

> [!danger]+ Problem
> Last hop can identify who the message is for -> Paper suggests leaving away receiver address
> The last mix would *broadcast* the message, and only the intended receiver can decrypt it using their private key

> [!warning] Network attacker can observe in- and outgoing messages

- Each proxy should perform *batching*: Collect several messages before forwarding (threshold) -> prevents mapping of tracked packets
- Additionally, the proxies should change the order of (*mix*) the messages -> *threshold mix*
- **Important**: messages to be padded to a *fixed length* to make them indistinguishable 

> [!warning] Intersection attack [^1]
> - Often, users only communicate with a small subset of other users
> - Idea: every time a message is seen by the target, register the sets of destinations
> - Using statistical analysis gives higher confidence in the communication partners [^2]

- proxies may also *create dummy traffic* (*cover traffic*) to prevent statistical disclosure
	- Both for sending and for receiving

## Sending a response
- Idea: $A$ includes an *untraceable return address* in its message to $B$:
	- Generate fresh public/private key pair $K_{A}/K_{A}^{-1}$
	- Generate fresh, random, *symmetric* keys $R_{i}$ for all intermediate mixes
- Return address for mixes 1, 2, and 3 (in forward direction): $$\Bigl\{R_{3},I_{2}, \bigl\{ R_{2}, I_{1}, \{  R_{1},I_{A}, \}_{K_{1}} \bigr\}_{K_{2}} \Bigr\}_{K_{3}}, K_{A}$$
$$B \to 3: \{M\}_{K_{A}}, \Bigl\{ R_{3}, I_{2}, \bigl\{ R_{2}, I_{1}, \{ R_{1},I_{A} \}_{K_{1}} \bigr\}_{K_{2}} \Bigr\}_{K_{3}}$$
$$3 \to 2: \bigl\{\{M\}_{K_{A}} \bigr\}_{R_{3}}, \bigl\{ R_{2}, I_{1}, \{ R_{1},I_{A} \}_{K_{1}} \bigr\}_{K_{2}}$$
$$2 \to 1: \Bigl\{ \bigl\{ \{M\}_{K_{A}} \bigr\}_{R_{3}} \Bigr\}_{R_{2}}, \{ R_{1},I_{A} \}_{K_{1}}$$
$$1 \to A: \biggl\{ \Bigl\{ \bigl\{ \{M\}_{K_{A}} \bigr\}_{R_{3}} \Bigr\}_{R_{2}} \biggr\}_{R_{1}}$$

# Circuit-based anonymity networks (onion routing)
- Problems of mix-nets: high-latency due to batching and mixing; overhead due to asymmetric cryptography
- Solution: *Layered encryption*, no batching and mixing, no cover traffic
- Flow-based circuits use the *same symmetric key* for all packets per flow
- constraining the threat model to only *local adversaries* which cannot launch confirmation attacks simplifies protocol

___
- Circuit-based anonymous communication systems (aka. **Onion Routing Systems**)
- Nodes are called *relays* (nodes or routers)
- Virtual circuit is called *tunnel*
- Popular implementation: [[TOR]]

## Life-cycle of a circuit
- *Circuit setup*
	- Initially, sender knows long-term public keys of relays
	- The sender negotiates shared keys with all relays on the path; this requires (expensive) *asymmetric cryptography*
	- The relays store the necessary state
- *Data forwarding*
	- Packets for one or more flows are forwarded along the circuit
	- Only *symmetric cryptography* is used ([[AES]])
- *Circuit tear-down*
	- The circuit is destroyed to free state on relays or to prevent attacks

### Data forwarding
- The sender has established a circuit (symmetric keys and *per-link IDs*)
- A data packet is encrypted as usual (layered encryption)
- The ID of the next relay is added in clear text
	- To protect against network adversaries, links can be encrypted ([[Transport Layer Security|TLS]]) 
![[Onion Routing.svg|600]]
### Direct Circuit Setup
- Establish state on relays by using a normal packet as for mixes
	- Message for each node contains address of next node and ephemeral [[Diffie-Hellman Key Exchange|Diffie-Hellman share]]
	- Each node replies with its own ephemeral Diffie-Hellman share
	- Encryption of setup packet uses long-term *asymmetric* keys of relays
- Relatively fast (though relays need to perform asymmetric crypto)

> [!definition] Forward security
> If long-term keys are compromised, anonymity of previously established circuits is preserved

- The direct setup does *not* provide (immediate) *forward security for link between communication partners*
	- No ephemeral information can be used to encrypt setup message
		- Need to use long-term public key of each node for encryption
		- Similar to 0-RTT data in [[Transport Layer Security|TLS]]
- Forward security for later packets is achieved through Diffie-Hellman exchanges

> [!danger] De-anonymization
> - Assumption: the adversary can record all network traffic and compromise individual relays
> - Once a relay is compromised, the adversary can check which incoming packet corresponds to the target circuit and identify next relay
> - To prevent this (with direct setup): change public keys of the relays
> 	- This is called *eventual forward security* -> significant overhead!

To prevent such an attack, we can employ **telescopic circuit setup**:
- Keys are negotiated one relay at a time
- The circuit is "extended" by one hop at a time
	- Ephemeral session keys are negotiated before the circuit is extended
- This setup is slower, but it offers *immediate forward security*:
	- As soon as the circuit is closed, the session keys are deleted
		
### Circuit tear-down
- Can be initiated both by sender and by intermediate relays  
	- The sender communicates the tear-down to one relay at a time, starting from the furthest away
	- The exit relay may tear down the circuit if a corrupt packet is detected, or some other attack
- Circuits have a limited lifetime, so they will eventually be destroyed

## Comparison of mix-nets and onion routing
|                     | Mix-net       | Onion Routing         |
| ------------------- | ------------- | --------------------- |
| Forwarding          | Message-based | Circuit-based         |
| Layered encryption  | asymmetric    | symmetric             |
| Mixing and batching | yes           | no                    |
| Cover traffic       | optional      | no                    |
| Forward security    | no            | with telescopic setup |
| Latency             | High          | Low/medium            |

## Attacks on circuit-based anonymous-communication systems
- Several attacks have been proposed against these systems
- For many it is unclear whether they fit the stated threat models
	- Some of them are practical, requiring limited resources
	- Others are only achievable by state-level adversaries (Five Eyes)
- [[#Traffic analysis]] attacks (confirmation attacks): flow fingerprinting, website fingerprinting
- [[#Higher-layer attacks]]: stack fingerprinting
### Traffic analysis
- **Passive traffic analysis**:
	- The adversary observes the edges of the network, recording traffic patterns (flow length, bandwidth pattern, inter-packet timings)
	- Real-time detection is challenging -> Alternative to store and compare later
- **Active traffic analysis**: 
	- The adversary actively modifies packet timings
		- Inter-packet timings (delaying/reordering packets)
		- Packet drops also possible but detectable
	- *Flow watermarking*: inject one bit of information (marked or not)
	- *Flow fingerprinting*: inject multiple bits (e.g. sender IP address!)
- **Traffic analysis resistance**:
	- There have been proposals to incorporate cover traffic and mixing -> much overhead and scalability becomes an issue
	- Only suitable for few applications (VoIP) with low bandwidth [^3]
### Higher-layer attacks
- OS network stack fingerprinting
	- Compromised adversary can probe TCP stack
	- *Solution*: per-hop TCP
	- Still, TLS or HTTP layer may be [identifiable](https://amiunique.org/fingerprint)!
- Most de-anonymization is still done through other means:
	- Trick user into downloading malware
	- Trick user into downloading file that will access the internet directly
	- Analyze user behavior like texts
- To achieve anonymity, all layers need to be anonymized:
	- Any gap will break anonymity
# Summary
- You cannot be anonymous on your own -> *anonymity set*
- Multiple relays and layered encryption enable anonymous communication
- Two main types of anonymous-communication systems:
	- *Mix-nets*: slow, strong guarantees
	- *Circuit-based (onion-routing) systems*: low latency, possible attacks for strong adversary
	- [[TOR]] is the most widely used onion-routing system
	- High-speed anonymity systems on next-generation Internet architectures
- Anonymous communication is a tool that can be used for both good and bad purposes


[^1]: Kesdogan et al., Limits of anonymity in open environments, IH, 2002

[^2]: Danezis and Serjantov, Statistical disclosure or intersection attacks on anonymity systems, IH, 2005

[^3]: Le Blond et al., Herd: a scalable, traffic analysis resistant anonymity network for VoIP systems, ACM SIGCOMM, 2015
