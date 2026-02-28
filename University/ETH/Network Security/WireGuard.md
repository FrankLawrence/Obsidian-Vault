Due to the many lines of code that the IPsec source code consists of, there was an increasing interest in creating a much simpler, smaller alternative.
# Design Concepts
- No cryptographic agility ("cryptographically opinionated")
	- Only use state-of-the-art primitives
	- Simplify negotiation and remove insecure primitives
	> [!question]- What happens if cryptographic primitives are found to be vulnerable
	> Update both the client and server to new versions with a fix
- Very simple configuration - similar to "authorized_keys" file in SSH
- Very small codebase -> minimal attack surface, formally verifiable

> Can I just once again state my love for [WireGuard] and hope it gets merged soon? Maybe the code isn't perfect, but I've skimmed it, and compared to the horrors that are OpenVPN and IPSec, it's a work of art.
> ~ Linus Torvalds [^1]
# Authentication and Keys
- Handshake follows the [Noise Protocol Framework](https://noiseprotocol.org)
	- Built exclusively on (elliptic-curve) Diffie-Hellman exchanges
- Each peer has a *static* key pair 
	- Initiator: $S_{I}, S_{I}^{\text{pub}}$; responder: $S_{R}, S_{R}^{\text{pub}}$
	- Peers specify in configuration which public keys are authorized and (are allowed to) use with addresses
	- Similar to adding SSH keys to the "authorized_keys" file
- Each peer creates *ephemeral* key pair: $E_{I}, E_{I}^{\text{pub}}$ (initiator); $E_{R}, E_{R}^{\text{pub}}$ (responder)
- Derive symmetric keys from four (elliptic-curve) Diffie-Hellman combinations: $\big\{DH(S_{I}, S_{R}), DH(S_{I}, E_{R}), DH(E_{I}, S_{R}), DH(E_{I}, E_{R})\big\}$

==TODO: 1-RTT handshake diagram==

One great aspect of WireGuard, is that it *does not store state before authentication* and *does not send responses to unauthenticated packets*
-> Invisible to attackers/scanners
-> Prevent state-exhaustion attacks
Initial messages contain timestamps to prevent replay attacks, and responders keep track of previously received timestamps per peer and discard messages with smaller timestamp.

> [!danger] After receiving an initial message, the responder needs to perform a Diffie-Hellman computation
> Diffie-Hellman operations are computationally expensive, so an attacker could just send large amounts of messages from spoofed IP address to overload the responder.
> **Solution**: Check authenticity of first message purely with *symmetric cryptography*

[[DDoS Attack]] Mitigation:
- Standard cookie mechanism (similar mechanisms in [[IPsec#Internet key exchange (IKEv2)|IKE]] and DTLS)
	- Responder sends a cryptographic cookie to the initiator (when under load): `cookie = MAC(R.periodically_changing_secret, I.ip_address)`
	- The initiator retries sending the initial message and includes a MAC using the cookie as key (-> field 'mac2' in first WG message): `msg.mac2 = MAC(I.last_received_cookie, msg[0:offsetof(msg.mac2)])`
		- Ensures the initiator can receive packets (not spoofing address), but only after second message! -> doesn't really help with DDoS using first message
- Additional 'mac1' in initial message using the has of $S_{R}^{\text{pub}}$ as the key: `msg.mac1 = MAC(HASH('mac1----' || R.static_public), msg_before[0:offsetof(msg.mac1)])
	- Responder can check that the initiator at least know who they are talking to -> **initiator must know public key and IP address of responder for associated key**

Because network administrators can see when packets are routed through a VPN, people have devised obfuscation techniques, because VPN traffic may be blocked:
- Generic TLS wrapper: stunnel
- UDP obfuscators: udp2raw and phantum
- Automated censorship evasion: Geneva
# Setting up WireGuard between two peers

[^1]: 0. https://lists.openwall.net/netdev/2018/08/02/124]
