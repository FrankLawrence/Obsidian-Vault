---
Tags: exercise
Created: 2026-01-21 20:57:20
---
(Links:: [[Network Security]])
# Exercise sheet 07: TLS Part 2 (31 points)

### Question 1 (4 points)
Consider TLS 1.3.

**1.1.** (2 points)
> [!question] We saw in the lecture that the TLS 1.3 Handshake always authenticates the server, but client authentication is left optional. Realistically, why do you think it was designed that way? (Why would it be infeasible, or useless to authenticate every user that connects to a web server?)

_Solution_:
It is not feasible to have a PKI for it.

Also, by the structure of the internet, ANY user can connect to servers.
There is nothing for servers to authenticate for if anyone is allowed
anyway. Conversely, if a web server is designed to be restricted to
certain users, then a client certificate is also often required.

**1.2.** (2 points)
> [!question] Most of TLS 1.3's handshake is encrypted. Why is this a good thing? What kind of insecurities can derive from having a cleartext handshake like in TLS 1.2 and below?

_Solution_:
The certificate (both server-side and client-side, if any) can reveal
any anonymised identities via passive sniffing.

But this is only one of many reasons, including making it harder for a
potential adversary to tamper with things.

### Question 2 (9 points)
Let's take a look at the handshake in more depth.

We want to avoid downgrade attacks where possible in the protocol, as
older versions have known vulnerabilities (see e.g. POODLE, Lucky 13, etc).
Downgrading all the way down to SSLv3 with RC4 selected as a stream cipher
can cause statistical analysis attacks as seen in last week's exercise too.

**2.1.** (2 points)
> [!question] An attacker can drop or modify packets in the network. Can they downgrade the negotiated TLS version in TLS 1.3?

_Solution_:
No. ClientFinished and ServerFinished messages contain a signature/MAC
tag of the hash of the handshake transcript. Missing or dropped messages
will change the hash and cause the authenticated signature to not match.

The connection could actually already fail earlier than the Finished
messages, because the ServerCertificateVerify message sent from the
server during the handshake already contains the server-side signature
of the hash of messages until that point.

(See https://tls13.xargs.org/#server-certificate-verify for an example
with ServerCertificateVerify)

**2.2.** (3 points)
Some (buggy) legacy web servers used to close the connection when they received a packet that contains an unknown newer TLS version field. As a workaround, during the transition from TLS 1.1 to 1.2, many clients started implementing a fallback mechanism for backwards compatibility: if a connection was closed by the server, they would attempt a new TLS handshake, but with a lower TLS version number.

> [!question] In TLS 1.2, how might attackers abuse this to perform downgrade attacks?

_Solution_:
A downgrade attack is now possible: the attacker drops packets when the
client attempts to use higher TLS version, the client will fall back to
a previous version.

**2.3.** (2 points)
> [!question] TLS 1.0, 1.1, 1.2 etc all have an authenticated transcript verification
> mechanism at the end of the handshake just like TLS 1.3. Then, why
> does that not protect against the downgrade attack described in the
> previous question?

_Solution_:
The client transcript after a fallback will be incomplete — it will not
include the previous handshake messages.

**2.4.** (2 points)
> [!question] When clients and servers support both TLS 1.3 and older TLS versions,
> the TLS 1.3 record protocol can actually prevent downgrade attacks to
> older protocol versions. Investigate how that mechanism works, by
> searching RFC8446 for "downgrade protection mechanism" and briefly
> summarise how it works from the perspective of both the server and
> client.

_Solution_:
It works as follows:

1. If a server receives a ClientHello that says it only supports TLS 1.2
   or lower, then it will downgrade, but the server's random nonce in
   the ServerHello must have its last 8 bytes set to "DOWNGRD\x01".
2. The client, upon receiving this ServerHello, can check. If the client
   wanted TLS 1.3, but for some reason the server has indicated that it
   has explicitly downgraded to TLS 1.2, it can suspect of MiTM attacks.

### Question 3 (10 points)
Let's investigate potential replay attacks.

**3.1.** (3 points)
> [!question] You discover that your TLS library uses an insecure pseudo-random number
> generator for nonces in the handshake. This PRNG has a very short period
> — its output is repeated every 10<sup>5</sup> accesses. Propose a
> possible attack, assuming a Dolev-Yao adversary.
> 
> Assume that the PRNG is used to generate Diffie Hellman key shares is
> different, and is unaffected by the insecurity.
> 
> Think about the different TLS 1.3 handshake modes (Full DHE, PSK,
> PSK + DHE). Is your attack possible in all modes, or no? Why?

_Solution_:
If the TLS (1.3) handshake includes a Diffie-Hellman Ephemeral key
exchange, attacks are unlikely: the Hello messages still contain an
unpredictable source of randomness.

In the case of a PSK-only handshake (no DHE) or if the servers reuses
Diffie-Hellman key shares (e.g., to save computational cycles) replay
attacks become possible: if the attacker can predict when a nonce in the
server repeats itself, a transcript of a previously recorded connection
can be replayed — all the other parameters in the TLS key establishment
will stay the same.

