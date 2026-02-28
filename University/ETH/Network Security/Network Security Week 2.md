---
Tags: exercise 
Created: 2026-01-21 16:58:25
---
(Links:: [[Network Security]])
# Exercise sheet 2: BGP security

### Question 1 
**BGP Routing Attacks.**

> [!question]- We have seen in the lecture that BGP is vulnerable to a multitude of attacks. List two concrete attacks, with a very short explanation on how they work. For full points, also briefly explain their practical impact on society and mitigation strategies.
> **BGP Prefix Hijacking**:
> *Attack*: Attacker advertises IP prefixes that they do not own, leading to traffic intended for those IP addresses being  rerouted through the attacker's network.
> *Impact*: Data interception, DoS.
> *Mitigation*: BGP monitoring, ROA.
> 
> **BGP Route Leaks**:
> *Attack*: AS unintentionally or maliciously announces routes, causing traffic to take unintended paths.
> *Impact*: Suboptimal routing, network congestion, DoS, data leaks.
> *Mitigation*: Strict route filtering and prefix validation, ROA.
> 
> **BGP Session Hijacking**:
> *Attack*: Attacker takes control of a BGP session between two ASes.
> *Impact*: Redirect traffic, intercept data, manipulate routing information, DoS.
> *Mitigation*: BGP monitoring and notification, BGP session authentication.
> 
> **BGP Prefix Deaggregation**:
> *Attack*: AS announces more specific prefixes of an IP range, increasing the size of the global BGP routing table.
> *Impact*: Larger BGP routing table, DoS.
> *Mitigation*: Route aggregation, filter and suppress overly specific routes.
> 
> More high level attacks that use these primitives, such as blackholing (DoS), DNS spoofing, etc are also accepted as an answer.

There have been diverse types of research related to routing attacks. Several papers were introduced during the lecture. What is the difference in the approach of each paper? [^1] [^2]

> [!question]- Briefly describe each paper's attack strategy(vulnerability) and corresponding defense mechanism. 
> # Bamboozling Certificate Authorities with BGP
> 
> This paper shows that routing attacks can be used to obtain fake TLS certificates. Attackers can request a TLS certificate (which we will learn about in more depth in the TLS part of the course), and prove the ownership of the relevant website domain by hijacking BGP during the validation process.
> 
> Two countermeasures are propsed: multi-viewpoint verification and a live BGP monitoring system.
> 
> # RAPTOR: Routing Attacks on Privacy in Tor
> 
> We will learn about Tor in depth in the anonymity section of the course, but the relevant part to understand is that a Tor path comprises of three segments: 1) the client to the Tor entry node, 2) the Tor network itself, and 3) from a Tor exit node to the destination server. This paper shows that BGP routing data on 1) and 3) can be used to deanonymize users, or that one can create spoofed Tor entry nodes to lure and analyze a client's packets.
> 
> The researchers proposed a taxonomy of countermeasures broadly dividing them into: 
> 1. mitigating traffic interception 
> 2. mitigating correlation attacks.

### Question 2
**Securing BGP**

> [!question]- Is BGP safe yet? Visit https://isbgpsafeyet.com/ (by CloudFlare) from your browser and test any ISP you have access to. Try to make the test succeed and paste a screenshot of a successful run. *Hint: perhaps another ISP could work, such as by using Cloudflare's free VPN?*
> Likely not safe yet. Students can use services like Cloudflare's free 1.1.1.1 VPN, or alternatively connect to the SWITCH network (which has RPKI) via the ETH/EPFL VPNs.

> [!question]- Describe three design shortcomings of traditional BGP. What does BGP lack? Focus on the security of routes and the availability guarantees of BGP, and list the currently proposed countermeasures, if any.
> 1. Lack of Authentication: BGP traditionally lacks robust authentication mechanisms,  which means that route announcements are often accepted without verification.  This makes BGP vulnerable to route hijacking and unauthorized route announcements. Countermeasure: ROA.
> 2. Lack of Route Validation: BGP does not inherently validate the authenticity  of received BGP route advertisements, making it challenging to determine  the legitimacy of routes. This vulnerability can be exploited for malicious purposes. Countermeasure: BGPSec.
> 3. Slow Convergence: BGP's slow convergence can lead to network instability  and delays in updating routing information.  This makes it difficult to provide availability guarantees on the internet,  especially during network failures or attacks. Countermeasure: SCION.

