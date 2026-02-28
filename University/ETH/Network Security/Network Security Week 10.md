---
Tags: exercise
Created: 2026-01-22 23:49:13
---
(Links:: [[Network Security]])
# Exercise sheet 10: Public Key Infrastructure (27 points)

### Question 1 (12 points)
In November 2022 Ian Carroll published a [blog post](https://ian.sh/etugra) 
disclosing severe security issues in the critical infrastructure of the turkish 
CA **e-Tugra**. This sparked a [discussion](https://groups.google.com/a/mozilla.org/g/dev-security-policy/c/C-HrP1SEq1A) 
within the community whether e-Tugra can still be trusted and what the consequences should be.

**1.1.** (2 points)
> What are the the vulnerablities found by Ian Carroll?

_Solution_:
Vulnerabilities included:
- **Default credentials**: The administrative tools were accessible with default credentials, 
  which allowed access to millions of log entries.
- **Sensitive Data Exposure**: By signing up in the administrative panel one could access 
  sensitive information like personal data but also domain validation codes.

These vulnerabilities could potentially be exploited to issue fraudulent certificates, impersonate arbitrary domains and steal personal information.

**1.2.** (2 points)
> What is the problem if just one CA get compromised?

_Solution_:
Even if just a single Certificate Authority (CA) is compromised, it can pose a severe 
threat to internet security. Such a compromise has a global impact as CAs are trusted 
worldwide. It can affect millions of users and services across different regions.

A compromised CA could issue fraudulent digital certificates for any website or domain. 
This opens up the possiblity to impersonation and man-in-the-middle attacks.

**1.3.** (3 points)
> Did e-Tugra handle this incident in an appropriate manner? What were the consequences?

_Solution_:
e-Tugra handled this incident poorly. Their incident report was lacking in detail and 
it downplayed the seriousness of the incident. The report did not contain a comprehensive 
root cause analysis and did not detail the changes made to prevent this from happening in 
the future.

Additionally, e-Tugra reacted slowly and with inconsistent responses. This lack of ugency 
is also reflected by the fact that the penetration testing report was only made available 
by April 2, 2023.

As a result the following two root certificates were removed from the Mozilla root store:

    E-Tugra Global Root CA RSA v3
    SHA-1 Fingerprint: E9A85D2214521C5BAA0AB4BE246A238AC9BAE2A9
    SHA-256 Fingerprint: EF66B0B10A3CDB9F2E3648C76BD2AF18EAD2BFE6F117655E28C4060DA1A3F4C2


    E-Tugra Global Root CA ECC v3
    SHA-1 Fingerprint: 8A2FAF5753B1B0E6A104EC5B6A69716DF61CE284
    SHA-256 Fingerprint: 873F4685FA7F563625252E6D36BCD7F16FC24951F264E47E1B954F4908CDCA13

**1.4.** (2 points)
> Mozilla, Chrome and Microsoft all have trust stores where trusted CA certificates are listed. 
> Complete the table below by specifying whether a trust store does or does not include the e-Tugra 
> root certificates. You can find more information about the trust stores [here](https://www.ccadb.org/resources).
> 
> |Trust Store    | Included (yes/no) |
> |:--------------|:---------------|
> |Microsoft      |...    |
> |Mozilla        |...    |
> |Chrome         |...    |

_Solution_:
|Trust Store    | Included (yes/no) |
|:--------------|:---------------|
|[Microsoft](https://ccadb.my.salesforce-sites.com/microsoft/IncludedCACertificateReportForMSFT)     |yes    |
|[Mozilla](https://ccadb.my.salesforce-sites.com/mozilla/IncludedCACertificateReport)        |no    |
|[Chrome](https://chromium.googlesource.com/chromium/src/+/main/net/data/ssl/chrome_root_store/root_store.md)         |no    |

**1.5.** (3 points)
> There are two more interesting stories about CAs from recent years. The first one is about Trustcor. 
> This CA was not accused of negligent behavior regarding certificate issuance, rather they were linked 
> to other shady business. This sparked distrust. You can read more about this [here](https://archive.ph/WMCHb) 
> and [here](https://groups.google.com/a/mozilla.org/g/dev-security-policy/c/oxX69KFvsm4/m/RJ3QSft-AgAJ).
> 
> The second one is quite similar to e-Tugra. It involves the CA Entrust, which showed concerning sloppy behavior 
> when they responded to security incidents. If you would like to know more about the distrust in Entrust, 
> have a look at [this blog post](https://webpki.substack.com/p/reflections-on-entrusts-distrust). 
> See the detailed disussions [here](https://groups.google.com/a/mozilla.org/g/dev-security-policy/c/LhTIUMFGHNw/m/wTqV_KlLAQAJ) 
> and [here](https://groups.google.com/a/mozilla.org/g/dev-security-policy/c/jCvkhBjg9Yw?pli=1).
> 
> 
> Similarly to the previous question, find out which trust stores include the Trustcor and Entrust root certificates.
> 
> 
> **Trustcor:**
> 
> |Trust Store    | Included (yes/no) |
> |:--------------|:---------------|
> |Microsoft      |...    |
> |Mozilla        |...    |
> |Chrome         |...    |
> 
> **Entrust:**
> 
> |Trust Store    | Included (yes/no) |
> |:--------------|:---------------|
> |Microsoft      |...    |
> |Mozilla        |...    |
> |Chrome         |...    |

_Solution_:
CSV file listing inclusion of CAs certificates for various trust stores can be downloaded here: ([download link](https://ccadb.my.salesforce-sites.com/ccadb/AllIncludedRootCertsCSV))

**Trustcor:**

|Trust Store    | Included (yes/no) |
|:--------------|:---------------|
|[Microsoft](https://ccadb.my.salesforce-sites.com/microsoft/IncludedCACertificateReportForMSFT)     |yes    |
|[Mozilla](https://ccadb.my.salesforce-sites.com/mozilla/IncludedCACertificateReport)        |no    |
|[Chrome](https://chromium.googlesource.com/chromium/src/+/main/net/data/ssl/chrome_root_store/root_store.md)         |no    |


**Entrust:**

|Trust Store    | Included (yes/no) |
|:--------------|:---------------|
|[Microsoft](https://ccadb.my.salesforce-sites.com/microsoft/IncludedCACertificateReportForMSFT)     |yes    |
|[Mozilla](https://ccadb.my.salesforce-sites.com/mozilla/IncludedCACertificateReport)        |yes, but distrust signatures made after November 30, 2024     |
|[Chrome](https://chromium.googlesource.com/chromium/src/+/main/net/data/ssl/chrome_root_store/root_store.md)         |yes, but distrust signatures after October 31, 2024    |

### Question 2 (7 points)
In 2015, it emerged that many laptops sold by Lenovo contained a
preinstalled software package: “Superfish”. Superfish injected advertisement
in webpages visited by the users — notably, it was used to interfere with HTTPS
domains, too [2].

[2] <https://www.eff.org/deeplinks/2015/02/further-evidence-lenovo-breaking-https-security-its-laptops>

**2.1.** (2 points)
> How would you implement a software that intercepts HTTPS connections?
> Are you exposing the users of your software to MITM by arbitrary
> parties?

_Solution_:
In general, it is not a good idea to intercept TLS traffic — if asked to do
so, you should question the ethics of the request before proceeding.

You can install a self-signed local root CA and mount a MITM attack
using this CA to sign fake certificates for the domains the user is
visiting. If the CA is different for every user you are MITMing, you are
not exposing the users to MITM by arbitrary parties.

**2.2.** (2 points)
> Chances are high that you did a better job than Superfish authors in the
> previous answer. Superfish installed an universal self-signed Certificate
> Authority on victim's laptops. This CA was the same for all the instances
> of Superfish, and its private key was included in the package. Let's say you
> somehow [3] obtain this private key - how could you attack the unlucky owners
> the laptops?
> 
> [3] <https://blog.erratasec.com/2015/02/extracting-superfish-certificate.html>

_Solution_:
All (unwitting) users of Superfish are now exposed to MITM attacks from you
— nothing restricts the certificates generated by the CA to only be used locally,
and with the private key you can sign arbitrary certificates.

For example, you could impersonate ubs.com — the browser would accept the
certificate signed by Superfish CA as valid without showing any warning.

**2.3.** (2 points)
> Which PKI extension measures could be used to prevent this or similar [4]
> abuses of locally installed root CAs? (HPKP? HSTS? OCSP? CT?)
> 
> [4] <https://bugzilla.mozilla.org/show_bug.cgi?id=1567114>

_Solution_:
- HSTS: only enforces HTTPS, no information on which certificate to use.

- HPKP: could in principle, but in practice:
    > *Chrome does not perform pin validation when the certificate chain
    > chains up to a private trust anchor. A key result of this policy
    > is that private trust anchors can be used to proxy (or MITM)
    > connections, even to pinned sites. “Data loss prevention”
    > appliances, firewalls, content filters, and malware can use this
    > feature to defeat the protections of key pinning. (extract from
    > <https://chromium.googlesource.com/chromium/src/+/master/docs/security/faq.md#how-does-key-pinning-interact-with-local-proxies-and-filters>)*
- OCSP: The attacker can specify their own OCSP server on the root CA
  certificate, rendering OCSP useless.
- CT: will help to expose the misbehavior (if the certificate is
  logged/an auditor fetches it), won’t stop the browser from immediately
  accepting the certificate.

  Eventually, browsers would start blocking the rougue certificates that appear
  in the logs, e.g. using [CRLSets](https://www.chromium.org/Home/chromium-security/crlsets/) for Chromium-based browsers
  and [OneCRL](https://blog.mozilla.org/security/2015/03/03/revoking-intermediate-certificates-introducing-onecrl/) or [CRLLite](https://blog.mozilla.org/security/2020/01/09/crlite-part-1-all-web-pki-revocations-compressed/) for Firefox.

  If a real CA were to publish rogue certificates in a log, the consequences would be
  different: it would immediately be sanctioned in the CA/Browser forum,
  and lose its CA status.
  (If you want to see Ryan Sleevi roasting Let's Encrypt in the CA/B forum for a
  quite minor off-by-one-second issue, look  [here](https://bugzilla.mozilla.org/show_bug.cgi?id=1715455))

**2.4.** (1 points)
> It should by now be clear that intercepting TLS traffic presents many
> challenges. Nevertheless, corporations and computer security applications
> (so-called "antivirus software" [5]), are still widely used. Can you think
> of other problems that could emerge even with a carefully designed lawful
> interception?
> 
> [5] <https://bugs.chromium.org/p/project-zero/issues/detail?id=978>

_Solution_:
- User interaction is required for some TLS errors — responsibility
    for this interaction would be moved to the MITM box.
- The MITM box should regularly update its CRLs for correctly handling
    revoked certificates.

### Question 3 (4 points)
> Let *h* be a cryptographic hash function. The probability of a collision
> between two hash values is *p*<sub>*c*</sub>. An attacker has limited
> computing power, and can only generate *n* = 2<sup>*k*</sup>, *k* &gt; 1
> messages and the relative hashes. Calculate the probability of:
> 
> - a hash collision between the *n* generated messages and a given
>   message *m*
> - a hash collision between *n*/2 generated messages with a certain
>   content and the remaining *n*/2 messages.
> 
> In which case is the collision more likely?

_Solution_:
1) *Weak collision resistance*: the probability of a single message having a collision with $m$ would be $p_c$.
This probability is the same regardless of which of the $n$ messages we look at, and we can assume that all the hashes are independent.
Therefore, we can model this as a binomial distribution $X \sim \text{Bin}(n, p_c)$, where $X$ represents the number of generated messages that have a hash collision with $m$. We can then calculate: $$Pr[X \geq 1] = 1 - Pr[X < 1] = 1 - Pr[X = 0] = 1 - (1 - p_c)^n$$

2) *Strong collision resistance*: as above, the probability of one message (out of $\frac{n}{2}$) colliding with any of $\frac{n}{2}$ messages can be modelled as a binomial distribution $Y \sim \text{Bin}(\frac{n}{2}, p_c)$, yielding $p_s$: $$p_s = Pr[Y \geq 1] = 1 - Pr[Y < 1] = 1 - Pr[Y = 0] = 1 - (1 - p_c)^\frac{n}{2}$$
We can then model that any of the other $\frac{n}{2}$ messages collide with any of the $\frac{n}{2}$ messages as a binomial distribution $Z \sim \text{Bin}(\frac{n}{2}, p_s)$. Therefore: $$\begin{aligned}
  Pr[Z \geq 1] &= 1 - Pr[Z < 1] = 1 - Pr[Z = 0] = 1 - (1 - p_s)^\frac{n}{2}\\
  &= 1 - (1 - (1 - (1 - p_c)^\frac{n}{2}))^\frac{n}{2} \\
  &= 1 - ((1 - p_c)^\frac{n}{2})^\frac{n}{2} \\
  &= 1 - (1 - p_c)^{\frac{n}{2}\cdot\frac{n}{2}} \\
  &= 1 - (1 - p_c)^\frac{n^2}{4}
\end{aligned}$$

The second probability is always higher than the first one for $n > 4$ (and always lower for $n < 4$). They are equal for $n = 4$.
As we know that $n = 2^k \geq 4$ (as $k > 1$), the second collision is more likely to happen.
Therefore, given the same computing power, strong collision resistance is more likely to be violated than weak resistance.

### Question 4 (4 points)
CRLite is a technology to efficiently compress revocation information for the whole Web PKI into a format easily delivered to Web users.

(paper: https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=7958597&tag=1)

**4.1.** (2 points)
> What types of attacks can occur if compromised certificates are not revoked in a timely manner?

_Solution_:
Client software will continue to believe that the certificate is valid until it expires so attackers could use such certificates to perform 
effective Man-in-the-Middle (MitM) and phishing attacks against users. This can be expanded to large-scale vulnerabilities like the Debian PRNG bug    
and Heartbleed, which potentially compromised millions of private keys.

**4.2.** (2 points)
> What are the benefits of using CRLite instead of other TLS revocation mechanisms? Explain how CRLite uses Bloom filters.

_Solution_:
CRLite reduces latency and eliminates privacy concerns. CRLite also has low bandwidth costs.
- Certificate Revocation Lists (CRLs) have scalability issues, and rely on the client having enough network access to download them 
  prior to checking a certificate's status.
- The Online Certificate Status Protocol (OCSP) also suffers from scalability issues. It relies on the client having network access at the time of     
  checking the certificate's revocation status. There is also a privacy issue since OCSP responders can learn about the users' browsing as requests 
  to the responder are made in response to users' browsing. It also introduces latency to connections, as the responder must be queried before a new 
  connection can be used.

CRLite data comes in the form of a series of cascading Bloom filters, with each filter layer adding data to the one before it. 
Individual Bloom filters have a certain chance of false-positives, but using Certificate Transparency as an oracle, 
the whole Web PKI’s certificate corpus is verified through the filter. When a false-positive is discovered, 
the algorithm adds it to another filter layer to resolve the false positive.
         

---
References:
  [1] https://blog.mozilla.org/security/2020/01/09/crlite-part-1-all-web-pki-revocations-compressed/
  [2] https://blog.mozilla.org/security/2020/01/09/crlite-part-2-end-to-end-design/ 