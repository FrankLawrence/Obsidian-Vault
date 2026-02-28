---
Tags: exercise
Created: 2026-01-21 20:56:54
---
(Links:: [[Network Security]])
# Exercise sheet 06: TLS Part 1 (28 points)

### Question 1 (2 points)
> [!question] As we have learnt in the last lecture, the TLS 1.3 record protocol offers an
> optional padding feature to hide the true length of fragments. Can you think of
> a possible way to recover the true length of the fragments? Think of the way
> TLS implementations might remove the padding. Could it reveal something?
> (Note: it’s not a padding oracle)

_Solution_:
It is still possible to guess the length of the padding using a
side-channel attack: it would be hard to make a padding-removal function
perfectly constant-time, and even then the output of the function would
probably be fed into data-dependant functions. Therefore, it can be possible
to observe how long a given TLS implementation takes to remove the padding
from a record (e.g. by measuring the covariance of the response time to
requests for which the amount of padding is known), and use those measurements
to infer the unpadded length of a record.

### Question 2 (6 points)
In the TLS 1.3 record protocol, the per-record nonce is based upon a
64-bit counter.

**2.1.** (2 points)
> [!question] The counter always starts at 0. Why is this not a problem?

_Solution_:
First, because it is XORed with a per-session IV before being used in
the AEAD cipher. Second, it only needs to be a nonce: a number used
once. It does not really matter if it is predictable as long as it
is not reused with the same keys. As keys are newly generated at the
beginning of each session, this condition is fulfilled.

**2.2.** (2 points)
> [!question] Given an average record length of 1000 bytes and a modern connection
> speed of 1Gb/s, and ignoring the fact that any sane TLS implementation
> would rekey and update the IV earlier[1], how many years would it take to
> reset the counter back to 0?
> 
> [1]: https://datatracker.ietf.org/doc/html/rfc8446#section-7.2

_Solution_:
Possible nonce values: 2<sup>64</sup>

Average record length: 8⋅1000 bits

=> Amount of data to be transmitted before nonces repeat:  2<sup>64</sup> ⋅ 8⋅1000
bits

Data transmission rate: 1⋅10<sup>9</sup> bits/s

Seconds in a year: 3600⋅24⋅365 s/year

=> Transmission rate per year: 1⋅10<sup>9</sup> ⋅ 3600⋅24⋅365 bits/year

Years until nonce repeats: (2<sup>64</sup> ⋅ 8⋅1000 bits) / (1⋅10<sup>9</sup>
⋅ 3600⋅24⋅365 bits/year) = 4, 679, 539 years

**2.3.** (2 points)
> [!question] What would happen if some records were to be received out of order?

_Solution_:
As verification and decryption uses the locally stored counter
as the input nonce when calculating the AEAD authentication tag to
compare, an out-of-order record will lead to an authentication
failure and the connection will be torn down.

### Question 3 (4 points)
TLS 1.3 has fixed several vulnerabilities and design problems that plagued
previous versions of the protocol. It also greatly reduced the complexity of
some key aspects of the protocol.

**3.1.** (2 points)
> [!question] In TLS 1.2, cipher suites were represented as, for example:
> 
> ```
> TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
> ```
> 
> In TLS 1.3, the same cipher suite is instead represented as:
> 
> ```
> TLS_AES_128_GCM_SHA256
> ```
> 
> Clearly, some information has been removed. What is missing in the new
> representation? Can you speculate (or find out) why this information was
> removed?

_Solution_:
The specification for key exchange (`ECDHE`) and signature algorithms (`ECDSA`)
has been removed.
This is to separate the choice of algorithms between for key exchange,
signatures, and the record protocol.

**3.2.** (2 points)
> [!question] The total number of accepted cipher suites, key exchanges, and signature
> algorithms has also been greatly reduced. What are the security advantages
> of this approach?

_Solution_:
Many of the parameter choices supported by TLS 1.2 suffered from various
vulnerabilities. Thus, TLS 1.3 allows only a very limited set of algorithms
to reduce the attack surface. And this is not only limited to the choice of
algorithms: even the choice of Diffie-Hellman groups and elliptic curves is
restricted to a few, high-security options.

### Question 4 (6 points)
At USENIX '13, AlFardan, Bernstein, Paterson, Poettering, and Schuldt
presented an attack on the usage of the RC4 stream cipher in TLS version
1.2 and below. (Note that RC4 has since been prohibited in RFC7465 for
the affected versions).

The attack exploits probability biases in the bytes of the RC4 keystream,
where certain bytes are more common in certain positions (single-byte bias
attack). The authors also present a variant exploiting biases in
consecutive bytes (double-byte bias attack).

The attack allows them to *fully* recover the TLS-protected plaintext,
given enough repeated encryptions of the same data.

This information is sufficient to solve the questions below, but for those
who are curious, the paper below explains the mechanism in depth (and
may make it easier to answer questions):

