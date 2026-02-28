# Basics
- Circuits established over 3 relays
- Telescopic setup
- *Per-hop TCP*, established on the fly
	- Avoid TCP stack fingerprinting
- *Per-hop TLS* (except on the last hop)
	- Multiple circuits over same TLS connection
	- End-to-end HTTPS is possible
- Mainly used with Tor browser (Firefox fork)
	- Cleans HTTP/HTTPs traffic
- Supports SOCKS proxy
	- Any TCP application can make use of a Tor connection
- *End-to-end integrity* checking
	- Establishes a secure channel between client and exit relay
- *Exit policies* restrict what destinations they connect to and advertise
- Multiple *streams* per circuit
- Censorship resistance (*bridges*, *pluggable transports*)
- **Onion services** provide receiver anonymity and use `.onion` URL

## Tor Cells
- Basic unit is the cell (512 bytes)
- It contains a circuit ID and a command field (in cleartext)
- Same for cells in both directions
- A `relay` cell's payload is decrypted, and *its digest is checked*:
	- If correct (this means the current relay is the intended recipient): check command
	- Otherwise (it is an intermediate node just forwarding the cell): replace circuit ID and forward cell along
	- only exit relay sees unencrypted payload
## Circuit extension with `relay_early`
- There is no limit to the amount of relays!
- We can create long path to DoS -> Tor `extend` cells can only be contained in `relay_early` cells
- *Each relay allows only 8 `relay_early` cells per circuit* -> maximum path length capped at 9
## Circuit Setup
![[Onion Routing Circuit Setup.svg|900]]

# Onion Service
1. Onion service generates onion address (=pubkey)
2. Onion service sets up introduction points (IP)
3. Onion service publishes service descriptor
4. Client retrieves service descriptor
5. Client sets up session with rendezvous point (RP)
   Client picks a fresh, random rendezvous *cookie* & sends it to RP
6. Client contact introduction point (IP)
   Client sends [[Diffie-Hellman Key Exchange|Diffie-Hellman share]], rendezvous point address and rendezvous cookie, encrypted for the onion service
7. Introduction point (IP) forwards introduction to onion service
8. Onion service contact rendezvous point
   Onion service sends rendezvous cookie and $g^{y}$
9. Rendezvous point forwards $g^{y}$ to client
10. Rendezvous point connects circuits
    Onion service and client have shared secret $g^{xy}$

![[19252.png|600]]

# Directory Authorities
- *10 directory authorities* running a consensus algorithm
- The authorities track the state of relays, store their public keys
- Client software (Tor browser) comes with a list of the authorities' keys
	- A client accepts a consensus document if signed by >= 50%
- The centralized authorities are an *important weakness* of Tor
	- An adversary compromising 5 authorities can compromise Tor
- Every relay periodically reports a signed statement (state, stats.)
- DAs also act as *bandwidth authorities*: verify bandwidth of nodes
	- This determines the stable and fast flags, and weight
- Every hour, the DAs compute and sign a new consensus document
- Sybil protection: DAs limit the number of relays per IP subnet
- Centralized architecture can be a problem for scalability
	- Solved through caching: almost every relay acts as a *directory cache*
# Censorship Resistance
- *Problem*: relay nodes are publicly listed and can be blocked
- The Tor network contains several *bridge relays* (or bridges)
	- Not listed in main Tor directory, downloaded on demand
	- Used to circumvent censors which block IP addresses of Tor relays
	- Not (all) publicly available, some distributed through friends' networks
- *Problem*: deep packet inspection allows detection of Tor traffic
- **Solution**: obfuscate the traffic (*pluggable transports*)
- TOR users have been de-anonymized using [[Anonymous-Communication Systems#Traffic analysis]]
## Pluggable Transports
- Obfuscation tries to hide Tor traffic features
	- Packet lengths
	- Timing
	- Additional encryption
	- ...
- Censors improve their detection heuristics
  -> This gives rise to the *censorship arms race*