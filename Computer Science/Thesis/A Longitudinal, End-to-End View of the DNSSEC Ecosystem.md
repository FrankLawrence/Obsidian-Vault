---
Tags:
  - DNS
  - Security
Created: 2025-03-04 10:14:37
---
# Background
- `DNSKEY` records: public keys -> each zone uses two records
	- Key Signing Key (KSK): produces `RRSIG`s for `DNSKEY` records
	- Zone Signing Key (ZSK): produce `RRSIG`s for all other record types
- `RRSIG` records: signatures of all records of a given type for a certain name
	- Ex: all `A` records of `example.org` use a single `RRSIG`
	- created using private key of respective `DNSKEY`s public key
- `DS` records: Hash of `DNSKEY`s (KYK) signed by parent zone
- DNSSEC PKI relies on expirations for keys

![|600](https://upload.wikimedia.org/wikipedia/commons/5/59/All_active_dns_record_types.png)
# DNSSEC Deployment and Management
- small percentage of popular websites have deployed DDNSEC: max 1.85%
  [[DNSSEC Ecosystem.pdf#page=7&selection=65,6,67,39|DNSSEC Ecosystem, page 7]]
- Cloudflare supports [CDS and CDNSKEY](https://datatracker.ietf.org/doc/rfc8078/) since 2018: https://blog.cloudflare.com/automatically-provision-and-maintain-dnssec/
  [[DNSSEC Ecosystem.pdf#page=7&selection=189,39,189,57|DNSSEC Ecosystem, page 7]]

---
References: [[DNSSEC Ecosystem.pdf]]