**3.2.** (2 points)
> [!question] TLS 1.3 PSK mode (0-RTT) handshake uses a pre-shared key for its
> encryption and does not have a key exchange. Thus, we concluded in the
> lectures that it does not have forward secrecy.
> 
> TLS 1.3 PSK (EC)DHE mode attempts to re-introduce forward secrecy, while
> still enabling the benefits of 0-RTT (i.e. send application data with
> the first message already -- `EarlyData`).
> 
> Does this mode have any security tradeoffs that the application layer
> should be aware of?

_Solution_:
Replay attacks are possible as EarlyData uses the PSK to encrypt. This
part is also not forward secure.

Applications should make sure that it can work around this.

**3.3.** (5 points)
> [!question] TLS 1.3 PSK (EC)DHE mode is in fact what is used for TLS 1.3 0-RTT
> Resumption.
> 
> The PSK in this case is derived from a master secret shared between the
> two parties. A "NewSessionTicket" message, issued optionally by the
> server during the handshake, associates this PSK with a "ticket", which
> is often the PSK encrypted and authenticated with a key that only the
> server knows. In this case, the key is called the STEK (Session Ticket Encryption Key).
> 
> With that in mind, read this short GitLab issue thread on GnuTLS (a
> commonly used TLS implementation library).
> 
> https://gitlab.com/gnutls/gnutls/-/issues/1011
> 
> How can a passive MitM attacker exploit the GnuTLS bug? What are the
> consequences?
> 
> What about an active MitM attacker?

_Solution_:
Due to a bug in GnuTLS, a commonly used TLS implementation library,
the STEK was actually all-zeros in GnuTLS before the first key rotation.

This means, a passive adversary is able to decrypt the "ticket"
(which, remember, is the PSK encrypted and authenticated with the STEK)
to retrieve the PSK.

A passive MiTM adversary can use the PSK to decrypt EarlyData for any
client resumption requests using the relevant, because the EarlyData is
encrypted with the PSK.

An active MiTM attacker can use this PSK to pretend to be the server to
a victim client, or pretend to be the client to a victim server, ruining
all security guarantees for future conversations. (Security guarantees
for past conversations depend on the same PSK being used before, and
that they weren't using DHE; otherwise they remain confidential)

### Question 4 (8 points)
The modern web has long moved away from the one-server-per-webpage paradigm.
Today, content is often replicated across multiple servers, for example in
content-delivery networks (CDNs). At the same time, a single server (with a
single IP address) may host many virtual machines serving many different
webpages. The server therefore needs to hold the certificates for all the
websites it is hosting.

This, however, poses a problem for TLS: during the TLS handshake, the server
does not know which certificate to send to the client, as the HTTP header
containing the hostname (the name of the webpage being requested) is sent
only after the handshake.

**4.1.** (2 points)
> [!question] The common way to solve this problem is to use the server name
> indication (SNI) extension to TLS. This is an unencrypted field in the
> TLS header, specifying the hostname of the service being requested.
> 
> Does this solution have privacy drawbacks? If so, argue why and what the
> possible mitigating strategies are, considering the wider ecosystem of
> protocols you often need to establish a TLS connection.

_Solution_:
SNI tells everyone who is passively observing traffic which services users
are connecting to. This facilitates censorship and traffic analysis.

A potential mitigating strategy is to use Encrypted Client Hello (also
explained in further depth with the next question), where the client
uses the server's public key to encrypt the Client Hello message. The
server's public key is often made available via a DNS record.

Unfortunately, this alone would not prevent an attacker from performing
traffic analysis, because the lookup of the server's public key on the
DNS will be unencrypted and easily observed. Even if the server's ECH
public key is distributed without using DNS, clients will often still
use DNS to resolve the domain name to an IP address.

By using encrypted DNS protocols, like DNS over HTTPS or DNS over TLS,
the ECH strategy finally becomes viable.

**4.2.** (2 points)
> [!question] Can you think of another "lower-layer" solution that would allows us
> to address multiple VMs on a single physical server? Mention advantages
> and drawbacks.

_Solution_:
The server could assign a different IP address to each website. The advantage
is that no key distribution nor additional encryption are needed. The
disadvantages are that using many different IP address is expensive (in case
of IPv4, where addresses have run out), and complex to manage.
Also, via a reverse DNS lookup the service a user connects to can still be
leaked without too much effort.

**4.3.** (4 points)
> [!question] Another way to solve the problem of having different certificates for
> each webpage/service on a server, is to use one common certificate for
> everything.
> 
> At USENIX '21, Brinkmann, Dresen, Merget, Poddebniak, Müller, Somorovsky, Schwenk, and Schinzel presented "ALPACA", an attack that can exploit
> such configuration.
> 
> Read the short "Introduction" paragraph on the ALPACA website:
> https://alpaca-attack.com/ (For this question, the website is enough and
> you are not required to read the full paper. But we encourage you to do
> so if you are interested.)
> 
> To the best of your ability, explain how this attack works.

_Solution_:
ALPACA is a protocol confusion attack enabled by TLS. The attack works by
exploiting two victim services that use different application-layer
protocols (like HTTP and FTP) but share a common TLS certificate. By
using MiTM to send traffic destined to one to another, the authors
claimed they could potentially steal HTTPS cookies or perform
cross-site scripting attacks!

(A reminder that TLS does not protect the IP or port, enabling the same
certificate to be used across multiple services).

The TLS Server Name Indication extension (mentioned above) could help mitigate
attacks like ALPACA.
