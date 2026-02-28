---
Tags: 
Created: 2026-01-12 12:35:59
---
(Links:: [[Network Security]])

Until know, when two parties want to communicate securely through [[Transport Layer Security|TLS]], they cannot know if they exchanged public keys are really theirs ([[MITM]]). We can associate long-term identifiers by ...
- Trusting the first key received (Trust On First Use mechanism; TOFU)
- Using [[DNSSEC]] (not widely deployed)
- Using newspaper articles to distribute key (doesn't scale) [^3]

> [!check] Solution: use certificates & [[PKI]]
> **Certificates** certify bindings from cryptographic keys to identities. A **Public Key Infrastructure** is an ecosystem to manage and distribute certificates

# Certificate Authorities
![[WebPKI Certificate Authority.svg|500]]

- Initial connections require *prior trust*, namely **Certificate authorities**
	- Includes private and state-owned companies
	- Browsers have many [[#Public Key Pinning|preconfigured]] trusted CA Certificates [^2]
- Validating domain ownership to CA (all have problems)
	- Sending email from 'admin', 'hostmaster', etc.
	- Contacting domain registrar
	- Calling phone number in DNS TXT record

> [!warning] You cannot create trust out of thin air!

- [[ACME]] automate certificate issuance and renewal
	- Supports fully automated HTTP-, DNS-, and TLS-based challenges

![[WebPKI Certificate Structure.svg|600]]
- Intermediate certificates are used in case *private root key goes offline*, to offload requests, and to **revoke** and replace without cascading failures
- There are two common **trust hierarchies**:
	- *Monopoly*: Single trust root
		- Used by [[DNSSEC]], BGPSEC, RPKI
		- Hard to find a single entity that everyone trusts
		- Single trust root has a control over "kill-switch"
	- *Oligarchy*: Multiple trust roots
		- Used by WebPKI
		- Security is determined by weakest trust root
		- Not all trees trusted -> unverifiable entities

> [!danger]- CA's make mistakes
> - July 10, 2011: An attacker inside DigiNotar’s issues certificate for `*.google.com` 
> - July 19, 2011: DigiNotar noticed intrusion; kept it secret
> - August 28, 2011: Google notices fraudulent certificate is used in Iran  
> - September 4, 2011: 531 fraudulently issued certificates were identified
> 
> DigiNotar was part of “PKIoverheid”, the Dutch government’s PKI 
> - Dutch government asked DigiNotar if their certificates were affected.
>   They say “no” 
> - Dutch government kept trust in DigitNotar’s certificates  
> - Explicitly requested for certificates not to be removed from root stores  
> - Later, this turned out to be false; all certificates were revoked
> 
> [CA Incident Dashboard](https://wiki.mozilla.org/CA/Incident_Dashboard) shows all current CA issues.

## Revoking Certificates
1. We can wait for certificates to expire, but adversaries might be able to change date on victims machine
2. **Certificate Revocation Lists** (CRL)
	- Each CA hosts a file containing a list of revoked certificates
	- Browser downloads CRL, checks if certificate is in it
	- Files can be large!
3. **Online Certificate Status Protocol** (OCSP)
	- Each CA hosts a *service* for checking certificate status
	- Browser queries service for specific certificate

> [!danger] Problems with OCSP
> - *Additional latency* due to checks required before accessing website (~300ms [^1])
> - When OCSP response times out, certificate is accepted by default ("Soft fail") [^4]
> - Privacy issues: OCSP server sees domains you visit

To combat latency and privacy issues, we can employ *OCSP stapling*:
- Web server obtains OCSP response in advance
- Web server serves OCSP response together with certificate
- No OCSP response stapled -> browser performs OCSP lookup
  -> Problem: Adversary can still suppress OCSP response
- OCSP *must staple flag* ensures OCSP response must come with certificate
	- Most browsers ignore "Must Staple" for better user experience
	- Support in Web servers is not great
- OCSP support is being discontinued

![[OCSP Stapling.svg|500]]

Another alternative method is to *patch the browser* to update the set of CA certificates. Firefox uses "OneCRL" to push revoked CA certificates to browsers.

## Certificate Trust Levels
There are 4 types of leaf certificates that a domain can employ:
1. Domain Validated (DV)
2. Individual Validated (IV)
3. Organization Validated (OV)
4. **Extended Validation** (EV): Stricter requirements on validation process
	- Creates less trust when commercial and legal name are not identical

## DNS-based Authentication of Named Entities (DANE)
To distribute public keys, we can add signed record to DNS that specifies
1. Which CA certificate must be used; or
2. Which end-entity certificate must be used (path to trust root verified); or
3. A new trust anchor, outside of default trust store; or
4. A custom end-entity certificate (path to trust root *not* verified)

> [!danger] Problems with DANE
> - DNSSEC is not deployed
> - DNSSEC uses a monopoly trust model -> kill switch
> - Browser does not perform recursive DNS lookup -> How to verify DANE?
> - All the prior problems of OCSP

- The solution is to let the webserver serve DANE chain with cert (DANE stapling) -> same problems as OCSP stapling
- DANE support is small
## Public Key Pinning
- Another method of public key distribution

**HPKP: HTTP Public Key Pinning**:
- HTTP header specifies certificate public key whitelist for domain, which will be used for a year
- TOFU mechanism -> too dangerous
	- HPKP suicide: Losing access to serve results in useless domain for a year
	- RansomPKP: Compromised web server pins attackers public key, which cannot be changed until it expires
- Similar mechanism used today for mobile applications

**Built-In Public Key Pinning** (used today):
- Preload allowed pubkeys / CAs in browser
- Allowed Google to detect 2011 Diginotar attack

## DNS Certification Authority Authorization (CAA)
- Puts information in DNS checked by CAs, rather than by browsers
- CA checks if they are present in domains list of CAs, when request comes for certificate from browsers -> distributes certificate if true
- Allows domain owner to specify which CAs can issue certs
## Certificate Transparency (CT)
In order for people to be able trust and verify CAs, they keep a **public log** of all issued certificates, holding them *accountable if illegitimate certs* are issued.

![[Certificate Transparency.svg|700]]

- *Log Server*: keeps a public append-only log of certificates that have been issued by CAs 
- *CAs*: Requests addition of newly issued certificates, obtains the guarantee in form of a "Signed Certificate Timestamp" from the log server that the certificate will be added, periodically audits/exchanges views of the log with the monitor 
- *Monitors*: Monitors log servers and checks for suspicious/bogus certificates and inconsistencies, potentially informing domain owners and/or CAs 
- *Clients*: Can verify certificate inclusion in the log, exchange information with monitors about the log server status

### CRLite
Shipping a list of all revoked certificates to browser ahead of time is much too large (~ 0.5 GB). *CRLite* cannot do so, but in a much smaller package using [[Bloom Filter]]s:
1. Acquire list of *revoked* certificates (obtained through CRLs)
2. Insert list into Bloom Filter
3. Cross examine against list of *non-revoked* certs (obtained through CT) to get *false positives*
	- False positives are certificates which are not revoked, but seem to be in the 'revoked' Bloom Filter
4. Insert false positives into second Bloom Filter
5. Cross examine FP Bloom Filter against list of revoked certs for false positives
	- False positives are certificates which are revoked, but seem to be in the 'non-revoked' Bloom Filter
6. Repeat until no more false positives in the Bloom Filters, and send all to browsers (10MB full dataset; 580KB updates)
## Qualified Web Authentication Certificate (QWAC)
- QWACs are issued by *government-approved ~~CAs~~ TSPs* (Trust Services Providers) 
- **Browser MUST recognize** QWACs & display identity in "a user-friendly manner"
  -> Massive risk for WebPKI, large protest from industry & academia
- EU Parliament adopted text 
	- QWACs sole purpose: displaying *identity* data
	- QWACs do not limit how browsers establish encryption
- **2 certificate solution**
	- A normal certificate issued by a normal CA
	- A QWAC issued by a TSP
## Downgrade attacks
**HTTP Strict-Transport-Security** (**HSTS**)
- HTTP header telling browser to always load domain over HTTPS
- Chrome and Firefox support HSTS preloading

---
> [!summary]
> - In order for TLS to be useful, we need a trust distribution mechanism
> - WebPKI uses the Oligarchy model, >100 CA certs trusted
> 	- Certificate pinning is too dangerous to use in practice  
> 	- DANE’s reliance on DNSSEC makes it practically unusable (for the web) 
> 	- CT facilitates move from “trust” to “trust, but verify”
> - Long cert lifetimes make revocation necessary  
> 	- CRLs are too resource intensive for in-browser checks  
> 	- OCSP (stapling) is broken  
> 	- CRLite makes it feasible to distribute the web’s complete CRL to browsers
> - We see a trend to shorter and shorter certificates



[^1]: https://www.imperialviolet.org/2012/02/05/crlsets.html

[^2]: https://ccadb.my.salesforce-sites.com/mozilla/CACertificatesInFirefoxReport

[^3]: https://www.coverdrop.org/

[^4]: https://wiki.mozilla.org/CA/Revocation_Checking_in_Firefox
