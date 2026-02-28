---
Tags: 
Created: 2026-01-17 18:22:43
---
(Links:: [[Network Security]])
[[Lecture 11 Application Layer Part 1#Domain Name System|Domain Name System]] is the *phonebook* of the internet, where domains are structured into 4 parts: *root*, *TLD*, *SLD*, *Subdomain*.
DNS was designed to be distributed and scalable, but far too often do we see when Cloudflare goes down, that this is not the case.
- There are recursive resolvers, which only forward the query, but collect all the data, and may even change it

---
References: