---
Tags: 
Created: 2025-06-16 18:10:02
---
(Links:: [[Bachelor Thesis Computer Science VU]])
DNS amplification attacks are a type of distributed denial of service ([[DDoS Attack]]) that make use of open DNS resolvers and nameservers to overload a victims system. An attacker will send many DNS requests to such open resolvers, and using IP spoofing, will redirect the DNS response to it's victim. Such responses are much larger than usual DNS requests as the request type, "ANY", would request all information about a DNS zone. Such responses have been reported to be on average 28 times larger, known as the amplification factor (insert citation), which is what gives it it's name. 
Such attacks are notoriously hard to fight against, as the data often stems from valid resolvers making it hard to distinguish from legitimate traffic. Additionally, such resolvers are often connected via high bandwidth channels to ISP's, allowing for more data flow towards a target.
On top of this, DNSSEC enabled recursive resolvers can provide an even high amplification factor (insert citation), due to additional cryptographic keys being included in the DNS response through Resource Record (RR). 
Less common is querying TLD ANS, however it is harder to block such requests in comparison to recursive resolvers.


---
References:
- https://www.cisa.gov/news-events/alerts/2013/03/29/dns-amplification-attacks