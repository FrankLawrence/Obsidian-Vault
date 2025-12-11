---
Tags: 
Created: 2025-09-30 10:15:59
---
(Links:: [[Network Security]])
**Vision**: high available, efficient communication on global networks with malicious adversaries through formal verification

> [!attention] Non-scalability of Trust
> there is no single entity that is trusted by everyone

- each local grouping (ISD) of ASes has their own local trust roots
	- prevents ASes in other groups from hijacking their ASes
	- organic forming structure
- path-based network architecture
- **The endpoint chooses the path**, by controlling which paths it advertises
- SCION creates path segments at the higher level through the ISD cores
- **With SCION, both paths can be offered**, allowing for higher bandwidth
	- Multipath is important for high availability

## Segment Registration
- Each AS runs their own **local path servers**
	- Sent to core path server, which can be queried globally, from other ISDs

---
References: