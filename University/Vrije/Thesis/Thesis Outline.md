---
Tags: 
Created: 2025-05-25 05:49:13
---
(Links:: [[Bachelor Thesis Computer Science VU]])

The title of my thesis is the following: "Re-evaluating the Potential for Abusing the DNS Ecosystem". It revolves around (the extent of) the use of [[DNSSEC]] and some problems that may occur. DNSSEC was designed to ensure that the DNS query result is in fact correct. There have been previous studies showing how widespread DNSSEC is used, and to what extent it was hosted correctly[^6]. 

One issue with DNSSEC is the length of the responses (due to the signature keys) can be exploited to be used in a [[DDoS Attack]] called a DNS Amplification DDoS Attack[^1]. When many clients start querying an **open resolver** (usually run by ISP's as well as other organisations like those of [cloudflare](1.1.1.1) or [google](8.8.8.8)), the ISPs infrastructure can be forced to transmit gigabytes of data, due to the DNS servers usually having multi-gigabit connections to the ISPs. In so far, we are interested in the following:
> [!question] What part of the network is most vulnerable?
> By looking at where these **open resolvers** are situated (which country and ISP/AS), we can determine what parts of the internet could be taken down when these are attacked.

> [!question] How many open resolvers use DNSSEC?
> We would like to figure out how many (open) resolvers set the AD flag which requests the DS record from the respective nameserver.

A proposed solution to this would be to disallow an ISP run open resolver to respond to DNS queries from other [[Autonomous Systems]]. This would then usually keep the attack contained to a single ISP where *all clients making the request must reside in* (which is much harder, even with VPNs). 

Along side the problem of open resolvers, we are still interested in how many, and which types of **domains** have DNSSEC enabled. We might for example expect to see that *most government domains have DNSSEC enabled*. We would also like to go so far as to say that more [adult websites](https://github.com/Bon-Appetit/porn-domains/) have DNSSEC enabled than government domains.

> [!info] Checking DNSSEC on a domain
> Although a domain might have a DS record, it could still be wrong, as keys might be outdated.

We are also interested in the **nameservers** of the domains.

> [!question] How many domain owners run their own nameserver?
> Some larger institutions and organisations do not rely on the domain registrar to run the nameserver, instead they run their own. If many domains are configured incorrectly, it may be because the nameserver is misconfigured. 

> [!question] How many local resolvers have been made open
> We are interested in finding resolvers that an ISP advertises to it's customers (which should be local resolvers that are only accessible from within that AS), but that might be configured to be open. For this we would need to use RIPE atlas probes, to find their resolvers, and check if they have been accidentally made open. 

# Introduction
**This section includes the context and motivation behind the work, explicitly or implicitly highlights the main research question(s), provides a high-level explanation of the solution, and describes the contributions.
Context, motivation, research questions, and original contribution could be organized in subsections.**
## \section{Introduction}\label{s:intro}

The Domain Name System (DNS), a system born in the 1980s designed to translate human-readable domains into IP addresses, was never built with security as a primary concern. 
As the Internet grew, so did the number and sophistication of attacks targeting its infrastructure and end users. 
One such attack is distributed denial-of-service ([[DDoS Attack]]) attacks, which aim to overwhelm open services with traffic, have become increasingly common and powerful.
DNS is particularly attractive in this regard because it has the potential of creating responses that are significantly larger than the original queries, often referred to as an \textit{amplification attack}.

There are two main types of DNS servers that attackers typically exploit in these scenarios: open recursive DNS resolvers and authoritative name servers. 
Open resolvers are particularly risky because they respond to any query from the Internet. 
Authoritative nameservers, which serve domain zone information, are also increasingly being targeted directly \cite{noauthor_dns_2019}.
In a typical amplification attack, an attacker sends small DNS queries to public DNS servers but forges the source IP address to make it look like the request originates from the victim (known as IP spoofing \cite{paxson_analysis_2001}). 
The DNS server replies with a much larger response, directing it at the victim, overloading their infrastructure and possibly disrupting a service. 
This is usually done at scale using a botnet (a large amount of end devices that are controlled by the attacker), but is feasible using a single uplink and exploiting routing loops \cite{nosyk_routing_2022}.
Such attacks are notoriously hard to defend against, as the traffic they receive often comes from valid DNS services, making it hard to distinguish and filter them from legitimate traffic.
Amplification attacks using DNS infrastructure have, however, mainly gained popularity after the release of DNSSEC.

DNS Security Extensions (DNSSEC) was developed to prevent the \textit{tampering} of DNS query responses such as those caused by DNS cache poisoning attacks, thus ensuring data integrity.
Ironically, while DNSSEC helps protect users from some attacks, it also introduces new opportunities for others. 
Because DNSSEC responses tend to be much larger due to cryptographic signatures and extra metadata, they are often exploited for amplification attacks \cite{van_rijswijk-deij_dnssec_2014,noauthor_open_nodate}. 
Despite the growing importance of DNSSEC, its adoption in domains and the support for recursive resolvers to verify these responses remains inconsistent. 
Some networks fully support it, while others ignore it entirely. 
Misconfigurations are also common, which only adds to the potential for abuse. 
At the same time, the Internet's DNS infrastructure has become increasingly centralized, with a few large entities handling a significant portion of global DNS traffic. 
While this improves performance and manageability in many cases, it also introduces single points of failure and increases the risk of widespread outages or attacks \cite{salter_todays_2021,noauthor_understanding_2021,noauthor_understanding_2021}.

In this paper, we take a closer look at the current state of DNS infrastructure from a security perspective. Our goal is to understand how DNS infrastructure remains vulnerable, how DNSSEC is being deployed and adopted, and how this affects the Internet's resilience to amplification attacks.

We aim to address the following key research questions.
\begin{enumerate}
    \item How widely are DNSSEC-capable resolvers distributed across Autonomous Systems (ASes), countries and IP address blocks, and how do they compare to general DNS resolver availability?
    \item Which parts of the DNS infrastructure are most vulnerable to being used in amplification attacks?
    \item How does the distribution of (recursive) DNS resolvers relate to DNSSEC resolvers?
    \item How centralized is the authoritative DNS infrastructure for \texttt{.gov} domains.
\end{enumerate}

\textbf{TODO: Insert difference to other literature}

\subsection*{Contributions}
By answering these questions, we contribute updated empirical insights into the robustness of DNS infrastructure in the face of abuse, and can be summarized as follows:
\begin{itemize}
    \item We present an up-to-date snapshot of the DNS landscape, focusing on infrastructure that can be abused in large-scale amplification attacks.
    \item We highlight common misconfigurations in open resolvers and authoritative name servers that can make them vulnerable to abuse.
    \item We analyze the centralization of DNS infrastructure across ASes, IP blocks, and organizations, showing where responsibility—and risk—is concentrated.
    \item We provide a focused analysis on \texttt{.gov} domains, identifying their infrastructure dependencies and potential amplification exposure.
\end{itemize}

---

## \section{Introduction}\label{s:intro}

Attackers often abuse core Internet protocols in unintended ways to magnify the scale of such attacks while hiding their origin. Among these, the Domain Name System (DNS) has emerged as a particularly attractive target.

DNS is a foundational protocol that translates human-readable domain names into IP addresses. Its widespread use, reliance on UDP, and global infrastructure make it a frequent tool in amplification and reflection attacks. In a typical DNS amplification attack, an adversary sends small queries to publicly accessible DNS servers—called \textit{reflectors}—while spoofing the source IP address to direct the much larger response toward a victim. When done at scale, the victim's infrastructure can be flooded with a high volume of DNS traffic, disrupting legitimate services.

A particularly dangerous class of reflectors are \textit{open resolvers}, which are DNS servers that respond to recursive queries from any IP address, regardless of origin. These are typically intended for internal or local use but are often exposed to the wider Internet due to misconfigurations. Another class of amplifiers are \textit{authoritative nameservers}, which provide DNS records for domain names. While not designed to be recursive, authoritative servers—especially those hosting DNSSEC-enabled domains—can also be abused to generate large response payloads.

DNS Security Extensions (DNSSEC) were introduced to address a different threat: the manipulation of DNS responses by adversaries performing cache poisoning or man-in-the-middle attacks. DNSSEC ensures the integrity and authenticity of DNS data by attaching cryptographic signatures to responses. However, these protections come at a cost. The larger response sizes introduced by DNSSEC can significantly increase the amplification factor of a DNS response—making such servers even more attractive to attackers. While DNSSEC mitigates risks to end-user integrity, it simultaneously increases the threat to availability, as it can be co-opted to execute stronger amplification-based DDoS attacks.

Despite the security enhancements it offers, DNSSEC adoption remains limited and uneven. Many networks and operators either do not support it, or deploy it inconsistently. Compounding the issue is the increasing centralization of DNS infrastructure: a small number of organizations and autonomous systems are responsible for a large fraction of DNS traffic. This consolidation, while beneficial for performance and manageability, introduces systemic risks. If key providers are misconfigured or targeted, large portions of the Internet’s naming infrastructure could be affected \cite{salter_todays_2021, noauthor_understanding_2021}.

\textbf{This study revisits the DNS infrastructure through a security lens.} We present updated measurements of open resolvers, DNSSEC adoption, and authoritative nameserver behavior across the global IPv4 space. We focus on identifying misconfigurations and concentration patterns that make DNS infrastructure prone to abuse in amplification attacks. Our work builds on a rich body of prior studies in DNS measurement and abuse detection \cite{rossow2014amplification, nawrocki_far_2021, xu_measuring_2023}, but introduces a novel combination of large-scale active scanning, geographic and organizational attribution, and path-level analysis via traceroute.


## Introduction

While some DNS-based attacks aim to **disrupt the infrastructure** itself, others focus on **deceiving end users**. DNS Security Extensions (DNSSEC) were introduced to improve the authenticity of DNS responses by adding cryptographic signatures, helping mitigate attacks like cache poisoning. However, DNSSEC does not address **availability** (ensuring users can get a response at all) or **confidentiality** (ensuring the content or usage of DNS is private). In fact, DNSSEC's reliance on large cryptographic payloads has created new opportunities for abuse especially in the form of **DNS amplification attacks**, where large signed responses are sent to spoofed IP addresses.

These attacks exploit two main DNS components: **open resolvers** and **authoritative name servers (ADNS)**. Open resolvers are recursive servers that respond to queries from any IP address, making them easily exploitable if misconfigured. Authoritative name servers are responsible for serving DNS zone data, and are increasingly targeted particularly those hosting DNSSEC-enabled zones that generate large responses.

Recent studies have highlighted another concern: the growing **centralization** of DNS infrastructure. A small number of large cloud providers and content delivery networks (CDNs) now operate a disproportionately large share of resolvers and nameservers. This trend increases operational efficiency but also creates potential **single points of failure** and exacerbates the risk of **systemic abuse** or misconfiguration. Prior work has shown that while surface-level DNS infrastructure may appear distributed, much of the actual resolution and data hosting is concentrated among a handful of entities, undermining the original federated design of the DNS.

## \section{Introduction}\label{s:intro}

One of the most damaging and persistent forms of such attacks are Distributed Denial-of-Service (DDoS) amplification attacks, which exploit the open nature of DNS and the statelessness of the UDP protocol.

In these attacks, an adversary sends small DNS queries to publicly accessible DNS servers—known as \textit{open resolvers}—but forges the source IP address to appear as though it came from a victim. These DNS servers then send significantly larger responses to the spoofed address, overwhelming the target’s infrastructure. This form of reflection and amplification can generate large volumes of traffic with minimal attacker effort. Although typically carried out by botnets, even single uplink attacks are feasible through IP spoofing and recursive query abuse \cite{nosyk_routing_2022}. Since responses come from legitimate servers, they are difficult to filter at the victim end.

DNS Security Extensions (DNSSEC) were introduced to help protect users against attacks like DNS cache poisoning by allowing resolvers to verify the authenticity of DNS responses. It does so by attaching cryptographic signatures (RRSIGs) to DNS records, validated through a chain of trust using DNSKEY and DS records. While DNSSEC improves integrity for end users, it does not provide confidentiality or ensure service availability. Ironically, its use can facilitate larger amplification attacks, as DNSSEC responses are often substantially larger than unsigned responses due to their additional cryptographic overhead \cite{van_rijswijk-deij_dnssec_2014,noauthor_open_nodate}.

Two main types of DNS servers are typically exploited in such scenarios: recursive resolvers—especially those misconfigured to be open—and authoritative name servers. The former answer queries for any client on the Internet, while the latter serve zone data for specific domains. Attackers increasingly target authoritative name servers directly, particularly those supporting DNSSEC, since they generate large responses that are ideal for amplification \cite{noauthor_dns_2019}.

Despite its security goals, DNSSEC adoption remains inconsistent. Many networks do not deploy it at all, while others do so only partially or with misconfigurations. At the same time, DNS infrastructure has become more centralized. A small number of providers—primarily content delivery networks (CDNs), cloud services, and ISPs—now manage a substantial portion of the global DNS traffic. While this centralization can improve performance and manageability, it also introduces single points of failure and increases the risk of systemic exploitation or disruption \cite{salter_todays_2021,noauthor_understanding_2021}.


# `fas:Check` Background
**This section is optional. If needed, it provides the necessary context to help the reader understand the remainder of the thesis.**

\section{Preliminaries}\label{s:background}

In the following section, we introduce foundational concepts which aid in understanding the security issues explored in this work.
We begin by reviewing the role of the DNS protocol and the types of attacks that they are most commonly involved in, followed by a description of DNSSEC, and its security implications.
We conclude with a discussion on what it means for DNS infrastructure to be centralized, and misconfigurations that make it vulnerable to abuse.

## `fas:Check` \subsection{DNS, DNSSEC and public infrastructure}
Domain Name System, is a critical protocol responsible for translating human-readable domains (e.g. \texttt{google.com}) into IP addresses used by machines.
It plays a central role in nearly all web, email, and cloud services.
DNS makes use of a hierarchy of resolvers and authoritative name servers, often maintained by ISPs, cloud providers or dedicated third-parties.

For many years, DNS has been a frequent target of abuse.
Attacks on DNS generally fall into two broad categories: 1. attacks on end users, such as cache poisoning or domain hijacking, which aim to redirect traffic; and 2. attacks leveraging DNS infrastructure, such as reflection and amplification-based DDoS attacks, which use misconfigured servers to overload a victim with traffic.

In an attempt to mitigate response manipulation, such as forged DNS responses, DNS Security Extensions (DNSSEC) was introduced. 
One example are cache poisoning attacks, where an attacker can mask themselves to appear to be an authoritative name server, responding to DNS queries, whilst providing a falsified IP address (one that usually guides the victim to their own site). 
DNSSEC ensures authenticity by appending digital signatures (RRSIG records), validated by a chain of trust that runs to the root domain using DNSKEY and DS records. 
DNSKEY records are used to sign and verify zone, while DS (Delegation Signer) records contain hashes of the DNSKEY record and are stored in the parent zone (which allows for the chain-of-trust model). 
\textit{Recursive DNS resolvers} (RDNS), the first step that devices query to translate domains, retrieve the relevant records from the \textit{authoritative name server} (ADNS) of the domain in question. 
It is their job to verify these records by following the chain of trust, however not all resolvers support this, and it's deployment is continuing to be adopted and promoted \cite{chung_longitudinal_2017,clouflare_dns}. 
Although mitigation strategies for cache poisoning are well-established, another type of attack continues to be widely employed even today.

## `fas:Check` \subsection{Reflection and Amplification Attacks}
The oldest known DDoS amplification attack is the SMURF attack, and originally made use of the ICMP echo requests sent to network broadcast addresses with spoofed source IPs. All devices on a network would reply to the victim, with packets larger than those sent originally.
Much like SMURF attacks, DNS amplification attacks utilize the inherent danger in the UDP protocol to spoof the source address, using \textit{open} RDNSes and ADNSes to overload a victim's system.
Open DNS resolvers are crucial infrastructure in such attacks, as they respond to any IP address regardless of origin AS.
For their role in such attacks, we often refer to recursive DNS resolvers and authoritative name servers simply as \textit{reflectors}.
The attacker sends a small request with a falsified source IP (that of the victim), prompting the reflector to respond with a larger reply, redirecting traffic towards the target. 
The efficiency of such an attack is measured by the \textit{amplification factor} - the ratio between response and request size \cite{rossow_amplification_2014}.

Among UDP-based protocols, DNS remains a favourite due to its global distribution, prevalence of open RDNS, and optional features like EDNS0 and DNSSEC that increase response sizes \cite{anagnostopoulos_large-scale_2022}. Amplification factors vary by query type and configuration, with DNSSEC-signed zones producing some of the largest responses \cite{van_rijswijk-deij_dnssec_2014}.
DDoS attacks that abuse DNS are some of the most well known and often cause attacks that exceed hundreds of Gbps in traffic volume in observed incidents \cite{matthew_ddos_2013}. 
To reduce the impact of such attacks, best practices recommend rate limiting (e.g., RRL for BIND9), access control lists to restrict recursive queries, and aggressive deployment of SAV \cite{kuhrer_exit_2014}. 

Despite its many benefits, DNSSEC unintentially increases the size of DNS responses due to its cryptographic payloads. 
When paired with other records (such as those provided by EDNS0), responses have been reported to reach amplification factors up to 98.3 using name servers and custom domains \cite{rossow_amplification_2014}. 
Such responses are much larger than usual DNS requests as the request type, "ANY", would request all information about a DNS zone. 
As a result, DNSSEC-enabled recursive DNS resolvers can server as reliable reflectors in DDoS attacks.
Although RDNSes are maintained by ISPs, hosting providers or enterprises for legitimate internal use, \textit{misconfigurations} such as poor implementation and a lack of security mechanisms expose them to the public Internet as open RDNSes.
In some cases however, organizations may intentionally deploy these services in public for their customers (like CDNs such as Cloudflare and Google).
Attackers usually need to keep a list of preferred reflectors, which can be cumbersome due to many resolvers becoming inaccessible due to IP address churn \cite{reducing-impact}. 
This has, however, become trivial, with many projects routinely running scans for services on port 53 across the IPv4 space \cite{noauthor_open_nodate}.

Due to the challenges involved in deploying and maintaining the necessary components DNSSEC adoption remains limited \cite{chung_longitudinal_2017}. Some argue that DNSSEC deployment is essential, while others question its operational feasibility \cite{chung_longitudinal_2017,cloudflare_dns}.
Because of their potential for misuse, open DNSSEC resolvers are especially problematic, and the most effective way to mitigate such disasters is by reducing the amount of open (DNSSEC) resolvers available to an attacker. 
A common mitigation strategy is Source Address Validation (SAV), which blocks IP spoofing at the network edge.
Yet, as Nosyk \etal show, outbound SAV is still not universally deployed across ASes, with more than 75\% of ASes deploying it in the year 2022.
\cite{nosyk_closed_2023}. Furthemore, Anagnostopoulos \etal \cite{anagnostopoulos_never_2018} demonstrated that even authoritative name serversw, and not just resolvers, can be exploited in relfection attacks if improperly configured.

## `fas:Check` \subsection{DNS Infrastructure Centralization}
While DNS was initially designed as a decentralized system, with distributed authorities and resolvers across networks, recent studies such as Doan \etal reveal centralization becoming the new norm, with much of it being attributed to few "big players" \cite{doan_evaluating_2021}. 
Public RDNS, such as those offered by Google (8.8.8.8) and Cloudflare (1.1.1.1), now serve large proportions of global DNS traffic. 
While centralization can offer performance and security improvements, it also creates single points of failure and governance concentration and raises privacy concerns.
Centralization here refers not to geographic location, but to the \textit{logical dependence} on a handful of operators for DNS resolution and zone hosting.

Nawrocki \etal further investigate open DNS resolvers, revealing that 26\% of what we perceive as being open DNS infrastructure, are actually forwarders that relay queries to a small number of upstream resolvers \cite{nawrocki_far_2021}. 
Over 90\% of these forwarding resolvers are downstream of fewer than 5\% of indirect resolvers. 
This reflects a deep concentration in the \textit{effective} resolution infrastructure, despite surface-level diversity.
In addition, these forwarding resolvers can obfuscate traceability, as they often spoof return addresses to appear as the original querier, which complicates attribution \cite{nawrocki_transparent_2021}. 
This finding challenges previous assumptions made by Anagnostopoulos \etal, who expected forwarders to respond to the source \cite{anagnostopoulos_large-scale_2022}.

The cumulative effect of centralization, misconfiguration, and lack of universal SAV creates a DNS ecosystem that remains vulnerable to exploitation at scale. 
In the next sections, we explore how these vulnerabilities with empirical data gathered across the Internet.

# Methodology
**In this section, you would provide a description of: the research strategy (well-formulated and justified); the data collection and data analysis methods; and a clear link explaining how research strategy and methods are suitable to answer the research question(s). Include a figure providing a overview of the whole thesis in terms of research strategies/methods, RQs, inputs and outputs. An example is given in Figure \ref{fig:method}.** 

![[Thesis Methodology Pipeline Diagram.canvas|Thesis Methodology Pipeline Diagram]]

## \subsection{Open resolver identification} \label{ss:method-open-resolver}

To identify open resolvers, a full IPv4 scan was conducted using ZMap from two vantage points — Amsterdam and Utah — to filter out local resolvers. 
The network wide scans were conducted on the 20th of April and 17th of June respectively. 
Any device responding to an tcp syn packet on port 53 was marked as a potential open resolver \footnote{By default, zmap uses the \textit{tcp\_synscan} probe module.}. Testing for each IP address was only done once.
For each IP address, we queried for the A record of google.com. 
DNS responses were analyzed to determine recursion availability (RA flag) and DNSSEC support (via AD flag and presence of RRSIG/DS records). 
Country-level identification and mappings to ASNs were collected using whois and geoiplookup queries and using the RIPEstat API respectively. 
To use the RIPEstat API, we asked for permission to query more than 1000 times per day, and use a custom tag for queries.
RIPE derives the AS from BGP data collected by their routing information service.
Probing of the IPv4 addresses was done so in a random order to prevent raising rate limits from ISPs.
In addition, we configured a webserver with details on the project and contact information\footnote{http://130.37.198.76}.
In order to find authoritative namesevers, we used openintels list of the tranco top 1 million domains, which contains all configured authoritative nameservers for each domain.
In addition, we made use of the \verb|.gov| TLD zone (note the measurement only covers US Federal domain names included in the list obtained from the US Federal Government open access API). The intersection of this list and our list of open resolvers obtained through an internet wide scan gave us a dataset with which we could estimate the distribution of recursion enabled nameservers.
To map nameservers to open DNS servers, we queried cloudflares DNS for the rDNS record (TPR) of each IP that we had collected.

How this helps with answering the original questions: this lets us group the findings into different ways to illustrate how centralised the system is. It will both take into account geological (countries) and "network" (AS and IPv4 address spaces) types of centralisation.

\subsection{Traceroute Analysis} \label{ss:method-traceroute}
To understand network paths and potential chokepoints, traceroutes were conducted from two locations: One machine was located in Vrije University, another was deployed via Cloudflare in Utah. Both scanned routes to all collected public dns resolvers as described in \ref{ss:method-open-resolver}. Intermediate hops and their AS affiliations were recorded. Frequent AS occurrences were identified to determine path convergence and centralization. 

After collecting all traceroutes, we seperated each hop, marking them with the corresponding index that they appeared in the traceroute. For each IP we queried the RIPEstat API like before to retrieve the corresponding AS.
In addition, we normalize the AS count by each traceroute, so we only count an AS once for single traceroute (where it might occur more than once). 
E.g. A traceroute to google's 8.8.8.8 resolver would not count the AS of google as an additional AS (since it's obvious that the AS would be in that traceroute, and thus should not be counted).

## Active Scanning of Resolvers
- **Open Resolver Discovery**: Scanned entire IPv4 address space with zmap from two locations in a random order as described by Durumeric et al. on 24 June 2025
	- Consists of Open resolvers, transparent forwarders, and authoritative nameservers
- Filtered out local resolvers by taking the intersection of both
	- Looked for all ips that responded to ping on port 53 and checked for RA flag in response
	- Tested resolution of google.com with dig against all IPs
	- Tested DNSSEC by retrieving RRSIG signature of root server and checking for AD flag bit being set with `bash dig +dnssec @resolver_ip`
- Traceroute
	- Traceroute from two locations: `130.37.198.76` and `128.110.219.102`
- AS: Retrieved via RIPEstate API directly after having tested for recursiveness of resolvers
- Geo Location: Retrieved from `whois` and geoiplookup tool directly after having tested for recursiveness of resovlers
- Onwer: Retrieved from `whois` and using fuzzy matching on specific fields (OrgName, Organization, org-name, org, desc, owenr)
- Made a website available for them to see the test I'm running and left contact information [^10]

## Traceroute path measurements

To acquire a list of open resolvers (that respond to us), I used a simple script that uses zmap to map the entire ipv4 address space for dns servers. I would then query to all of these servers using the `dig` command to resolver google.com. By running the same script from another IP outside of my AS and country (utah instead of amsterdam), i then took the intersection which gave me all open resolvers, since I could have had local resolvers in the first query. To test if they were recursive resolvers, I checked for the RA (recursion available) flag, and i could mark those which are authoritative nameservers by looking if the AA (Authoritative answer) flag was set. I then tested DNSSEC capabilities by looking at if they could give me the RRSIG signature of the root server (`.`) with `dig +dnssec @resolver_ip`. For each IP, i got the respective ASN and AS name after testing that it was a recursive resolver. The test was carried out on the 2025-04-20. I retrieved the geolocation of each IP from a whois query and merged it with geoiplookup. Owners of the DNS servers were retrieved from the whois query and fuzzy matched together.

## \section{Methodology}\label{s:method}

To investigate the structure, centralization, and deployment strategies of the global DNS infrastructure, we employed active Internet-wide scanning, DNS record analysis, traceroute probing, and entity correlation across multiple vantage points. Our methodology is designed to answer the research questions introduced in Section \ref{s:intro}, with particular attention to identifying recursive and authoritative DNS reflectors, analyzing their distribution, and assessing potential amplification surfaces.

The study distinguishes between several DNS server types:
\begin{itemize}
    \item \textbf{Open resolvers:} Recursive DNS servers that respond to any IP on the Internet.
    \item \textbf{Local resolvers:} Recursive servers that only respond to queries from their own network or trusted clients.
    \item \textbf{DNSSEC resolvers:} Open resolvers that support DNSSEC validation, evidenced by DNSSEC-related flags and records.
    \item \textbf{Authoritative name servers (ADNS):} Servers responsible for a domain's zone data, queried via NS records.
\end{itemize}

We describe each measurement component in the following sections.

\subsection{Open Resolver Identification}\label{ss:method-open-resolver}

To identify open recursive DNS servers, we conducted active scans across the full IPv4 space from two geographically distinct vantage points:
\begin{itemize}
    \item A host in Amsterdam at Vrije Universiteit Amsterdam (VU).
    \item A cloud-hosted server located in Utah, U.S. (via Cloudlab).
\end{itemize}

This dual vantage point strategy allows us to distinguish between open DNS resolvers and those configured for local use only. 
A DNS server responding from both locations is considered an open resolver. 

\paragraph{Scanning Procedure.}

We used ZMap to perform a full IPv4 TCP SYN scan on port 53, which is commonly used for DNS services. 
\footnote{By default, ZMap uses the \texttt{tcp\_synscan} module.} Scans were conducted on April 20 and June 17, 2025, from Utah and VU, respectively.
To avoid reduce the chance of overwhelming specific networks, IPv4 addresses were probed in a randomized order. 
We hosted an informational webpage with contact and project details at \url{http://130.37.198.76}, adhering to best practices for ethical Internet-wide measurements, as described by Durumeric \etal \cite{durumeric_internet-wide_2014}.

For each responsive IP address, we issued a DNS query for the A record of \texttt{google.com}. From the responses, we classified servers based on:
\begin{itemize}
    \item \textbf{RA (Recursion Available) flag:} Indicates that the server performs recursive resolution.
    \item \textbf{AD (Authentic Data) flag and RRSIG/DS records:} Indicates DNSSEC validation support.
\end{itemize}

This allowed us to identify recursive resolvers, and among them, those with DNSSEC support.

\paragraph{AS and Country Attribution.}
We used RIPEstat's API to map each IP address to its corresponding Autonomous System (AS) and country. 
RIPEstat uses BGP data from its routing information service to provide current AS-level information. 
To handle the volume of lookups required, we requested explicit consent to use their API with an increased bandwidth limit and used a custom project identifier in all queries. 
GeoIP information was retrieved using the \texttt{geoiplookup} utility and cross-verified with country-code data of WHOIS queries.

\subsection{Authoritative Name Server Collection}

To understand the distribution of authoritative DNS infrastructure, we used two domain lists.
The Tranco Top 1M list \cite{LePochat2019} provides a stable ranking of popular domains, and the \texttt{.gov} TLD zone from the U.S. Federal Government's open access API.
Both of these lists were gathered by measurements of OpenINTEL \textbf{(insert citation)}.
To understand the operator centralization, we compiled a list of ADNS from the NS records of all \texttt{.gov} domains. 
For our open resolver dataset, we reverse-mapped IPs to PTR records using Cloudflare's DNS service, grouping them by organizational domain.
To identify potential overlaps between authoritative and recursive infrastructures, we intersected these two lists. 
This does not guarantee the same IP acts in both roles, but it does reveal shared operational ownership, which has implications for attack surface.

\subsection{Traceroute Analysis}\label{ss:method-traceroute}

To study the network paths taken by DNS traffic and assess the role of intermediary ASes, we conducted traceroutes from both scanning locations (VU and Utah) to every open resolver identified in Section \ref{ss:method-open-resolver}. Traceroutes were performed using the ZMap scan setup described in \ref{ss:method-open-resolver} 

For each traceroute:
\begin{itemize}
    \item We parsed every hop's IP address and used RIPEstat to map it to its AS.
    \item The source and destination resolver's AS were \textit{excluded} to avoid biases.
    \item ASes were deduplicated within each traceroute to prevent multiple counts of the same network.
\end{itemize}

We then aggregated AS appearances across all traceroutes to identify which ASes frequently occur in the middle of paths. 
This step helps us understand how dependent DNS resolution paths are on a small number of transit ASes. 
By calculating a Gini coefficient over AS appearance frequency, we can quantify the centralization of routing infrastructure in reaching DNS services.

\subsection{Entity Grouping and Centralization Metrics}

To analyze structural centralization, we grouped IP addresses and resolvers according to five dimensions:
Autonomous System (AS), Country, and IPv4 address prefixes (/8, /16, and /24). For each group, we computed:
\begin{itemize}
    \item Number of unique entities (e.g., unique ASes)
    \item Maximum, mean, and median number of resolvers per entity
    \item Gini coefficient to quantify distribution
\end{itemize}

This allowed us to ascertain relevant centralization habits in various logical groupings, helping us in determining fairness of distribution, and risks related to attack surface concentration.

\subsection{Ethical Considerations}

This research involved scanning Internet infrastructure at large scale, which can raise operational and ethical concerns. To mitigate these, we followed community best practices:
\begin{itemize}
    \item Our scans used legitimate DNS queries, not malformed or exploitative payloads.
    \item Scanning was randomized and rate-limited to avoid overwhelming specific networks.
    \item We provided contact information and opt-out instructions via a public-facing website.
    \item All data collection and storage complied with GDPR regulations and institutional research ethics guidelines.
\end{itemize}

\paragraph{Summary.}
Our combined methodology offers a multi-faceted view of the DNS ecosystem: from protocol behavior (recursion, DNSSEC) and infrastructure ownership (AS, PTR records), to routing dependencies and organizational control. This approach enables us to quantify both the



# Study Execution
**Discuss the execution of your project, the results you obtained, and how they help in addressing the claims you made in the introduction. 
This section can be customised to the specific thesis project. For instance, it can be split in more than one section, with revised titles.
You may also use the evaluation results in this section to justify your design choices or assess the contributions of different aspects  of your design towards the overall goals.**

We theorize that more DNS resolvers locations provide a larger attack vector for a wrongdoer. 
In order to investigate this claim, we look at the data gathered from the port scan and identify the AS, country, and IPv4 spaces in which they are located. 
We then identify the biggest players in hopes of finding misconfigurations and proposed solutions. 
This clearly indicates that DNSSEC resolvers are much less centralized, which is great since it's harder to take it down fully if attacked.
Despite low absolute numbers, DNSSEC resolvers show better distribution. This may suggest that when DNSSEC is deployed, it is often in isolated or smaller deployments, rather than in bulk by large providers.
Centralization implies a resilience issue: targeting just a handful of ASes could impact a disproportionate number of resolvers.

#### Insight: U.S. and Hosting Providers Dominate
- Unified Layer, OVH, Hetzner, Akamai — these are all either U.S. or EU-based hosting providers.
- **Implication:** Open resolvers are largely hosted in data centers rather than ISP networks. That means misconfiguration in hosting environments is a key driver of availability of open resolvers.

## Statistics
- Responses
	- Zmap responses to ping (TCP synack packets) on port 53 from utah: 7.535.563
	- Zmap responses to ping (TCP synack packets) on port 53 from amsterdam: 1.315.829
		- Intersection: 1.119.248
		- 21.832 responded to dig query (open resolvers and forwarders)
	- Open resolver project reported 11.345.686 total responses to udp/53 probe on 2020-05-24
- Amount of dig queries with REFUSED: 
	- Open resolver reported 3.608.236
- Amount of dig queries with `ra` flag set: 21.832
- Amount of dig +dnssec queries with RRSIG signatures and ad flag set: 1210
- Amount of nameservers in the tranco top 1 mil that are recursive resolvers
	- **Requires rDNS lookup of dns resolvers**
- Distribution of nameservers from .gov domains based on country and AS (map)
- Distribution of resolvers on /24 addresses 
- AS that do not have open resolvers
- Categories of Operator types that run open resolvers (commercial, eduaction, government, ...)
	- **Requires rDNS lookup of dns resolvers**

Details on reserved address blocks: https://www.iana.org/assignments/iana-ipv4-special-registry/iana-ipv4-special-registry.xhtml and https://en.wikipedia.org/wiki/List_of_assigned_/8_IPv4_address_blocks
To calculate how many /8, /16, and /24 address blocks can be distributed, we start with the total number of IPv4 addresses and then exclude the reserved address blocks.

- Num Reserved /8: 35
  Num Unreserved /8: 256 - 35 = 224
  Num /8 with >1 resolvers: 204 (91%)
- Num reserved /16: $35\cdot 256 + 64 + 16 + 1 + 1 + 2 = 9044$
  Num unreserved /16: 65.536 - 9044 = 56.492
  Num /16 with >1 resolvers: 8398 (14.9%)
- Num reserved /24: $9044 \cdot 256 + 1 + 1 + 1 + 1 + 1 + 1 + 1 + 1 = 2.315.282$
  Num unreserved /24: $16.777.216 - 2.315.282 = 14.461.934$
  Num /24 with >1 resolvers: 23694 (0.16%)
## \section{Study Execution} \label{s:execution}

### \subsection{Open recursive DNS reflectors}
DNS resolvers are typically deployed by ISPs, hosting providers, and other institutions which Yazdani \etal. describe as \textit{hypergiants}, to serve users in a controlled manner \cite{yazdani_swamp_2024}. 
However, in practice, misconfigured resolvers may respond to queries from any IP address, unintentionally acting as open resolvers. 
Without any rate limitations or validation mechanisms in place, they are a valuable asset for attackers seeking to launch amplification attacks.

Our active DNS measurement scans (see Section \ref{sec:method-open-resolver}) identified 7,535,563 and 1,315,829 open services running on port 53 from the Utah and VU vantage points, respectively.
Their intersection yielded 1,119,248 common IP addresses. 
This aligns reasonably well with previous work; for example, the Open Resolver Project reported 11.3 million responses to UDP/53 probes as of May 2020, showing a gradual decrease in publicly accessible services.

Among the measured resolvers, 21,713 responded with the Recursion Available (RA) flag set, indicating full recursive resolution capability. 
We identify these reflectors as open recursive resolvers, which we use in our centralization and DNSSEC capability analysis.

To assess the concentration of these resolvers across network entities, we calculated the Gini coefficient for Autonomous Systems (ASes), countries, /8, /16, and /24 IPv4 network blocks. 
A higher Gini coefficient indicates more centralization. Table \ref{tab:gini} presents the comparison between all open DNS resolvers and those that support DNSSEC between the aforementioned entities.

We found that the 21,713 open DNS resolvers were spread over 3,792 ASes and 153 countries, while the 921 DNSSEC-enabled resolvers were distributed over 513 ASes and 92 countries. 
Despite the smaller sample size, DNSSEC infrastructure clearly shows a more diverse geographic and network distribution.
In particular, the top AS (Unified Layer) alone accounted for 1,469 resolvers. 
This suggests a skewed distribution in which few organizations dominate most of the resolver infrastructure.

As expected, Gini coefficients decrease as we move to finer address block granularity. 
This is expected behavior, as smaller address blocks naturally have fewer resolvers, decreasing the likelihood of a high Gini coefficient.
The only exception is AS-based centralization, which remains higher than even /8 networks (0.7582 vs. 0.5688), indicating that ownership, rather than just address allocation, plays a central role in shaping DNS infrastructure. 
DNSSEC resolvers exhibited significantly less centralization across \textit{all} metrics, with a Gini coefficient of only 0.3930 for ASes.
By far the most uneven distribution lies with the geographical location, with a Gini coefficient of 0.8696 and 0.7051 for DNS and DNSSEC-capable resolvers respectively. 
Table \ref{tab:dns_dnssec_entities} shows the United States, which owns 30\% of all open resolvers, and many EU countries are the most impactful.
This reflects the concentration of hosting and cloud providers in North America and Europe, which further validates similar findings by Xu \etal \cite{xu_measuring_2023}.

The cumulative distribution function in Figure \ref{fig:cdf-as} reinforces these observations. 
Approximately 20\% of ASes host over 83.5\% of all open resolvers and 69.2\% of DNSSEC resolvers, underscoring the concentration of DNS control in a small set of networks.
Additionally, the largest 1622 ASes are responsible for 90\% of all open resolvers, as opposed to 2680 for DNSSEC resolvers.

Table \ref{tab:dns_dnssec_entities} further supports this view at an organizational level. Unified Layer, OVH SAS, and Hetzner Online collectively account for over 17\% of DNS resolvers. 
OVH is \textit{also} the top host for DNSSEC resolvers (5\%), followed by Comcast and Chunghwa Telecom. 
However, the shares here are much lower and flatter, indicating less concentration.
The results of Table \ref{tab:dns_dnssec_entities} also suggest that even with larger entity groups such as /16 address groups, there are a few examples that especially stick out.
We found some major groups, and who they belong to:

Moreover, when comparing median to maximum resolvers per entity, it becomes apparent why AS, /8 and /16 networks are so centralized. With most AS and /16 having 1 resolver, the largest entities host up to 1469 and 413 resolvers respectively.
The strong presence of the U.S. in resolver infrastructure is apparent from table \ref{tab:dns_dnssec_entities}. We find this to be consistent with other measurement studies, including Xu \etal \cite{xu_measuring_2023}. 
Large hosting providers and ISPs such as OVH, Hetzner, and Comcast dominate both DNS and DNSSEC infrastructure.

Table \ref{tab:open_resolver_ptr} presents the results of the reverse DNS lookups of all identified open resolvers.
The majority of resolvers with meaningful PTR records belong to a small number of well-known hosting providers, including Endurance International Group, Akamai Technologies, and Hetzner Online. 
Combined, the three largest organizations are responsible for over 7\% of all open resolvers. 
Interestingly, many cloud platforms, including Amazon Web Services and Google Cloud, host their own open resolvers, in contrast to traditional ISPs such as HINET and Alliance Broadband which appear far less frequently. 
This shows that most publicly available resolvers are run by enterprises for their customers,  “intentional deployment mostly reside in datacenter networks” [Yazdani et al., 2024, p. 4].
This strengthens our findings on centralization from Gini coefficient analysis, showing that hosting environments are the primary source of resolver infrastructure.
This may indicate that large IP providers have largely adopted security practices to restrict resolvers from public access.

Summarized, the combined findings from Gini metrics, organizational ownership and PTR record data suggests that DNS infrastructure is still very centralized, creating systemic points of failure.
Specifically, a small number of large commercial hosting providers are responsible for most open resolvers.
This however, is really only the case for non DNSSEC resolvers, as adoption remains limited, and thus centralization is not as common.
Since hosting providers contribute disproportionately to the amplification surface, targeted mitigation strategies such as SAV could yield significant security benefits.

### `fas:Check` \subsection{Name server Centralization} \label{ss:execution-name-server}
- How many (distinct) .gov domains exist: 12,349
- How many name servers manage these domains: 6,808
- How many of tranco are open resolvers: ~~1,321~~
- How many of gov are open resolvers: 
- How many entities control .gov name servers: 1530 (distinct ns_address domains)
	- Which are the largest entities that control .gov name servers?
	- How many domains do they control?
	- How many of the ns_addresses domains appear in dns_resolvers: 73
		- How many .gov domains do these control: 4044 -> many .gov domains simply use popular CDN operators for their name servers
- Is there a centralization of nameservers of .gov domains based on ~~country, AS and~~ Owner

Name servers are another critical part of the DNS ecosystem that can be abused for reflection attacks configured incorrectly. 
Prior studies have shown that \verb|.gov| domains are used most frequently in amplification attacks due to their high adoption rate of DNSSEC \cite{nawrocki_far_2021}, are particularly vulnerable when they lack proper response rate limiting (RRL). 
For example, in BIND9, a popular DNS server software, RRL is not enabled by default, allowing attackers to exploit DNSSEC-enabled zones for amplification. 

To asses the configuration landscape and centralization of authoritative DNS infrastucture in government domains, we analyzed all \texttt{.gov} domains provided by OpenIntel's dataset on \textbf{data}.
In total, we identified 12,349 distinct \texttt{.gov} domains, which are served by a total of 6,811 unique ADNSes.
Interestingly, 3,554 of these name servers serve more than one domain, meaning that over 93\% (11,554) of all \texttt{.gov} domains share their name server infrastructure with at least one other domain.
This indicates a reliance on shared systems, which are often managed by third-party service providers.

In order to understand the overall ownership and management of these servers, we grouped them based on their PTR records, revealing the organizational domains.
This yielded 1,530 distinct PTR-based ADNS domains, of which the most popular of them are listed in \ref{tab:gov_nameserver_ptr}.
As shown, Cloudflare alone provides authoritative name service for 2,741 \texttt{.gov} domains, accounting for 22.2\% of the total.
Other major providers include GoDaddy, Amazon, Microsoft, Akamai, and DigiCert. In total, the top 10 providers are responsible for serving over 7,000, more than half, of all government domains.

To explore the amplification potential of these infrastructures, we cross-referenced our open resolver dataset (see Section~\ref{ss:execution-open}) with the PTR records of the \texttt{.gov} authoritative name servers. 
While determining whether a single IP serves dual roles as both recursive and authoritative server proves challenging, we discovered that 73 domain-level PTRs appear in both datasets.
These overlapping PTRs indicate that 73 organizations operate both authoritative name servers for \texttt{.gov} domains and open resolvers, although not necessarily on identical IP addresses. 
Together, these 73 shared organizations handle the authoritative DNS infrastructure for 4,044 \texttt{.gov} domains, representing roughly one-third of the total. 
This overlap suggests that many government domains depend on providers whose broader infrastructure also includes open recursive resolvers, potentially increasing exposure to DNS-based amplification attacks.

While delegating authoritative DNS services to third-party providers such as Cloudflare, AWS, and Microsoft is often due to the \textbf{...} in performance and ease of deployment, this practice introduces risks through logical centralization. 
A configuration error or targeted attack against one of these providers could simultaneously impact numerous domains. 
Furthermore, many of these same providers are known to operate open resolvers, raising concerns about potentially multiple attack surfaces.

The practice of relying on shared authoritative DNS infrastructure is not inherently problematic, but it places the responsibility for proper configuration solely on the service provider. 
As previous research has demonstrated, a substantial number of resolvers and name servers remain improperly configured, by either lacking response rate limiting, failing to implement proper filtering, or responding from incorrect interfaces \cite{kuhrer_exit_2014, anagnostopoulos_large-scale_2022}.

Our findings reinforce the critical importance of separating authoritative and recursive functionality from one another, implementing best practices (such as RRL), and checking in on essential deployments, especially those related to \texttt{.gov} namespaces. 
Given that a relatively small number of operators control the majority of \texttt{.gov} DNS infrastructure, configuration vulnerabilities may have far-reaching consequences.

We matched the PTR records of our gathered open RDNS set against the set of ADNS used by domains in the Tranco top 1 million list, and in all \verb|.gov| domains. 
We gather 12,349 distinct \verb|.gov| domains, provided by openintel, and find that they are operated by 6,811 ADNSes.
3,554 of these name servers operate at least 2 distinct domains, showing that 11,554 domains share a name server with at least one other domain.
Based on the domains of the ADNSes, we found 1530 distinct ones.

Table \ref{tab:gov_nameserver_ptr} shows why: here we grouped the ADNS based on their PTR record by the domain and found that Cloudflare alone controls a set of ADNS for 2741 unique \verb|.gov| domains (roughly 18.5\% of all \verb|.gov| domains).
We found 73 shared domains (and thus organizations) between our DNS resolver's PTR records and the \verb|.gov| name server domains.
With this we can find organizations/entities that run a .gov ADNS and open RDNS, this however does not mean that a .gov ADNS is also simultaneously an open RDNS.
We took this approach, because when getting the PTR records, it would often include subdomains based on the ip address of the open RDNS, thus we couldn't match open RDNS and ADNS directly.
In total, 4044 \verb|.gov| domain's ADNSes are operated by popular CDN operators that also service open RDNSes.
It is important to configure response rate limiting (RRL) for BIND9, which is used the most (citation), on nameservers. 
It sets a limit to how many responses can be sent to a specific client.

\begin{table*}[!h]
    \centering
    \begin{tabular}{l|l|r|r|r|r}
    \toprule
    \textbf{Organization} & \textbf{Domain} &\textbf{Count Domains} & \textbf{Share of Domains} & \textbf{Count ADNS} & \textbf{Share of ADNS} \\
    \hline
    Cloudflare             & cloudflare.com.                & 2741 & 22.19\% &  877 &  877/6,811 \\
    GoDaddy Inc            & domaincontrol.com.             & 1668 & 13.50\% &   94 &   94/6,811 \\
    Amazon                 & awsdns-\{0-99\}.co.uk.         & 771  &  6.24\% & 1583 & 1583/6,811 \\
    Cloudflare             & civicplus.com.                 & 504  &  4.08\% &    4 &    4/6,811 \\
    Microsoft              & azure-dns.\{info,org,com,net\}.& 394  &  3.19\% &  126 &  126/6,811 \\
    Akamai                 & akam.net.                      & 387  &  3.13\% &  172 &  172/6,811 \\
    DigiCert               & dnsmadeeasy.com.               & 252  &  2.04\% &   14 &   14/6,811 \\
    Wix.com Ltd.           & wixdns.net.                    & 170  &  1.37\% &   16 &   16/6,811 \\
    Wild West Domains, LLC & uberns.com.                    & 155  &  1.25\% &    8 &    8/6,811 \\
    DigiCert               & digicertdns.\{com,net\}.       & 145  &  1.17\% &   15 &   15/6,811 \\
    \bottomrule
    \end{tabular}
    \caption{PTR records of name servers from the openintel .gov TLD zone}
    \label{tab:gov_nameserver_ptr}
\end{table*}

~~Our findings suggest that many popular domains rely on resolvers or name servers that are not hardened against reflection abuse. 
This creates a large pool of amplifiers that can be discovered and reused by attackers. 
Further, we observed that many of these name servers are co-hosted in cloud or ISP environments with lax access policies.~~

---

To investigate the amplification potential of these infrastructures, we cross-referenced our open resolver dataset (see Section~\ref{ss:execution-open}) with the PTR records of the \texttt{.gov} ADNSes. While it is not always possible to determine whether a single IP simultaneously acts as both a recursive and authoritative server (due to PTR record ambiguity and shared domains), we found that 73 domain-level PTRs are present in both datasets.

These shared PTRs correspond to name server organizations that operate both authoritative name servers for \texttt{.gov} domains and open resolvers—though not necessarily on the same IPs. Collectively, these 73 shared organizations are responsible for the ADNS infrastructure of 4,044 \texttt{.gov} domains, or roughly one-third of the total. This finding implies that many government domains rely on providers whose broader infrastructure also includes open recursive resolvers—potentially increasing the exposure to DNS-based amplification.

While the delegation of authoritative DNS services to third-party providers such as Cloudflare, AWS, and Microsoft is often motivated by performance, redundancy, and manageability, it introduces risks through logical centralization. A misconfiguration or attack on one of these providers could affect a large number of domains simultaneously. Moreover, many of these providers are also known to operate open resolvers (either unintentionally or via customer-controlled deployments), raising further concerns about shared abuse surfaces.

The practice of using shared ADNS infrastructure is not inherently insecure, but it places the burden of proper configuration—such as DNSSEC key management, rate limiting, and access control—on the service provider. As prior research has shown, a significant number of resolvers and name servers remain misconfigured, either lacking RRL, failing to enforce query filtering, or responding from the wrong interfaces \cite{kuhrer_exit_2014, anagnostopoulos_large-scale_2022}.

Our findings reinforce the importance of isolating authoritative and recursive functionality, enforcing best practices such as RRL, and auditing name server deployments—especially when supporting critical namespaces such as \texttt{.gov}. Given that a small number of operators manage the vast majority of \texttt{.gov} DNS infrastructure, any vulnerabilities in their configuration or policy could have outsized consequences.



### `fas:Check` \subsection{Traceroute Analysis} \label{ss:execution-traceroute}

\subsection{Traceroute Analysis} \label{ss:execution-traceroute}

To understand the network paths that DNS traffic traverses, we performed traceroute measurements from two vantage points to each discovered open resolver. 
We collected 3,066 Autonomous Systems from VU and 4,196 from Utah, amounting to a combined set of 4,332 distinct Autonomous Systems. 
On average, traceroutes had 15.8 hops from VU and 18.8 hops from Utah, which coincide with the range reported by Doan \etal \cite{doan_evaluating_2021}.
We extracted all intermediate ASes from these traceroutes, excluding the destination AS and the source AS (to avoid bias from the resolver's owner and our measurement devices). 
Table \ref{tab:traceroute_asn} lists the intermediate ASes that appeared most frequently. 
Unsurprisingly, many of these are large tier-1 and tier-2 networks such as Cogent, Zayo, and Liberty Global that act as backbone providers for all internet traffic.
We found that these providers would often peer directly the target's networks, indicating that these organizations are often connected via large IXes.
This centrality of traffic routes can cause widespread disruptions, even if DNS endpoints remain unharmed.
The substantial amount of traffic crossing international boundaries also suggests that much of the open DNS resolver infrastructure likely does not employ Anycast routing.
Additionally, these longer paths can negatively impact performance by introducing higher response latencies.
Understanding these routing patterns is useful for identifying shared dependencies in reaching DNS services. 
If these ASes experience outages or are targeted in an attack, access to many resolvers may be indirectly affected. 
In some cases, attackers have bypassed resilient Anycast setups distributing the load on multiple machines by attacking intermediate ASes instead of the intended victim directly \cite{matthew_ddos_2013}.
Using Gini coefficient analysis over AS frequency in all traceroutes, we observed a Gini of 0.939, indicating a high degree of routing centralization.
This complements the resolver ownership centralization observed earlier and suggests that not only are resolvers concentrated in a few networks, but access to them also relies on set of few intermediate ASes.

### `fas:Check` \subsection{DNSSEC Adoption} \label{ss:exeuction-adoption}
- Adoption of DNSSEC:
	- 1.077.908 distinct domains from openintel (excludes dmarc and www subdomains)
	- 156.875 have rrsig signature
	- 107.924 have ad flag set
	- 107.924 have ad flag set and rrsig signature
- Adoption of DNSSEC in .gov domains
	- 14.811  distinct domains from openintel (excludes dmarc and www subdomains) from originally 31609
	- 4.105 have rrsig signature
	- 3.465 have ad flag set
	- 3.465 have ad flag set and rrsig signature

We assessed DNSSEC adoption in two aspects: (1) support for validating DNSSEC responses at the resolver level, and (2) adoption of DNSSEC keys in domains.

Of the ASes that hosted at least one DNSSEC-capable open resolver, we found  that it rarely correlates to hosting large numbers of open DNS resolvers. 
Figure \ref{fig:dnssec-scatter} plots the share of DNS vs. DNSSEC open resolvers per AS. 
A reasonable assumption would be that ASes hosting many open resolvers would also host many DNSSEC-capable open resolvers.
If the adoption rate were proportional, we would expect a trend that would follow the diagonal shown. 
Instead, the scatter shows that many ASes specialize either in regular resolvers or DNSSEC, by having a disproportionately larger share of all resolvers in question, but rarely both. 
Of the top 10 ASes, only 5 by total resolvers also appear in the top 10 for DNSSEC support. 
This contradicts our initial assumption, suggesting that attackers may not assume DNSSEC support based on open resolver availability, thus increasing the complexity of finding viable amplifiers.
Unsurprisingly, the lesser the number of open DNS resolvers an AS controls, the higher the adoption rate of DNSSEC in that AS.

We compared DNSSEC signing adoption between .gov domains and domains from the Tranco top 1 million list. 
Among 14,811 distinct .gov domains (after filtering out `_dmarc` and `www.` subdomains), 3,465 had both RRSIG records and the AD flag set, giving an adoption rate of 23.4\%. 
In contrast, only 107,924 of 1,077,908 Tranco domains (10\% adoption rate) met the same criteria. 
This imbalance supports prior findings showing that .gov domains are often used in amplification attacks due to their greater adoption of DNSSEC compared to more common domains from TLDs such as .com and .org \cite{nawrocki_far_2021}.

# Discussion
**Here you put your results in context (possibly grouped by research question). Usually, this section focuses on analyzing the
implications of the proposed work for current and future research and for practitioners.**

## \subsection{The Paradox of Centralization and Security in DNS Infrastructure}

These findings highlight an important tension in DNS infrastructure security. 
The current situation—where DNSSEC deployment is limited but geographically distributed—may actually represent a reasonable balance between reducing attack opportunities and maintaining infrastructure resilience.

As DNSSEC adoption grows, it will be important to maintain this balance. Future infrastructure development should consider how to expand DNSSEC deployment while avoiding new patterns of centralization that could create vulnerabilities. This may require coordinated planning to ensure that increased DNSSEC adoption continues to follow a distributed model rather than concentrating in a few major providers or regions.

## \section{Discussion}\label{s:discussion}

%%This section synthesizes our findings in the context of the research questions posed earlier, analyzing their broader implications for security, DNS infrastructure policy, and future directions in DNS operations and measurement.%%

### \textbf{How centralized is the RDNS landscape?}
Through analysis of our measured data, we determined that only 921 of 21,713 open RDNS support DNSSEC, and these are distributed across 513 ASes and 92 countries.
This stands in contrast to the open RDNS population, which is concentrated in just 3,792 ASes and dominated by a handful of hosting providers (e.g., Unified Layer, OVH SAS, Hetzner).
This gives rise to a seemingly paradoxical behaviour; while regular open RDNS infrastructure remains concentrated, DNSSEC-enabled RDNSes show a more decentralized image, which brings both advantages and disadvantages.

The small number of DNSSEC-enabled open resolvers provides some security advantages. 
With only 921 DNSSEC resolvers available compared to over 21,000 total DNS resolvers, attackers have fewer options for launching amplification attacks. 
This scarcity makes it harder to coordinate large-scale attacks, since DNSSEC responses have much larger payloads due to the cryptographic signatures they contain.
With fewer DNSSEC capable recursive resolvers, it naturally limits the total amplification capacity that one can achieve.
Although unintended, this acts as a protective measure against DNS amplification attacks.

Conversely, we should aim to promote DNSSEC adoption, as it is crucial in the integrity of DNS responses. 
A diverse and widely distributed network of DNSSEC RDNS and ADNS ensures response validation in a reliable manner. 
Additionally, a decentralized DNSSEC infrastructure is harder for an attacker to monitor, making it difficult to take advantage of unrestricted reflectors unnoticed, but it also avoids single points of failure.
Thus to continue deployment of DNSSEC services, whilst mitigating potential for misuse in DNS DDoS reflection attacks, prevention strategies such as Source Address Validation are key to preventing spoofed requests from triggering amplification responses.
This also comes as a benefit to IP providers, as it is in their interest to only serve their own customers.
Prior work has shown that outbound SAV deployment remains limited \cite{nosyk_closed_2023}, and our findings reinforce the urgency of closing that gap.

### \textbf{Which parts of DNS infrastructure are most vulnerable to amplification?}
Unsurprisingly, the most vulnerable components are DNSSEC-enabled open RDNS, particularly when operated without rate limiting or access control. 
While only 921 such resolvers were identified, they exist across a broad range of ASes, which increases the attack surface despite low absolute numbers, giving attacks many chances to alternate attack source.
We also found that ADNS infrastructure, specifically those supporting \texttt{.gov} domains, are concentrated in a small number of providers. 
Cloudflare alone serves 22.2\% of all \texttt{.gov} domains. 
When misconfigured or lacking response rate limiting (e.g., RRL in BIND9), these authoritative name servers become attractive targets for attackers seeking high amplification factors.
Moreover, our PTR-based analysis revealed 73 organizations operating both open resolvers and \texttt{.gov} domain name servers (though not necessarily on the same machines), covering over 4,000 domains. 
While not a direct vulnerability, the overlap in services suggests the potential for interdependent failures across public and government infrastructure.

### \textbf{What is the relationship between DNS and DNSSEC RDNS distribution?}
One might reasonably expect that ASes operating many open RDNS would also operate many DNSSEC-enabled ones. Our analysis shows this is rarely the case. 
As shown in Figure \ref{fig:dnssec-scatter}, there is little correlation between an AS's share of open resolvers and its share of DNSSEC-capable ones, which can also be observed in \ref{fig:cdf-as}.
This unexpected asymmetry implies that attackers cannot simply assume DNSSEC support the resolver footprint of an AS alone. 
Instead, they would need to conduct more extensive (and thus expensive) scanning to identify suitable amplifiers,  potentially making attacks less worthwhile, whilst also improving chances of being detected by monitoring systems (such as purposefully placed honeypots \cite{nawrocki_far_2023}).
Furthermore, we observed that \textit{smaller ASes tend to have higher proportions of DNSSEC support}, even if their absolute numbers are low. This may be due to niche providers or research institutions that adopt DNSSEC proactively, in contrast to large hosting providers that prioritize availability or simplicity over cryptographic assurance.

### \textbf{How centralized is the \texttt{.gov} domain infrastructure?}
Our results confirm a high degree of \textit{logical centralization} in the name server infrastructure of \texttt{.gov} domains. 
Over 93\% of these domains share their ADNS infrastructure with at least one other domain, and more than 50\% are hosted by just 10 organizations. 
This suggests that a small number of DNS providers represent a potential point of systemic failure or abuse.
This reliance on third-party DNS hosting is understandable as providers like AWS, Microsoft, and especially Cloudflare offer resilience, scalability, and ease of management and deployment. 
However, this outsourcing comes at the cost of \texttt{operational sovereignty}, and security decisions may be outsourced as well. 
If such providers also operate open resolvers (as many do), the attack surface effectively widens.
The concentration of \texttt{.gov} domains in a small number of commercial providers raises critical questions about governance, policy, and trust. 
The impact of misconfigurations or attacks against these providers would not be isolated events, but linked outages which could affect hundreds of services simultaneously.

### \textbf{Shared routing infrastructure through traceroute observations}

Our traceroute analysis uncovered that \textit{the majority of traffic to open RDNS traverses a small set of intermediate ASes}. 
These are predominantly backbone internet providers such as Cogent, Liberty Global, Zayo, and GEANT. 
The Gini coefficient of AS frequency in traceroutes was 0.939, confirming extreme centrality in routing paths.

This has two key implications:
First, these backbone networks represent critical infrastructure, and disruptions in any form could affect access to a significant portion of recursive DNS resolvers.
Second, the lack of widespread Anycast deployment is evident, as many resolvers are only reachable via long, international paths. 
Encouraging the deployment of Anycast-enabled DNS infrastructure, especially by large providers, could alleviate issues such as response latency and dependency on tier-1 and tier-2 networks.
These findings suggest that \textit{using local resolvers where possible}, or increasing Anycast-based deployments, would improve decentralization of the DNS ecosystem.

# Limitations
**Report about each type of limitations of your study, or threat to the validity of aspects of its design or execution, and how did you mitigate them, according to the classification framework that fits best your study design. For instance, for empirical experiments, use the one proposed by Wohlin \etal \cite{wohlin2012}. Accordingly, the threats as organized as the following sections.
You may also use the work of Verdecchia \etal \cite{verdecchia2023} to present this section at best, for example, if discussing the limitations is a better fit.**

- Didn't use paris traceroute
- Did forward traceroute instead of reverse traceroute (questionable if the paths are the same)
	- The results are affected by *transparent forwarders* which aren't the ones that are responding (Nawrocki et al., “Transparent Forwarders.”)
- IPv4 zmap scan only one time -> many resolvers were not found and used in the results
- Due to zmap scan being at two different times, might be susceptible to IP address changes like with dynamic IP addresses \cite{exit-from-hell} (however we do then concentrate on longer term soluions)

## \section{Limitations}\label{s:limitations}

%% While our study provides new insights into the current state and distribution of open DNS resolvers, nameservers, and DNSSEC adoption, there are several limitations that constrain the precision and generalizability of our findings. %%

Our active scans were conducted only once at a single point in time from each vantage point. 
It is well within reason that some resolvers were either missed or temporarily unreachable during scans.
This means our data provides a snapshot rather than a longitudinal view of the DNS ecosystem. 
Repeating the measurements periodically would help assess stability and evolution over time.

All measurements were conducted from two vantage points—Amsterdam (VU) and Utah (Cloudflare). 
While these locations offer coverage from Europe and North America, they do not represent a global view of network conditions. 
Traceroute paths, resolver reachability, and filtering of traffic can vary significantly based on geographic origin. 
Measurements from additional regions such as Asia, Africa, and South America would provide a more comprehensive understanding of global routing paths and resolver accessibility.

Our measurements did not attempt to categorize forward resolvers - a subset of open DNS resolvers - although their behavior is different.
More accurate attribution could be achieved by operating a custom domain and an authoritative DNS server and recording the source of queries.
Additionaly, the classification of DNSSEC-enabled resolvers through the presence of the RA flag does not alone guarantee proper DNSSEC validation, as shown by Chung \etal \cite{chung_longitudinal_2017}.
 A more robust method, such as using \texttt{delv}, could have provided stronger assurance of DNSSEC validation.
Similarly, we inferred resolver and nameserver ownership based on WHOIS records and reverse DNS (PTR) lookups. 
These identifiers often reflect the organization that owns the IP address but may not reflect operational or administrative control.

%% While traceroutes offered useful insight into the routing dependencies of DNS infrastructure, they were applied uniformly to all IP addresses discovered through ZMap scans. 
This includes DNS forwarders, which do not themselves perform recursive resolution and may simply relay queries upstream.
Including these hosts in our path analysis could slightly distort measurements of centrality, as traceroutes would not terminate at the actual resolving entity.  %%

%% Our domain-level analysis, especially in assessing authoritative name servers and DNSSEC signing rates, was based on two domain sets: the Tranco Top 1 Million and the U.S. \texttt{.gov} TLD zone. While this offers a useful high-level view, it excludes large parts of the DNS namespace such as less popular domains, country-code TLDs, and newer gTLDs. Future work should incorporate a broader domain base—for example, OpenINTEL's data on over 1,200 gTLDs—to improve representativeness. %%

%% Additionally, our detection of DNSSEC adoption was based on the presence of RRSIG records and the AD flag in responses. As mentioned, this provides only a rough approximation of correct configuration. Misconfigurations or partially deployed DNSSEC zones may inflate perceived adoption rates, and domain-side validation was not cross-checked with full trust chain resolution. %%

Our entire scanning and measurement pipeline was limited to the IPv4 address space. 
While IPv4 still dominates DNS infrastructure, IPv6 adoption is steadily increasing, particularly in ISP deployments. 
Resolver distribution, and routing centralization may look different in the IPv6 landscape. 
Future work should replicate this study in the IPv6 space to offer a broader picture of DNS security at Internet scale.

All scans were performed in accordance with ethical standards and with notification infrastructure in place. 
However, out of caution, we opted not to repeat scans to measure time-based changes, nor to use overly large queries that might have revealed additional statistics. 
These decisions limit the aggressiveness but ensure minimal disruption to production services.

Despite these limitations, our study reveals a broad overview of the distribution, centralization, and risk exposure of DNS infrastructure. 
We believe the methodology is sound and provides a solid foundation for future work to build upon with more vantage points, fine-grained categorization, and deeper DNS protocol inspection.


---

The data set is based on a single-time IPv4 scan and may not reflect all active resolvers due to rate limitation, filtering, or IP churn by dynamic addresses. 
Repeating these measurements over time would yield more robust conclusions. 
The results are based on measurements from only two geographic locations, which may skew the traceroute results. 
Resolver ownership was inferred via WHOIS data, which may not always reflect operational control. 
The data gathered includes forward resolvers that do not respond to the data; thus, they should theoretically not be included in the traceroute experiment. To correclty find them, wel could employ our own nameserver that can read the source of the DNS requests (which would be the recursive resolver instead of the forwarder).

DNS resolvers and the nameserver were not thoroughly checked to determine if they support DNSSEC:
\cite{chung_longitudinal_2017} shows that even if they respond with the AD bit set, it does not indicate if they verified the resolver. To solve this, one can use delv to verify the DNSSEC chain of validation instead of dig.

The list of recursion enabled nameservers is strictly limited to the top 1 million domains. We could improve this by gathering many more domains (like the list of new 1200 gTLD also offered by openintel).
Also, having the AD flag set and an rrsig signatures for a domain does not strictly indicate that it is correclty configured \cite{chung_longitudinal_2017}. As such, the results can only be used as rough reference and are not 100\% accurate.

Future work might attempt the same, but instead look at the adoption and concentration in the IPv6 address space.
# Related Work
**Describe here scientific papers similar to your thesis work, both in terms of goal and methodology. One paragraph for each paper (we expect about 5-8 papers to be discussed). Each paragraph contains: (i) a brief description of the related paper and (ii) a black-on-white description about how your work differs from, or overlaps with, the related paper, hence emphasizing the novelty contributed by this thesis. You may place this section immediately after the Background section, if necessary.**
## Annotations (Yazdani et al., 2024)

“For attackers, this means that they need to run discovery scans rather frequently, which might increase the chance that their infrastructure gets detected by network security monitoring systems.” ([Yazdani et al., 2024, p. 4](zotero://select/library/items/STS8Z3FI)) ([pdf](zotero://open-pdf/library/items/P5L87WCJ?page=16&annotation=K2J7YPGK))

“intentional deployment mostly reside in datacenter networks” ([Yazdani et al., 2024, p. 4](zotero://select/library/items/STS8Z3FI)) ([pdf](zotero://open-pdf/library/items/P5L87WCJ?page=16&annotation=PIA7E2MR))

“the majority of persistent resolvers reside in user access networks” ([Yazdani et al., 2024, p. 4](zotero://select/library/items/STS8Z3FI)) ([pdf](zotero://open-pdf/library/items/P5L87WCJ?page=16&annotation=JF2BMVH7))

“For each IP address, we issue a DNS query (toward destination port 53) using a unique subdomain (to avoid caching)” ([Yazdani et al., 2024, p. 4](zotero://select/library/items/STS8Z3FI)) ([pdf](zotero://open-pdf/library/items/P5L87WCJ?page=16&annotation=GX8W2N79))

“Relevant to our study, OpenINTEL measures the authoritative nameserver records (i.e., NS) of domain names and resolves nameserver names to IP addresses (i.e., A records), providing us with a picture of the authoritative landscape.” ([Yazdani et al., 2024, p. 5](zotero://select/library/items/STS8Z3FI)) ([pdf](zotero://open-pdf/library/items/P5L87WCJ?page=17&annotation=NS5BSNLJ))

“We fuse the open resolver scan data (2022-Jan-17) with anycast census data [1,29] from the same month and observe that roughly 9.3 k (∼0.34%) resolvers are hosted in 216 distinct /24 anycasted network prefixes.” ([Yazdani et al., 2024, p. 9](zotero://select/library/items/STS8Z3FI)) ([pdf](zotero://open-pdf/library/items/P5L87WCJ?page=21&annotation=8J4FKHIG))

“58 distinct networks” ([Yazdani et al., 2024, p. 9](zotero://select/library/items/STS8Z3FI)) ([pdf](zotero://open-pdf/library/items/P5L87WCJ?page=21&annotation=RH89QUPZ))

“hypergiants are prominently present in the open resolver ecosystem” ([Yazdani et al., 2024, p. 9](zotero://select/library/items/STS8Z3FI)) ([pdf](zotero://open-pdf/library/items/P5L87WCJ?page=21&annotation=WUL9Z95B))

“2023Oct-30 we find 10.6 k of these non-compliant open resolvers” ([Yazdani et al., 2024, p. 10](zotero://select/library/items/STS8Z3FI)) ([pdf](zotero://open-pdf/library/items/P5L87WCJ?page=22&annotation=Y6PICNHN))

“468 k {domain name, IP address} tuples” ([Yazdani et al., 2024, p. 10](zotero://select/library/items/STS8Z3FI)) ([pdf](zotero://open-pdf/library/items/P5L87WCJ?page=22&annotation=VDDGKE86))

“pointing their NS addresses to 11.8 k unique open resolvers” ([Yazdani et al., 2024, p. 11](zotero://select/library/items/STS8Z3FI)) ([pdf](zotero://open-pdf/library/items/P5L87WCJ?page=23&annotation=PYJWZ8AW))

“Approximately 18 k domains have at least one answer coming from a server which is not authoritative for that domain name” ([Yazdani et al., 2024, p. 11](zotero://select/library/items/STS8Z3FI)) ([pdf](zotero://open-pdf/library/items/P5L87WCJ?page=23&annotation=5D8CIUI8))

“This could constitute a potential security risk since authoritative nameservers are typically well-provisioned and this could make them attractive for being misused in R&A DDoS attacks” ([Yazdani et al., 2024, p. 11](zotero://select/library/items/STS8Z3FI)) ([pdf](zotero://open-pdf/library/items/P5L87WCJ?page=23&annotation=RAE4AVZG))

“Roughly 83% of these IP addresses are visible on less than 5% of our snapshots, while 1% are consistently present in more than 95% of the snapshots (see Fig. 3)” ([Yazdani et al., 2024, p. 12](zotero://select/library/items/STS8Z3FI)) ([pdf](zotero://open-pdf/library/items/P5L87WCJ?page=24&annotation=JI8TIJPU))

“persistency of ASes that host open resolvers” ([Yazdani et al., 2024, p. 12](zotero://select/library/items/STS8Z3FI)) ([pdf](zotero://open-pdf/library/items/P5L87WCJ?page=24&annotation=9MUGMGFJ))

“The persistent concentration of open resolvers in a limited number of networks could have repercussions on the misuse of these servers in R&A attacks, as it could imply that an attacker might not need to sweep the entire address space to find reflectors.” ([Yazdani et al., 2024, p. 13](zotero://select/library/items/STS8Z3FI)) ([pdf](zotero://open-pdf/library/items/P5L87WCJ?page=25&annotation=P45FY4CZ))

“Kührer et al. [12] analyze the reverse DNS records of short-lived open resolvers and attribute them to residential networks” ([Yazdani et al., 2024, p. 13](zotero://select/library/items/STS8Z3FI)) ([pdf](zotero://open-pdf/library/items/P5L87WCJ?page=25&annotation=KT49JTF6))

“roviders with a static IP address assignment, even though these resolvers are likely not intentionally deployed” ([Yazdani et al., 2024, p. 14](zotero://select/library/items/STS8Z3FI)) ([pdf](zotero://open-pdf/library/items/P5L87WCJ?page=26&annotation=DZWPGZ2K))

“While datacenter-based resolvers benefit from a high link-capacity [30], these resolvers are often considered to deploy measures that limit damage in case they are misused in DDoS attacks. Additionally, there is a chance that these resolvers are honeypots deployed to detect malicious network activities.” ([Yazdani et al., 2024, p. 14](zotero://select/library/items/STS8Z3FI)) ([pdf](zotero://open-pdf/library/items/P5L87WCJ?page=26&annotation=EYDLYZS8))

“Persistent open resolvers are mostly hosted in user access networks. This, combined with the intuition that such resolvers likely exist due to misconfigurations, raises worries about their potential misuse in attacks.” ([Yazdani et al., 2024, p. 14](zotero://select/library/items/STS8Z3FI)) ([pdf](zotero://open-pdf/library/items/P5L87WCJ?page=26&annotation=65L4DL84))
## Annotations (Nosyk et al., 2023)
### Measuring the Deployment of Inbound Source Address Validation

“one of the most effective ways to prevent Reflection-based Distributed Denial-of-Service (DDoS) attacks” (Nosyk et al., 2023, p. 2589)

### Submitted Version

“revealing 49% IPv4 and 26% IPv6 Autonomous Systems (AS) that suffer from a consistent or partial absence of inbound filtering” (Nosyk et al., 2023, p. 1)
## Annotations (Xu et al., 2023)

“On 17 June 2021, the Akamai DNS outage left numerous top websites and online services inaccessible, including Google, Amazon, Steam, Cloudflare, and FedEx” (Xu et al., 2023, p. 1)

“The resolver is the executor of the resolution service, which acts as a bridge between users and name servers.” (Xu et al., 2023, p. 4)

“the FDNS does not perform any query to the ADNS but rather forwards the lookups along to RDNSes” (Xu et al., 2023, p. 4)

“the population of responsive open resolvers keeps dropping over time” (Xu et al., 2023, p. 9)

“More than 90% of forwarding resolvers are ultimately backed by fewer than 5% (4071) of indirect resolvers” (Xu et al., 2023, p. 9)

“indirect resolvers are highly concentrated among a small number of IP providers” (Xu et al., 2023, p. 9)

“The top 5 public DNS providers serve approximately 40% of all FDNSes, with the resolver pool operated by Google Public DNS serving more than 22.22% of the total FDNSes” (Xu et al., 2023, p. 10)

“Top five resolver pools.” (Xu et al., 2023, p. 11)

“Africa and Australia, it has less usage probably due to the less-developed economy, Internet access, or the dominant status of local DNS provider” (Xu et al., 2023, p. 12)

“OpenDNS was more prevalent in US and Brazil, while Cloudflare was more popular in Europe and China” (Xu et al., 2023, p. 12)

“We recommend that the internet community focuses on the centralization tendencies within DNS infrastructure and takes steps to alleviate the influence of service provider oligopolies on resolution services, such as promoting the community-driven deployment of public DNS” (Xu et al., 2023, p. 14)

“Do leading NS providers share the IP infrastructure” (Xu et al., 2023, p. 14)

“More than 48.5% (102,245,486) of domain names depend on the top 10 NS providers” (Xu et al., 2023, p. 14)

“21.6% of name server providers rely on just 10 IP providers” (Xu et al., 2023, p. 15)

“Top 10 IP infrastructure providers.” (Xu et al., 2023, p. 16)

“Nearly half (49%) of all domain names are supported by a few name server providers (Top 10), whose infrastructure is centralized into a small number of IP providers, posing quiet yet fatal threats” (Xu et al., 2023, p. 17) Try and do this, but only for .gov domains specifically.
## Annotations (Sommese et al., 2022)

“serves as a support infrastructure for most applications, content distribution platforms, and many security services” (Sommese et al., 2022, p. 51)

“Our data confirms the effectiveness of the use of anycast and diversity in nameserver deployment in providing resiliency against DDoS attacks.” (Sommese et al., 2022, p. 52)
## Annotations (Doan et al., 2021)
“28.3% of the probes” (Doan et al., 2021, p. 1)

“use at least one public DNS service,” (Doan et al., 2021, p. 1)

“IP and AS paths to local resolvers are generally shorter” (Doan et al., 2021, p. 1)

“(DNS) is said to become increasingly centralized [1], [2]” (Doan et al., 2021, p. 1)

“AS paths to public resolvers involve around 2–5 ASes” (Doan et al., 2021, p. 2)

“Responses from local resolvers are faster for 36–60%” (Doan et al., 2021, p. 2)

“Ager et al. [14] (2010)” (Doan et al., 2021, p. 2)

“ocal resolvers managed by the ISPs generally outperform public resolvers” (Doan et al., 2021, p. 2)

“Zembruzki et al. [20] (2020) develop dnstracker” (Doan et al., 2021, p. 2)

“12k name servers share the same infrastructure, which may result in single points of failure” (Doan et al., 2021, p. 2)

“Moura et al. [21] (2020)” (Doan et al., 2021, p. 2)

“centralization benefits the deployment of DNS features such as DNSSEC” (Doan et al., 2021, p. 2)

“As centralized public resolvers leverage IP anycast [42], we expect IP paths from the probe to these resolvers to not be inflated unnecessarily.” (Doan et al., 2021, p. 4)

“inflated path lengths would reveal cases in which centralized public resolvers lack points of presence” (Doan et al., 2021, p. 4)

“5–17 IP hops” (Doan et al., 2021, p. 5)

“AS Path Lengths: We further determine the AS paths to the resolvers based on the IP path measurements. We lookup the ASes of the encountered IP address prefixes via RIPEstat, which derives AS information from BGP data collected by RIPE’s Routing Information Service (RIS). The first IP hop represents AS hop 1; whenever the AS announcing the IP prefix of the intermediate IP hop changes, we increment the AS hop by 1. In case routers along the path are non-responsive or do not have announcing ASes based on RIPEstat, we keep the current AS hop count, but drop measurements with more than two missing AS mappings to avoid incorrect counting.” (Doan et al., 2021, p. 5)

“meaning that Google directly peers with ISPs” (Doan et al., 2021, p. 5)

“Fig. 2. Distribution of IP (top) and AS (bottom) path lengths. Most local resolvers are located in the probe’s AS and have the shortest IP and AS path lengths. In contrast, most public resolvers have AS path lengths of 2–3.” (Doan et al., 2021, p. 5)

“users do not benefit substantially from switching to public DNS resolver services in the average case and may consider to keep using their local resolvers to avoid sharing sensitive outbound DNS traffic with large (CDN) providers” (Doan et al., 2021, p. 7)
## Annotations (van Rijswijk-Deij et al., 2014)

“Nevertheless, an attacker needs only one or a few domains with large amplification factors, and by carefully choosing a signed domain attackers can achieve significant amplification using e.g. DNSKEY queries.” (Rijswijk-Deij et al., 2014, p. 459)

“It is clear then that this needs to be addressed, both by mitigating the risk of carrying out a successful amplification attack as well as by improving the DNSSEC protocol, for instance by using cryptographic signature schemes with more favourable key and signature sizes that reduce the size of DNSSEC responses, such as elliptic curve digital signatures (ECDSA) [23].” (Rijswijk-Deij et al., 2014, p. 459)

“DNSSEC does, however, give attackers more options, such as directly abusing authoritative name servers instead of open resolvers and foregoing the use of crafted domains.” (Rijswijk-Deij et al., 2014, p. 459)
## Annotations (Kührer et al., 2014)

“The amplifiers that are still reachable after 13 weeks presumably have longer lease times or static Internet connectivity. For example, we can see a clear distinction between countries in which SSDP hosts disappeared after a week (e.g., China, Argentina, Russia) and countries in which most hosts are still reachable after 13 weeks (e.g., Korea, United States, Canada). While only 3.4% of the Chinese amplifiers were still reachable after 13 weeks, still more than 69% of the Canadian amplifiers were available. This shows that the geolocation of Internet links (and thus the risk to face IP address churn) highly influences the availability of amplifiers.” (Kührer et al., 2014, p. 115)

“ur remote spoofing test has identified more than 2,000 networks that do not use proper egress filtering” (Kührer et al., 2014, p. 124)
## \section{Related Work}\label{s:related}

DNS infrastructure has been studied throughly, both in terms of it's growing centralization \cite{xu_measuring_2023,nawrocki_transparent_2021,yazdani_swamp_2024,zembruzki_dnstracker_2020,moura_clouding_2020,doan_evaluating_2021,park_where_2019}, and it's use in reflection-based amplification attacks \cite{sommese_investigating_2022,park_where_2019,nawrocki_far_2021,anagnostopoulos_dns_2013,rossow_amplification_2014,kuhrer_exit_2014,nosyk_routing_2022,yazdani_matter_2022,nawrocki_transparent_2021,kopp_ddos_2021,quadir_efficient_2020,anagnostopoulos_never_2018,van_rijswijk-deij_dnssec_2014}.

Several studies have shown that the DNS infrastructure, originally designed as a distributed and federated system, is becoming increasingly centralized. Consistent with our findings, Moura \etal \cite{moura_clouding_2020} and Zembruzki \etal \cite{zembruzki_dnstracker_2020} accentuate that authoritative DNS services are largely consolidated among a few providers, potentially introducing single points of failure and limiting resilience. Doan \etal \cite{doan_evaluating_2021} perform a large-scale traceroute analysis using over more than 10,000 RIPE Atlas nodes, finding that 28.3\% of them have been configured with at least one open DNS resolver. Their work underlines the dependence on public resolvers such as Google and Cloudflare, and reveals how clients are often configured without regard for redundancy or privacy.

Xu \etal \cite{xu_measuring_2023} introduced new active measurement techniques and concluded that the majority of open resolvers are in fact forwarders. 
These forwarders delegate queries to a small set of upstream resolvers, indicating at a structural bottleneck and even tighter centralization in the resolution process. 
They also show that ten DNS providers are responsible for operating name servers for 48.5\% of all domains across 1138 gTLDs. 
Nawrocki \etal \cite{nawrocki_transparent_2021} independently validate these findings, showing that 90\% of forwarders are concentrated behind just 5\% of indirect resolvers.

DNS has long been exploited in reflection-based DDoS attacks due to its support for unauthenticated UDP queries and the ability to evoke disproportionately large responses. Rossow \cite{rossow_amplification_2014} compared amplification factors across 14 different UDP protocols and found that DNS, particularly with DNSSEC and \texttt{ANY} queries, yields among the highest amplification factors—reaching up to 98.3×. 
Van Rijswijk-Deij \etal \cite{van_rijswijk-deij_dnssec_2014} also examined the role of DNSSEC in amplification attacks and emphasized the importance of proper rate limiting, whilst reporting even higher amplification factors (in rare cases up to 179 times greater).

Kührer \etal \cite{kuhrer_exit_2014} studied the lifecycle of DNS amplifiers, including IP churn and uptime, and proposed a fingerprinting-based methodology to detect misconfigured resolvers. 
They found over 2,000 ASes that do not implement source address validation (SAV), which significantly increases the feasibility of spoofed traffic. 
Their work serves as a basis for evaluating how network topology and policy affect attack surfaces, as well insight into new types of DNS amplification attacks using the TCP protocol.

Nawrocki \etal \cite{nawrocki_far_2021} focus on DNSSEC-enabled \verb|.gov| domains, arguing that their high DNSSEC adoption makes them a great resource for large amplification responses. 
They also point out operational mistakes, such as incorrect or stale key rollovers, that make these domains vulnerable to exploitation. 
Anagnostopoulos \etal \cite{anagnostopoulos_never_2018} demonstrate that more often than not, authoritative name servers—not just resolvers—are abused for amplification, contradicting previous assumptions in the field.

Sommese \etal \cite{sommese_investigating_2022} further highlight that DNS reflection is particularly attractive to attackers due to the persistent availability of open resolvers and the difficulty of tracing back to the culprit. 
They also evaluate resilience strategies such as Anycast, which is the most effective mitigation for operators by distributing load across multiple geographically distributed nodes. 
Similarly, Quadir \etal \cite{quadir_efficient_2020} propose filtering methods to censor spoofed traffic and argue for greater cooperation among ISPs and cloud providers.

==Insert summary and how my work differs.==

---

DNS infrastructure has been studied throughly, both in terms of it's growing centralization \cite{xu_measuring_2023,nawrocki_transparent_2021,yazdani_swamp_2024,zembruzki_dnstracker_2020,moura_clouding_2020,doan_evaluating_2021,park_where_2019}, and it's use in reflection-based amplification attacks \cite{sommese_investigating_2022,park_where_2019,nawrocki_far_2021,anagnostopoulos_dns_2013,rossow_amplification_2014,kuhrer_exit_2014,nosyk_routing_2022,yazdani_matter_2022,nawrocki_transparent_2021,kopp_ddos_2021,quadir_efficient_2020,anagnostopoulos_never_2018,van_rijswijk-deij_dnssec_2014}.
Doan \etal \cite{doan_evaluating_2021} also did traceroutes but with $\approx 10.6$k RIPE atlas nodes and found that 28.3\% of them have been configured with at least one open DNS resolver.
Rossow showed that DNS is among the UDP enabled protocols that yield the highest amplification factors.
Nawrocki stated that.gov domains are the most queried domains because they have the highest DNSSEC deployment, as well as failing to implement key rollover correctly. 
Xu \etal developed new measurement techniques and concluded that most open resolvers are forwarders that send data to a small selective group of resolvers that perform most domain resolutions. On top of that, 48,5\% of domains across 1138 gTLD are operated by 10 DNS providers' name servers.
Kührer \etal observed patterns in the deployment of DNS using fingerprinting techniques. They also looked at IP churn rates and determined the amplification potential using TCP. Finally, they found over 2000 AS that do not implement SAV.
Some other best-known practices for mitigating DDoS attacks are outlined by Sommese \etal \cite{sommese_investigating_2022} where Anycast is known to provide effective resilience to end users.

In summary, the existing literature provides a strong foundation for understanding DNS vulnerabilities and design shortcomings. However, most of these studies either predate current DNSSEC deployment trends or rely on static datasets. This thesis builds upon their insights by re-evaluating DNSSEC adoption, amplification potential, and network centralization using updated empirical measurements.
# Conclusion
**Briefly summarize your contributions, and share a glimpse of the implications of this work for future research.**
## \section{Conclusion}\label{s:conclusion}

The Domain Name System remains a foundational yet fragile component of the Internet. 
Although efforts such as DNSSEC have been introduced to eradicate its flaws, our study shows that DNS infrastructure continues to be a reliable weapon in amplification attacks.
Through an internet-wide scan, traceroute analysis, and a correlation with open data sources such as OpenINTEL, we examined the centralization, and deployment characteristics of both open recursive resolvers and authoritative name servers. 
Our findings highlight an interesting paradox: while RDNS infrastructure is highly centralized and concentrated among a small number of hosting providers, DNSSEC-enabled resolvers are less common and thus more evenly distributed—though access to DNSSEC infrastructure becomes less accessible.

Just 20\% of ASes host over 83,5\% of all open resolvers; this if further quantified by a high Gini coefficient of 0.7582 for ASes and 0.8696 for countries, indicating a highly unequal distribution.
By contrast, DNSSEC-capable resolvers exhibit a lower Gini coefficient of 0.3930, suggesting that their deployment is more evenly spread across networks.
Name server infrastructure shows similarly concerning trends. We found that the top 10 providers serve over half of all U.S. government (\texttt{.gov}) domains. In extreme cases, a provider like CivicPlus serves over 500 domains using just four name servers.
We observed that government-operated domains (\texttt{.gov}) are disproportionately reliant on third-party DNS providers, many of which also run open recursive resolvers. 
This operational overlap expands the potential attack surface, when adoption of rate limiting and source address validation remains low. 
Our traceroute measurements further revealed a heavy dependency on a select few backbone networks, an avoidable circumstance with sufficient use of Anycast in resolver deployment and the use of local resolvers.
Plotting adoption of DNSSEC against the relative share of all resolvers of an organization exposed a previously unknown relation: an organization operating many open RDNS is not an indication for hosting many DNSSEC-enabled resolvers.

Against the nature of the internet, DNS infrastructure is not as decentralized as we once believed it to be.
In light of these findings, we argue that improving DNS resilience requires DNSSEC adoption to be accompanied by simultaneous expansions of source address validation across networks.

\paragraph{Acknowledgements.}  
This research was made possible by OpenINTEL~\cite{openintel}, a joint project of the University of Twente, SIDN, NLnet Labs, and SURF, whose datasets were instrumental to our domain analysis. We also used the Tranco list~\cite{LePochat2019}, version generated on 18 April 2025, available at \url{https://tranco-list.eu/list/83NKV}, to assess real-world domain relevance.

---
# Expectations
## Domains
As mentioned previously, we are interested in the deployment of DNSSEC in government domains. For this we will be looking at the .gov TLD available from [openintel](https://www.openintel.nl/data/forward-dns/zone-based/) on the 13th April 2025. To check normal domains for DNSSEC we will take a look at all domains that respond with an rrsig signature and ds record from [Tranco Top 1M openintel](https://www.openintel.nl/data/forward-dns/top-lists/).
## Resolvers
Using the list of open resolvers we would also like to especially look at why they are responding to me. We then want to find how centralised or decentralised they are by seeing in what and how many AS they are located. The more AS, the **more attack vectors** we have on them. Such DNS DDoS amplification attacks can lead to taking down the ISP that runs in that AS. 
- Some nameservers are also configured to do recursive resolution -> this should be turned off

> [!question]- Do AS provide as many resolvers in the IPv6 address space as in the IPv4 address space?
> **Description**: There are 29720 open resolvers broadcast/advertised in the IPv4 address space. Are there just as many with an IPv6 address?
> > [!question]- Is there a clear mapping of IPv4 to IPv6 address?
> > **Description**: If there are just as many resolvers in each address space, does each resolver operate dual-stack (IPv4 and IPv6).
> > **Expectation**: We expect that a resolver with an IPv6 address also has an IPv4 address.
> > **Implication** (for both cases):
> > 1. 
> > 2.
> > 
> > **Result**:
> > > [!info] Explanation of result (Why)
> 
> **Expectation**: We expect to have fewer resolvers with an IPv6 as it's not as common.
> **Implication**:
> 1. If there are fewer resolvers then it suggests slower IPv6 adoption in DNS infrastructure, limiting IPv6-only clients' resolver options. -> dependency on IPv4 infrastructure.
> 2. If there are more resolvers then this might be due to larger providers rolling out IPv6, or DNS specific IPv6 related configurations being common place.
> 
> **Result**:
> > [!info] Explanation of result (Why)

> [!question]- Are resolvers similarly distributed ?
> **Description**: Are geographic, network and organizational distribution patterns of open resolvers consistent in the IPv4 address spaces? We analyse distribution by Autonomous System, county as well as /8, /16 and /24 network blocks.
> > [!question]- Is the distribution the same, but in different groupings from the other type 
> > **Description**: The Gini coefficient measures inequality but doesn't capture which specific entities dominate each address space. Two distributions might have identical Gini coefficients but different dominant players. (e.g. we have 6 groups with the first 3 containing 10 resolvers in IPv4, whilst the other 3 contain 10 resolvers in IPv6)?
> > **Expectation**: We expect a rough overlap. Mostly new deployments will have IPv6 enabled.
> > **Implication**:
> > 1. Both services become available when a major provider goes down (both DNS and DNSSEC) -> Higher risk when DNSSEC amplification attacks take place to affect normal DNS usage
> > 2. DNSSEC is not "dependent" on DNS infrastructure?
> > **Result**:
> > > [!info] Explanation of result (Why)
> 
> > [!question]- Why are the resolvers grouped in certain areas; What groupings have the most amount of resolvers and why.
> > **Description**: This requires looking at the actual data. If there is one (or a few) groups with a substantially high amount of the objects in question, what might lead to that being the case. Understanding these patterns helps explain the underlying infrastructure, business, and policy drivers.
> > **Expectation**: We expect to see high concentrations in major cloud providers (AWS, Google, Cloudflare), large ISPs, and regions with advanced internet infrastructure (North America, Europe, parts of Asia).
> > **Implication**:
> > 3. Dependency on larger providers is a risk when outages occur and could significantly impact global DNS infrastructure.
> > 4. Better resilience to outages when more distributed
> > **Result**:
> > > [!info] Explanation of result (Why)
> 
> **Expectation**: We expect the distribution to be similar based on the AS that the resolvers are in (gini coefficient is similar) in each address space separately. We also expect to see the groups (AS,/8 and /16) with most resolvers to also have the most in IPv6. 
> **Implication**:
> 3. If the distribution is *worse* (higher gini coefficient), then we have areas with more resolvers. The common paths of these (e.g. an AS through which all of these resolvers "run" through) would be easily susceptible to a DNS amplification DDoS attack. It also means that if a path were cut off to these areas, then resolver infrastructure would be impacted significantly (but this might not be a problem since there are many other open resolvers).
> 4. If the distribution is *better* (lower gini coefficient), then it suggests more distributed and resilient DNS infrastructure. However, it provides more sources for attacks, since an attacke can query to way more resolvers without being seen as a threat (can query a single dns resolver less, but send more queries in total).
>  
> **Result**:
> - "70% of DNSSEC-enabled resolvers in my dataset are owned by 10 organizations"
> 
> > [!info] Explanation of result (Why)

-----------------------------------------

> [!question]- Do AS provide as many DNSSEC resolvers as DNS resolvers
> **Description**: There are 21713 open resolvers broadcast/advertised. Do they all support validating DNSSEC domains?
> > [!question]- Is the distribution the same, but in different groupings from the other type 
> > **Description**: The Gini coefficient measures inequality but doesn't capture which specific entities dominate each address space. Two distributions might have identical Gini coefficients but different dominant players.
> > **Expectation**: We expect a rough overlap.
> > **Result**: We find that there is likely no correlation between overall ownership over all DNS resolvers, and those that support DNSSEC. In the top 10 ASes, only 5 were in the top 10 for DNSSEC resolvers.
> **Expectation**: We expect to have fewer resolvers with such capabilities due to the additional complexity that outweighs the benefits.
> **Implication**:
> 4. If there are much fewer resolvers that support validating DNSSEC domains, then many clients cannot take advantage of the supposed benefits. (it has been shown that few use it, even google!)
> 5. If there are roughly as many resolvers that support validating DNSSEC domains, then this indicates good for security, but more resolvers are capable of DNS amplification attacks
> 
> **Result**:
> > [!info] Explanation of result (Why)

> [!question]- Does any AS (ISP) have no DNS(SEC) resolver?
> **Description**: Are there any AS in the IPv4/IPv6 address space with no open resolver? We want to every AS to host a dns resolver but not one that is open. Every AS should have a resolver so clients do not have to query outside, and maybe rely on open resolvers which we do not want in the first place.
> We do want to see many AS that do not advertise open dns resolvers; routers in this AS would not be subject to dns amplification attacks.
> **Expectation**: We expect to see many AS with no open resolver as there are only ~30k open resolvers but many more AS. However they may also not have a resolver that only responds to queries from within that AS (this can only be measured with a large proxy network like with RIPE atlas nodes).
> **Implication**: 
> - If an AS doesn't have a DNS(SEC) resolver, then amplification attacks will not originate from there. If an AS is found to be a major hotspot where many DNS responses go through (will be measured through traceroutes to all open resolvers), then that wouldn't help.
> - If traffic analysis shows that AS serves as a major transit point for DNS traffic to other resolvers, it could still be a critical infrastructure point vulnerable to attacks.
>
> **Result**: 
> > [!info] Explanation of result (Why)

> [!question]- Is the distribution of DNSSEC resolver similar to that of all DNS resolvers?
> **Description**: Is the calculated gini coefficient for the same between all recorded open DNS resolvers and those that support verifying DNSSEC domains? Are the groups that had most open DNS resolvers also those that have the most DNSSEC resolvers? For this we calculate the gini coefficient for a set consisting of ipv4 addresses.
> **Expectation**:
> **Implication** (for both cases):
> 1. A lower gini coefficient indicates more vantage points for dns amplification attacks
> 2. A higher gini coefficient indicates a higher risk of single points failures 
> 
> **Result**:
> > [!info] Explanation of result (Why)

> [!question]- Which are the most common AS/hops in all traceroutes of all open resolvers?
> **Description**: This experiment considers two "vantage points" (One from my personal computer, the other from a proxy). After running traceroute on both devices to all 29720 resolvers, we want to find those hops (ip addresses) which come up most often, as well as the AS of hops that come up the most often. We will disregard all hops belonging to the source and destination networks/AS.
> **Expectation**: We expect the AS of internet exchange points (IXPs) to be the largest contributors to these paths and especially tier 1 ISPs. We also expect to see those AS where there are most resolvers.
> **Implication** (for both cases):
> 3. If these traceroute are different to common critical infrastructure for usual internet traffic, they might be neglicted and a may fall easily to such attacks.
> 4. More transit paths indicates a sparser attack surface which is good for defending against amplification attacks.
> 
> **Result**:
> > [!info] Explanation of result (Why)

> [!question]- How many nameservers are resolvers?
> **Description**: We would like to see few, as there is no need to have them be configured to do recursive lookups, as this provides more vectors for a DDoS attack.
> > [!question]- Are they owned by the domain registrar, or run privately?
> > **Description**: If they are run privately, then this could be considered a mistake
> 
> **Expectation**:
> **Implication** (for both cases):
> 5. Indicates widespread misconfiguration that leads to more opportunities for amplification attacks
> 
> **Result**:
> > [!info] Explanation of result (Why)

> [!question]- What is the distribution of nameservers that server .gov domains and how many are there
> **Description**: .gov domains are those that are used most (cite: The Far Side of DNS Amplification), and so those nameservers are queried most for DNSSEC amplification attacks. We would like to look into the location of these.
> **Expectation**:
> **Implication** if
> 7. *false*: 
> 8. *true*:
> 
> **Result**:
> > [!info] Explanation of result (Why)

### Plots
- ==TODO==: Compare with list of open dns resolvers in 2016 http://dns.measurement-factory.com/surveys/openresolvers/ASN-reports/latest.html

# Previous Studies
These are findings of previous papers that i could compare to

- https://blog.cloudflare.com/deep-inside-a-dns-amplification-ddos-attack/
	- Date: 2012-10-30
	- A list of the top networks with abused open DNS resolvers

---
References:

[^1]: https://blog.cloudflare.com/deep-inside-a-dns-amplification-ddos-attack/

[^2]: https://archive.nanog.org/meetings/nanog45/presentations/Tuesday/Katz_reversetraceroute_N45.pdf

[^3]: https://lg.he.net/

[^4]: https://github.com/HSAnet/reverse-traceroute

[^5]: https://www.youtube.com/watch?v=uTTomnOjJ_o

[^6]: https://securepki.org/sec17.html

[^7]: https://www.cisa.gov/news-events/alerts/2013/03/29/dns-amplification-attacks

[^8]: https://openresolverproject.org/

[^10]: http://130.37.198.76