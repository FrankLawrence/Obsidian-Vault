---
Tags: 
Created: 2025-04-01 13:20:31
---
# What is DNSSEC
- DNS servers are vulnerable to a broad spectrum of attacks, including spoofing or the interception of private [personal information](https://www.cloudflare.com/learning/privacy/what-is-personal-information/).
	- [DNS spoofing](https://en.wikipedia.org/wiki/DNS_spoofing)/**cache poisoning**: forged DNS data inserted into DNS resolver's cache -> attacker can divert target to other location
	- **DNS tunneling**: Attackers can use other services to pass malware or stolen information into DNS queries

# How it works
**Four categories of DNS servers**:
1. Recursive resolvers (first stop)
2. [Root nameservers](https://www.cloudflare.com/learning/dns/glossary/dns-root-server/): 13 DNS root nameservers overseen by ICANN that choose TLD server
3. TLD nameservers: maintains information of all domains that share common domain extension (e.g. Verisign owns .com TLD nameservers), managed by IANA
4. Authoritative nameservers: contains information specific to domain (e.g. Cloudflare DNS distributes autoritative nameservers using Anycast routing)

# Intro to DNSSEC
- nameservers stores domain and it's respective signature
- checking signature will verify that the DNS record comes from the authoritative name server

Steps for resolvers:
- Request the desired RRset which contains RRSIG record
- Request DNSKEY records containing public ZSK and public KSK which returns RRSIG for the DNSKEY RRset
- Verify RRSIG of requested RRset with public ZSK
- Verify RRSIG of DNSKEY RRset with public KSK

---
References: 
- https://www.cloudflare.com/learning/dns/dns-server-types/
- https://www.cloudflare.com/learning/dns/dnssec/how-dnssec-works/
- https://www.dnssec.net/