> [!question]- What is ROA (route origin authorization)? Briefly explain how it works and against what attacks it is effective.
> "A ROA is a cryptographically signed object that states which Autonomous System (AS) is authorized to originate a particular IP address prefix or set of prefixes."
> 
> ROAs are used in the context of the Resource Public Key Infrastructure (RPKI), in an attempt to mitigate the fact that in BGP, advertised routes are not authenticated.
> 
> The RPKI is run by the Regional Internet Registries (RIRs), which issue certificates for IP prefixes and AS numbers. Holders of an address space can then create ROAs, signed with their certificate, attesting that they are allowed to advertise the prefix in question.
> 
> ROAs are published, and BGP routers that receive a route advertisement can check whether there exists a ROA matching that prefix. If so, the advertisement is valid if the AS number advertising the prefix matches the AS number in the ROA, and invalid otherwise.
> 
> Note that if there is no ROA for a prefix, the validation status is 'unknown', and the AS can still accept the advertisement, allowing for incremental deployment of Origin Verification.

> [!question]- Why is it not possible for a malicious AS to add another AS to a path in BGPSec?
> In vanilla BGP, the path vector is built step-by-step: each AS adds its own AS number in the path when propagating a BGP update.
> 
> In BGPSec, the process is the same, but each AS also includes the AS number the update is sent to, and the information added to the as path is signed.
> 
> A malicious AS 'E', targeting a victim AS 'V', will now need a signature from 'V' for it to be inserted in a BGP path.

### Question 3 
**BGP: TCP RST Attack**
BGP border routers use TCP sessions to reliably exchange routing advertisements, and this can become an attack surface to disrupt BGP routers. Specifically, an attacker sends the spoofed RST packet to close the TCP connection between two border routers, and the two routers delete advertisements learned from each other. At a later time, the two routers establish the TCP session again; however, there could be some delay after session establishment before any data traffic get passed on the link between the two routers.
  
> [!question]- What could have caused this delay, and how would you mitigate the TCP connection reset attack?
> BGP route re-convergence: when a BGP session is reset due to TCP RST attack,  routers exchange BGP updates and relearn routes.
> 
> Route propagation delay: even after the BGP session is re-established, it takes time  for routing information to propagate through the network.
> 
> A possible mitigation is to establish a trust boundary and allow sensitive packets only from trusted sources (eg. edge routers should implement ingress filtering, limit the interfaces the BGP daemon is listening on, etc. )

> [!question]- Imagine that in the previous example, the routers started using BGPSec. Unfortunately, admins haven’t fixed the vulnerability, so the adversary performs the same attack. Now the time between establishment of connections to live traffic jumps to several seconds. Explain why.
> Routers will need to verify all the signatures for the routes they receive, and at they will also need to produce new signature for the outgoing BGP updates.
> The additional delay can be attributed to the public cryptography operations, which can be especially slow on the (limited) hardware many Internet routers run on top.

When making forwarding decisions, BGP routers use longest prefix matching (LPM). For example, if you have both 1.1.0.0/16 and 1.1.1.0/24 in your routing table, you will use the route indicated by the latter entry when sending the packet to 1.1.1.1.

> [!question]- In order to prevent route hijacking, every AS could announce the route for every IP address. Why is this a bad idea?
> Even if we do this, the routes could be aggregated again in surrounding routers. If we presume that it isn’t the case, the routing tables would be too big and impossible to search through efficiently. Furthermore, every time a BGP connection is established, all that data would need to be exchanged between the routers. Finally, without any aggregation the size of routing tables would grow to the size of the whole IP address space: approximately 2<sup>32</sup>, or 4 billion, entries. Routers have to process packets at line rate, and routing-table lookups have to be fast. To achieve the desired rates, tables are implemented as expensive TCAMs (ternary content-addressable memory), and the biggest and most expensive routers have less than 1 million entries available. Therefore, 1 entry per-address is not technically and economically feasible today.

### Question 4 
**BGP: Recent attacks (1)**
On October 4th, 2021, Facebook experienced a global outage of its services,
internally as well as externally. We provide you with articles that should inform
you about the details of the event. In the following, you will answer questions
about the details of the outage.
[Article from Cloudflare (external view, technical details)](https://blog.cloudflare.com/october-2021-facebook-outage/)

[Optional article from Facebook (internal view, high-level)](https://engineering.fb.com/2021/10/05/networking-traffic/outage-details/)


> [!question]- What was the reason for the outage?
> Maintenance work should have measured the capacity of the global backbone but instead took it offline, which was possible due to a bug in the supervising audit software, disconnecting all data centers from each other as a consequence. Facebook's DNS servers disable BGP advertisements if they can't reach the data centers, and in this case the data centers declared themselves unhealthy because of the disconnect.

> [!question]- List any two ways an attacker could have abused this situation where Facebook BGP advertisements are gone.
> If lacking all BGP advertisements:
> - BGP Hijacking, DoS Attacks ...
> If lacking only BGP advertisements for DNS:
> - DNS-specific attacks (e.g. DNS spoofing, Domain Hijacking ... )

> [!question]- Would these attacks also be possible with BGPsec deployed globally? Argue why or why not.
> BGPsec could prevent such attacks. However, BGPsec alone cannot guarantee that  the originating AS was authorized to originate the prefix listed in the route. When used in conjunction with ROAs, BGP hijacking can be effectively defended.
> 
> - Route Origin Authorization ensures that a prefix origination was performed by an AS  authorized to do so. It ensures that the first AS in the path of a received BGP route  is authorized to originate the prefix listed in that BGP route.
> 
> - BGPsec provides Path Validation. BGPsec ensures that whenever a BGP participant receives a BGP route, the router at the start of the path listed in that route announced the prefix listed in the route, and that the path to the originating AS has not been tampered with.

### Question 5 
**Recent BGP Attacks (2)**

Prefix hijacks are the most common attacks in the BGP ecosystem. Attackers exploit 
by accident or by intention the longest matching prefix of BGP to redirect traffic through their AS. 
The list of attacks is pretty long, so we will highlight some of those in this exercise.

A recent BGP attack led to the theft of 1.9 millions worth of crypto from the South Korea platform KlaySwap[1].

[1] <https://medium.com/s2wblog/post-mortem-of-klayswap-incident-through-bgp-hijacking-en-3ed7e33de600>

> [!question]- How did the attackers manage to serve malicious code to KLAYswap clients?
> The attack made use of a standard BGP hijacking. The attacker announced the ownership of the IP belonging  to Kakao and consequently part of the traffic directed to Kakao was now directed to the attacker.

Even if the attacker managed to redirect the clients from the legitimate KLAYswap server to its own, the download happened over HTTPS (this requires a valid TLS certificate).

> [!question]- How could the attacker have managed to obtain a certificate for KLAYswap?
> The BGP hijack was used during the ACME protocol when requesting the certificate for KLAYswap.
> 
> The CA will verify the ownership challenge sending a request to the KLAYswap server, the request will be redirected to the  attacker who would be able to respond successfully.  Notice that ACME assumes that the chellenge request will be directed to the legitimate owner of the service. Attacks on BGP or DNS can undermine this assumption.

KLAYswap is only one of the many incidents related to BPG hijacking.  Read about this attack that happened on the 17th of August. [1] Similarly to the KLAYswap case, the attackers managed to get a bogus certificate for `cbridge-prod2.celer.network`.  Let's look at the BGP hijack that allowed this attack in more details.

[1] <https://arstechnica.com/information-technology/2022/09/how-3-hours-of-inaction-from-amazon-cost-cryptocurrency-holders-235000/>

> [!question]- The attackers announced the ownership of a `/24` prefix, would the BGP hijack have been successful  if they announced the ownership of `44.192.0.0/10`?
> Since Amazon had a valid announcement for the ownership of the /11 subnet, the attacker announcement would have been  more generic and routers would have used the valid one from Amazon.

> [!question]- Amazon uses RPKI and had a ROA for the prefix that was hijacked, so why didn’t RPKI ROA help here? What trick did the attackers use to circumvent this defense measure?
> The ROA used by Amazon allowed announcements with prefixes ranging in size from a `/10` all the way down to a `/24`. As you have seen in the lecture, RPKI only checks that the AS at the beginning of the PATH owns the announced prefix. The attackers just had to prepend the valid AS at the beginning of the PATH in the bogus announcement.

> [!question]- Suppose Amazon set a maximum prefix length to be `11` in the ROA. Would the bogus announcement have been blocked?
> The announcement would be blocked because it announced a prefix longer than the maximum allowed by the ROA.


---
References:

[^1]: Bamboozling Certificate Authorities with BGP <https://secure-certificates.princeton.edu>

[^2]: RAPTOR: Routing Attacks on Privacy in Tor <https://www.usenix.org/node/190965>
