---
Tags: #lecture
Created: 2026-01-09 23:34:22
aliases: TLS
---
(Links:: [[Network Security]])

Simply put, TLS is the underlying protocol that allows for the "s" in [[HTTPS]].
# TLS Security Goals
- We would like security for banking, government, medical data, ...
- We do not trust the transport layer -> attack vector
- [[TCP]] is *reliable*, but only without any adversary 

- A simple symmetric encryption protocol would not suffice since an attacker could
	- reorder packets
	- perform a [[Relay Attack]]
	- delete packets

Our goal for a secure channel has **confidentiality** and **integrity** (at the channel level), where an attacker has *full control over the network*. Additionally, we want no *reordering*, *replay*, *dropping* of messages. We also want to be able to *authenticate* the client and server. Lastly, we want *forward secrecy*, so that intercepted and stored data cannot be unencrypted after the fact, once the secret key is compromised.
# TLS 1.3 Intro
- TLS runs "in the middle" between application layer and transport layer
- From the point of view of TCP, TLS is an application
- From the point of view of application, TLS is a transport
	- Easy to add TLS to new applications: transparent
- The main benefits over the older 1.2 version are:
	- TLS 1.3 was proven mathematically to be secure
	- Client data can be sent after 1-2 RTT for the setup instead of 3

> [!info] High Level Goals
> > The primary goal of TLS is to provide a **secure channel between two peers**
> > ~ TLS 1.3 [RFC 8446]
>
> See also [[CIA Security Triad]]
> 
> **Authentication**
> - Server side of the channel is always authenticated
> - Client side is optionally authenticated
> - Via asymmetric cryptography or a symmetric pre-shared key
> 
> **Confidentiality**
> - Data sent over the channel is only visible to the endpoints
> - TLS does not hide the length of the data it transmits (but allows padding)
> - Data stays confidential after a key compromise: forward secrecy
> 
> **Integrity**
> - Data sent over the channel cannot be modified without detection
> - Integrity guarantees also cover reordering, insertion, deletion of data.

> [!info] Secondary Goals
> **Efficiency**
> - Attempt to minimise crypto overhead
> - Minimal use of public key techniques; maximal use of symmetric key techniques
> - Minimise number of communication round trips before secure channel can be used
> 
> **Flexibility**
> - Protocol supports flexible choices of algorithms and authentication methods
> 
> **Self-negotiation**
> - The choice is done "in-band", i.e. as part of the protocol itself
> - This is done through the version negotiation and cipher suite negotiation process: client offers, server selects
> 
> **Protection of negotiation**
> - Aim to prevent [[MITM]] attacker from performing version and cipher suite downgrade attacks
> - So the cryptography used in the protocol should also protect the *choice* of cryptography made

- **TLS Ciphersuites**: String encoding algorithms used by handshake and record protocols 
	- TLS 1.3: TLS_CIP_MAC
# TLS 1.3 Record Protocol
- TLS was designed to be a *stream-oriented* API for application
	- TLS fragments the stream into discrete units called *records*
- **Cryptographic protections**
	- Uses record sequence number to construct nonce for AEAD encryption
	- Prevents reflection attack using different symmetric keys for different directions

![[TLS Record Processing Pipeline.svg|600]]
- `ctype` field:
	- Single byte representing content type
	- Indicates whether content is handshake message, alert message or application data
	- AEAD-encrypted inside record; header contains dummy value `otype` to limit traffic analysis
- `padding`
	- Optional feature that can be used to hide true lengths of fragments
	- Not needed for encryption
	- Sequence of 0x00 bytes after non-0x00 content type field
	- Removed after integrity check, so no padding oracle issues arise
