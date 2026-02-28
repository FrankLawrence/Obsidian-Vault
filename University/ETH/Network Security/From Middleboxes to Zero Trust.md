---
Tags: #lecture 
Created: 2026-01-12 01:24:33
---
(Links:: [[Network Security]])
# Middleboxes
> [!definition] Middlebox
> Any in-path network element that performs functions other than packet forwarding

- **Firewall**: Block all traffic from IP, subnet or even entire transport layer protocols
- **Intrusion Detection Systems**: 
	- Can look at network layer signals like flow source, message sizes, flags, etc.
	- Very often, paired with **Deep Packet Inspection**
- Middleboxes are considered *harmful*, since they expect all traffic to be TCP/UDP -> other protocols are dropped ("ossification of the internet")
- Inspecting TLS
	- **Break PKI assumption**
	- Add special certificate authority to end device ([[MITM]] attack)
	- e.g. https://help.ui.com/hc/en-us/articles/18068491373975-UniFi-Gateway-NeXT-AI-SSL-Traffic-Inspection
- Middleboxes are sometimes good, but also bad: Violation of E2EE security, increased exposure, performance bottlenecks, worse transparency/debugging and internet ossification.
# Zero Trust
Traditionally, the *LAN* was considered a *safe place*, protected by a firewall. Normal endpoints have *not been designed* to be exposed to the wider Internet, and thus the LAN should stay safe.
In enterprise networks, many firewall rules are set in place to *segment the network as much as possible* ([[VLAN]]). This prevents attackers from moving horizontally, infecting only other devices in the same network.
One type of VLAN is the **DMZ** (demilitarized zone):
- Setting up firewall rules can become hard to manage in large companies
- Complex topology is useless in case of breaches!
- Lateral movement allow attackers to expand their foothold
- Even without considering common modern use cases such as remote work!

![[Middleboxes Enterprise Segmentation.svg|600]]

**Zero Trust** assumes that no part of the network can be trusted.
- Always authenticate and authorize user (based on policies) for each new session -> erase after

> [!example] Beyondcorp
> - Front-end reverse proxies connect to users via [[Transport Layer Security|TLS]], and with **mTLS** to backend servers, providing load balancing and *authentication & authorization*
> - *Identity Provider* used for authentication, and providing certificate
> - *Access Control Engine* used to determine user rights and authorization
> - *Inventory Service* talks with ACE for device registration and maintenance
> - Attacker space mitigated, except for when a reverse proxy is hijacked -> Already has access to most things

- Trust moves from network segments to users and user devices
	- Need managed corporate devices
	- Endpoint detection and response (EDR)
	- Modern Multi-Factor Authentication ([[U2F]], [[Passkeys]]) to prevent user fatigue

---
References: