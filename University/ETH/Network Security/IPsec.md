- very large and complicated protocol, with many options for cipher suites, authentication mechanisms, etc.
- IPsec is broken down into two stages
	- Setting up Security Association (a session) via IKE
	- Encapsulating packets and tunneling them between SA endpoints
# Internet key exchange (IKEv2)

![[Internet Key Exchange.svg|900]]

> [!question]- What is (not) guaranteed after the first 2 messages have been exchanged?
> We can setup an encrypted but unauthenticated channel (anonymous responder; [[MITM]]), and also check for tampering (integrity).

> [!question]- Why would we want to first setup encryption and then authentication?
> For a flexible protocol, you'd want to be able to choose a different type of authentication mechanism/protocol.

> [!question]- Is forward secrecy guaranteed after the exchange?
> Yes, because ephemeral Diffie-Hellman key exchange is used
# IPsec encapsulation security payload (ESP) in tunneling mode

![[IPsec ESP.svg|700]]

**Encapsulation**:
1. Add ESP trailer: Padding, type encapsulated (original) packet
2. Encrypt packet and trailer
3. Add ESP header: SA identification, sequence number
4. Create Integrity Check Value (ICV); think of Checksum: MAC over original packet, ESP header, ESP trailer
5. Add new IP header

> [!question]- Why do we need to add a sequence number to the TLS header?
> TLS relies on TCP sequence numbers. IPsec doesn't run on link layer, so there are no sequence numbers available.

**Decapsulation**:
1. Strip off outer IP header
2. Look up keys and configuration using information in ESP header
3. Check MAC
4. Strip off authentication tag and ESP header
5. Decrypt original packet
6. Remove ESP trailer
7. Forward original packet

# IPsec vs TLS
|                      | [[Transport Layer Security\|TLS]]                           | IPsec                                                           |
| -------------------- | ----------------------------------------------------------- | --------------------------------------------------------------- |
| Key exchange         | TLS handshake                                               | Additional protocol: Internet Key Exchange (IKE)                |
| Authentication       | Typically only server <br> Typically using x509 certificate | Server and client <br> Many different authentication mechanisms |
| Underlying transport | Reliable (runs on top of TCP)                               | Best-effort (runs on top of IP)                                 |

# IKE and IPsec Options
- Additional messages with IKEv2:
	- If Extensible Authentication Protocol (EAP) is used
	  Ex. username/password authentication required
	- If initiator chooses unsupported Diffie-Hellman required
	- Responder can use cookies for DoS defense
- Other IPsec modes:
	- Transport mode (instead of tunneling) for end-to-end connections
	- Authenticated Header (AH) protocol (instead of ESP) for only authentication but no encryption

# Problems with IPsec
- Configuration is difficult and error-prone due to many options
- Some options do not provide expected security
	- Possible to use NULL encryption **OR** not use any message authentication
- Insecure ciphers are possible -> similar problems as TLS until 1.2
	- Example: SLOTH attack on weak hashes
	- MITM attacks (Bleichenbacher attack on IKE) possible
- Authentication Header (AH) mode also authenticates outer IP header -> breaks with NAT, since it's changed
- IPsec replay protection causes issues when a packet is lost
	- Packet numbers increase
	- IPsec tracks highest packet number received, and only accepts packets with higher numbers
- Huge code base is a problem: maintainability, verifyability, general attack surface