- AEAD nonce:
	- Nonce = SQN ⨁ IV
	- Constructed from 64-bit sequence number (SQN)
	- SQN is incremented for each record sent on a connection
	- SQN is masked by XOR with IV field
	- IV is a fixed (per TLS connection) pseudorandom value derived from secrets in [[#TLS 1.3 Handshake Protocol]]
	- IV masking ensures nonce sequence is "unique" per connection, good for security in multi-connection setting
	- When packets are reordered, *decryption fails*
- Record header
	- Contains dummy type field ("application data", 1 byte), legacy version field (2 bytes), length of AEAD ciphertext (2 bytes)
	- Version field is anyway securely negotiated during handshake
	- SQN is not included in header, but is maintained as a counter at each end of the connection 

> [!important] When the decryption fails, aport the connection
> Connection is torn down, key material thrown away

- TLS allows *rekeying*, since even popular encryption schema rekey every ex. $2^{24}$ records
- Attacks not prevented by TLS 1.3 Record Protocol:
	- Truncation attacks on the stream of records
	- Application-layer confusion: record boundaries != application data unit boudnaries
	- Timing attacks on the padding scheme (recognised in RFC)
# TLS 1.3 Handshake Protocol
Assuming a [[Public Key Infrastructure]], we want the client and server to establish a fresh symmetric key for the Record Protocol which 
- is only visible to the trusted parties (confidentiality)
- cannot be changed, and such that the key used is the one they agreed on (session pairing)
- enables authentication of the parties
- guarantees forward secrecy

Additionally, we want it to be *fast*, *self-negotiating* (both parties can use cryptographic protocol) and can *downgrade resistance*.
The base of the handshake is the [[Diffie-Hellman Key Exchange]], specifically *Finite Field Diffie-Hellman*:

![[Finite Field Diffie-Hellman.svg|700]]

- Secure under distinguisher Diffie-Hellman assumption:
	- Hard to distinguish triple $(g^{a}, g^{b},g^{ab})$ from $(g^{a},g^{b},g^{c})$, $c \in \{1,...,q\}$

$$
\begin{array}{c|c|c}
& \text{Finite Field Diffie-Hellman (FFDH)} & \text{Elliptic Curve Diffie-Hellman} \\ \hline
\text{Public Key Size} & \text{256-1024 bytes} & \begin{array}{lr}\text{32 or 56 bytes} \quad \;\; & \text{65 or 97 or 133 bytes}\end{array} \\ \hline
\text{Algorithm} & \text{FFDHE 2048-8192} & \begin{array}{lr} \text{x25519 or x448} & \text{secp256r1 or 384 or 521} \end{array} \\ \hline
\text{Security Level} & \text{112-192 bits} & \begin{array}{lr}\text{128 or 256 bits}\qquad \;\;\; & \text{128/192/256 bits}\end{array}
\end{array}
$$

- **Ephemeral Diffie-Hellman** allows for forward secrecy:
	- Just pick new keys at every connection
		- $esk_{A},epk_{A}=KeyGen()$
		- Both for the client and the server
	- Throw away the secret key afterwards

Now we have both confidentiality and forward secrecy. To get session pairing, authentication and to able to downgrade resistance, we use **MACs & Signatures**.
## Cryptographic Negotiation
- Faster negotiation by *guessing* supported Diffie-Hellman groups
	- Possible for TLS 1.3 since there are select few supported groups
	- No need for parameter validation: in TLS 1.2, server sent $(p,g,g^{a})$ for FFDH
- If server does not like Diffie-Hellman groups offered by client, fallback to 2-RTT

![[TLS Handshake Cryptographic Negotiation.svg|400]]
## Authentication
- Server always authenticates at TLS level, optional for clients
- `ServerFinished` and `ClientFinished` guarantee *session pairing*: Client and server agree on transcript
- Because authentication come after crypto negotiation, all traffic for the handshake is *encrypted*

![[TLS Handshake Authentication.svg|700]]
# TLS 1.3 Resumption and 0-RTT
- Assume that we have a shared secret -> no need for [[PKI]]
- PreSharedKey just contains the *identity* (a label) for the PSK to use
	- New key derived from PSK and nonces in `Client/ServerHello`
- No signatures, **authentication of both parties now baed on PSK** and Finished messages
- Reduces latency and server load, since clients return frequently to same servers
- **Not forward secure** (all new keys derived from existing secrets)

![[PSK Handshake.svg|600]]

- Can also be used combined with the (EC)DHE key exchange to bring back forward secrecy for application data
- Still more lightweight than full exchange
- Allow for non-forward-secure `EarlyData`
	- EarlyData can even be replayed across connections
- With 0-RTT mode, `EarlyData` can be replayed
	- impossible to mitigate, since the server doesn't confirm a session with the client

There are two cases where two parties agree on a PSK: 
1. Out-of-band (e.g. in person)
2. A previous TLS 1.3 connection is *resumed*

## Resumption
![[TLS Resumption.svg|500]]
- Key derivation is a bit more complicated: $PSK = KDF(KDF(K, \text{"rms"}), \text{"resumption"}, \text{ticketNonce})$
- Multiple `NewSessionTickets` can be issued
- TLS 1.3 RFC does not specify how to manage the **PSK database**
	- Some server "offload" storage to the clients: $\text{psk-id}=Enc(STEK, PSK)$

> [!danger]- Server PSK offloading
> Assume $\text{psk-id}=Enc(STEK, PSK)$
> - STEK = Session Ticket Encryption Key
> - `psk_id` is sent in the clear by the client during resumption handshake!
> - Forward secrecy compromised when STEK is leaked

# TLS in Practice
- TLS doesn't protect if it is badly implemented
- TLS can be compromised if application layer is misused
- Since the domain name is sent in the `ClientHello` message (Server Name Indication), an attacker is able to see which websites you visit (not encrypted).
- Encrypted Client Hello:
	- Have fake outer ClientHello and encrypted *inner* ClientHello
	- Depends on DNS for $pk_ECH$ key delivery
	- Use of ECH can be detected (e.g. block in Russia)
	- Not sufficient alone: DNS A/AAAA query would leak domain anyway!
- TLS fails if Certificate Authority is compromised ([[WebPKI]])
## Hybrid Key Exchange

___
References:
- https://tools.ietf.org/html/rfc8446
- https://tls13.xargs.org/