["On the Security of RC4 in TLS"](https://www.usenix.org/conference/usenixsecurity13/technical-sessions/paper/alFardan)

**4.1.** (4 points)
> [!question] You want to implement this attack, targeting a secure cookie in the HTTPS
> session between a client running a web browser *B* and a web server *W*.
> 
> How could you force the browser to transmit the same data multiple
> times? You don’t have direct control of the browser nor of the
> web server, but you know *W* uses secure cookies over HTTPS, and you
> control a second web server, *E*, which the client visits.
> 
> Hint: The authors of the paper were able to perform this! Perhaps they
> have written the methods down in their Validation sections?

_Solution_:
The client will run javascript code served from *E*. While this code
cannot directly access cookies from *W* (due to the same-origin policy,
*SOP*), it can make requests to *W*: the cookies will be automatically
attached by the browser to the new requests. The JavaScript can repeat
enough request to make the attack succeed; allowing the attacker to
recover the plaintext cookie.

This assumes many things and remains a proof-of-concept attack, for
example at the time of the paper's release, it would've required the
victim web browser to keep open a connection with *E* for over 75 hours,
and also required both *B* and *W* to not notice the overloaded link
between the two.

It also assumes that the attacker is able to sniff the packets in the
link between *B* and *W* in order to perform the statistical analysis
on the ciphertexts.

However, it was still severe enough that RC4 has since then been
prohibited in RFC7465.

**4.2.** (2 points)
> [!question] Single-byte biases in the RC4 cipher are stronger at the start of the
> keystream. Using this information, propose a possible mitigation for the
> attack.

_Solution_:
It is possible discard the initial keystream of RC4. Note that this
will not stop the double-byte bias attack.

### Question 5 (5 points)
One famous OpenSSL attack, Heartbleed [1], exploits some server
implementations of keep-alive (heartbeat) messages. These messages can
be sent even before the handshake is complete. They are meant to check
if the other side is still online and verify that data transfer works
correctly. One side sends a buffer and requests a certain amount of
characters from that buffer. Unfortunately, the vulnerable servers had a
missing bounds check...

[1] <https://heartbleed.com/>

**5.1.** (2 points)
> [!question] Let’s assume that your website uses login cookies (e.g., string
> LOGIN_COOKIE=16 byte value) and stores recent request data in nearby
> memory locations. How would you use Heartbleed to hijack a session of
> someone who logged in recently?

_Solution_:
<img src="assets/heartbleed_explanation.png" alt="image" />

<https://xkcd.com/1354/>

**5.2.** (1 points)
> [!question] Does client authentication (e.g., using client certificates) offer
> protection against Heartbleed?

_Solution_:
No, it doesn’t help because heartbeat messages can be sent and be replied
to during the handshake, i.e. before the client is authenticated.

**5.3.** (2 points) (bonus)
> [!question] Let’s assume that you were using TLS 1.2 with ciphersuite `TLS_ECDH_ECDSA_WITH_AES_256_CBC_SHA384`,
> and the server you connected to was vulnerable to Heartbleed.
> Can an attacker who recorded your traffic decrypt it later?
> What if you had used any of the ciphersuites of TLS 1.3?

_Solution_:
The TLS ciphersuite we mention does not provide PFS: an attacker who recorded the TLS handshake
and leaked the server's private key through Heartbleed can decrypt the TLS stream.
All TLS 1.3 ciphersuites provide PFS, so in that case the traffic is safe,
no matter whether the attacker leaked the server's key or not.

### Question 6 (5 points)
The Heartbleed attack we have just seen belongs to the category of
implementation bug exploits. The TLS protocol has had, during the years,
several of these attacks, which have been solved by simply patching
software libraries and servers.

**6.1.** (2 points)
> [!question] In 2014, Apple discovered an implementation error in SecureTransport,
> the TLS library used in iPhones and Macs. What is the problem with this
> C code?
> 
> ```c
> static OSStatus SSLVerifySignedServerKeyExchange (SSLContext *ctx, bool isRsa,
>         SSLBuffer signedParams, uint8_t *signature, UInt16 signatureLen) {
>     OSStatus err;
>     ...
>     if ((err = SSLHashSHA1.update(&hashCtx, &serverRandom)) != 0)
>             goto fail;
>     if ((err = SSLHashSHA1.update(&hashCtx, &signedParams)) != 0)
>             goto fail;
>             goto fail;
>     if ((err = SSLHashSHA1.final(&hashCtx, &hashOut)) != 0)
>             goto fail;
>     ...
>     err = sslRawVerify(...);
> fail:
>         SSLFreeBuffer(&signedHashes);
>         SSLFreeBuffer(&hashCtx);
>         return err;
> }
> ```

_Solution_:
After the second if, there is a second repetition of the line `goto fail;`,
probably caused by a copy-paste error. Said line is not tied to any if
structure, so it will be executed every time. If all the checks prior to
this succeeded, the program will always return an error of 0 (no error)
and skip the actual TLS verification below.

**6.2.** (3 points)
> [!question] How might an attacker take advantage of this bug? You can find a hint
> in the method signature.

_Solution_:
The affected method signature tells us that it is responsible for
verifying the signature of the server key exchange message. Now, since
this always returns true, an attacker could MitM and use the certificate
of a legitimate website signed with a mismatched private key. After
that, the MitM will sign the ServerKeyExchange message with any key, or
not sign it at all. This breaks the authentication of the ephemeral key
in DHE and ECDHE cipher suites, and thus the authentication of all the
messages encrypted using that key.

The result is of course the ability to serve a fake website instead of
the original one.



---
References: