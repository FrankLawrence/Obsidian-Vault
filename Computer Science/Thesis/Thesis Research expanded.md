# Re-evaluating the Potential for Abusing the DNS Ecosystem: A Comprehensive Analysis of DNSSEC Deployment and Vulnerabilities

## Research Problem Statement

While DNSSEC was designed to enhance DNS security by providing cryptographic validation of DNS responses, its widespread deployment has introduced new attack vectors and amplified existing vulnerabilities. The complexity of DNSSEC implementation, combined with inconsistent deployment practices, creates a paradoxical situation where security mechanisms may inadvertently increase the DNS ecosystem's susceptibility to abuse.

## Primary Research Question

**How does the current state of DNSSEC deployment across domains, nameservers, and recursive resolvers create opportunities for DNS ecosystem abuse, and what are the implications for internet infrastructure resilience?**

## Sub-Research Questions

### 1. DNSSEC Deployment Patterns and Quality
- **RQ1a**: What is the distribution and quality of DNSSEC implementation across different domain categories (government, commercial, critical infrastructure)?
- **RQ1b**: How do DNSSEC deployment patterns vary across geographical regions and autonomous systems?
- **RQ1c**: What proportion of DNSSEC-enabled domains exhibit configuration errors that could be exploited?

### 2. Open Resolver Vulnerability Assessment  
- **RQ2a**: To what extent do open DNS resolvers support DNSSEC validation, and how does this vary by operator type and geographical location?
- **RQ2b**: What is the amplification potential of DNSSEC-enabled open resolvers compared to traditional DNS responses?
- **RQ2c**: How centralized is the open resolver infrastructure, and what are the implications for targeted attacks?

### 3. Nameserver Infrastructure Analysis
- **RQ3a**: What proportion of domains rely on third-party vs. self-managed nameserver infrastructure for DNSSEC?
- **RQ3b**: How do nameserver consolidation patterns affect the DNS ecosystem's resilience to targeted attacks?
- **RQ3c**: What correlation exists between nameserver operator concentration and DNSSEC implementation quality?

## Extended Literature Review and Context

### DNS Amplification and DNSSEC
Recent studies have shown that DNSSEC significantly increases the potential for DNS amplification attacks. Rossow (2014) demonstrated that DNSSEC responses can achieve amplification factors of up to 179:1, compared to 28:1 for traditional DNS queries [^7]. This amplification potential is exacerbated by the cryptographic signatures and additional resource records required by DNSSEC.

### DNSSEC Deployment Studies
Chung et al. (2017) found that while DNSSEC adoption has grown, many implementations suffer from operational issues including expired signatures, incorrect key rollovers, and validation failures [^8]. Their longitudinal analysis revealed that approximately 31% of DNSSEC-signed domains had validation issues at any given time.

### Infrastructure Centralization Concerns
Moura et al. (2020) highlighted the increasing centralization of DNS infrastructure, with a small number of providers handling a disproportionate share of DNS queries [^9]. This centralization creates potential single points of failure and increases the impact of targeted attacks against major DNS providers.

## Enhanced Methodology

### Data Collection Framework

#### 1. Global DNS Infrastructure Mapping
- **IPv4 Address Space Scanning**: Use zmap to identify active DNS services across the entire IPv4 space
- **Resolver Classification**: Distinguish between open recursive resolvers, authoritative nameservers, and hybrid configurations
- **Geographical and Organizational Attribution**: Combine MaxMind GeoIP data with ASN/WHOIS information for comprehensive mapping

#### 2. DNSSEC Support Assessment
- **Multi-tool Validation**: Employ dnsviz, delv, and custom validation scripts to assess DNSSEC support quality
- **Validation Chain Analysis**: Trace complete DNSSEC validation chains to identify weakest links

#### 3. Domain Dataset Assembly
- **Diverse Domain Categories**: Analyze government (.gov), commercial (Tranco Top 1M), financial institutions, and critical infrastructure domains
- **DNSSEC Quality Assessment**: Beyond mere presence, evaluate signature freshness, key rollover practices, and validation chain integrity
- **Longitudinal Tracking**: Monitor DNSSEC deployment changes over time

### Advanced Metrics and Analysis

#### Centralization Risk Metrics
1. **Herfindahl-Hirschman Index (HHI)** for DNS provider concentration
2. **Critical Infrastructure Dependency Score**: Percentage of essential services relying on top-N providers
3. **Geographic Distribution Entropy**: Measure of DNS infrastructure geographical spread

#### Security Posture Indicators
1. **DNSSEC Validation Rate**: Proportion of domains with working end-to-end validation
2. **Configuration Error Prevalence**: Types and frequency of DNSSEC misconfigurations
3. **Amplification Risk Score**: Calculated based on response size ratios and resolver accessibility

#### Attack Vector Quantification
1. **Theoretical Amplification Capacity**: Maximum traffic amplification achievable using identified open resolvers
2. **Target Vulnerability Index**: Scoring of high-value targets based on DNSSEC deployment quality
3. **Systemic Risk Assessment**: Impact analysis of attacking concentrated infrastructure points

## Expected Contributions

### 1. Empirical Security Assessment
Provide the first comprehensive, large-scale assessment combining DNSSEC deployment quality with abuse potential analysis across multiple infrastructure layers.

### 2. Centralization Risk Quantification
Develop metrics to quantify the systemic risks introduced by DNS infrastructure centralization, particularly in the context of DNSSEC deployment.

### 3. Policy Recommendations
Generate evidence-based recommendations for:
- DNS operator security practices
- Internet governance policies
- Critical infrastructure protection strategies

### 4. Methodological Framework
Establish reproducible methodologies for ongoing DNS ecosystem security assessment.

## Potential Findings and Implications

### Hypothesis 1: DNSSEC Deployment Quality Varies Significantly by Sector
- **Expectation**: Government and financial domains will show higher DNSSEC adoption but not necessarily better implementation quality
- **Implication**: Regulatory compliance doesn't guarantee security effectiveness

### Hypothesis 2: Infrastructure Centralization Amplifies Attack Impact
- **Expectation**: A small number of DNS providers serve a disproportionate share of DNSSEC-enabled domains
- **Implication**: Targeted attacks against major providers could have cascading effects

### Hypothesis 3: Open Resolver DNSSEC Support Creates Amplification Risks
- **Expectation**: Many open resolvers support DNSSEC without proper access controls, enabling abuse
- **Implication**: Current DNS amplification mitigation strategies may be insufficient for DNSSEC-enabled attacks

## Study Limitations and Mitigation Strategies

### Ethical Considerations
- Implement rate limiting to avoid service disruption during scanning
- Use passive measurement techniques where possible
- Coordinate with network operators for large-scale assessments

### Technical Limitations
- IPv6 space coverage challenges (address space too large for complete scanning)
- Temporal validity of measurements (DNS configurations change frequently)
- DNSSEC validation complexity may miss subtle implementation issues

### Mitigation Approaches
- Focus IPv6 scanning on known allocated prefixes
- Implement continuous monitoring for temporal consistency
- Employ multiple validation tools for cross-verification

## Timeline and Deliverables

### Phase 1: Infrastructure Discovery (Weeks 1-4)
- Complete IPv4 DNS service mapping
- Establish baseline resolver and nameserver databases
- Implement geographical and organizational attribution

### Phase 2: DNSSEC Assessment (Weeks 5-8)
- Conduct comprehensive DNSSEC support analysis
- Perform domain-level DNSSEC quality assessment
- Generate centralization and risk metrics

### Phase 3: Analysis and Validation (Weeks 9-12)
- Statistical analysis of findings
- Cross-validation of results
- Development of visualization tools (Hilbert maps, heat maps)

### Phase 4: Documentation and Recommendations (Weeks 13-16)
- Thesis writing
- Policy recommendation development
- Results dissemination preparation
---
> [!question]- Do AS provide as many resolvers in the IPv6 address space as in the IPv4 address space? 
> **Description**: There are 29,720 open resolvers broadcast/advertised in the IPv4 address space. This question examines whether IPv6 deployment has reached similar levels for DNS resolver infrastructure, or if IPv4 still dominates the open resolver landscape.
> 
> > [!question]- Is there a clear mapping of IPv4 to IPv6 address? 
> > **Description**: If there are similar numbers of resolvers in each address space, this sub-question examines whether each resolver operates dual-stack (having both IPv4 and IPv6 addresses) or if they represent separate resolver populations. 
> > **Expectation**: We expect that a resolver with an IPv6 address also has an IPv4 address, indicating dual-stack deployment rather than IPv6-only resolvers. 
> > **Implication**:
> > 
> > 1. If resolvers are predominantly dual-stack, then IPv6 adoption represents infrastructure enhancement rather than replacement, suggesting good transition practices.
> > 2. If many resolvers are IPv6-only or IPv4-only, this indicates fragmented resolver infrastructure that could affect client connectivity and DNS resilience. 
> > 
> > **Result**:
> > 
> > > [!info] Explanation of result (Why)
> 
> **Expectation**: We expect to have fewer resolvers with IPv6 addresses as IPv6 adoption is still progressing globally and many networks haven't fully deployed dual-stack infrastructure.
> **Implication**:
> 
> 1. If there are significantly fewer IPv6 resolvers, it suggests slower IPv6 adoption in DNS infrastructure, potentially limiting IPv6-only clients' resolver options and creating dependency on IPv4 infrastructure.
> 2. If there are unexpectedly more IPv6 resolvers, it could indicate aggressive IPv6 deployment by major providers or potential IPv6-specific resolver deployment strategies.
> 
> **Result**:
> 
> > [!info] Explanation of result (Why)

> [!question]- Are resolvers similarly distributed in both address spaces?
>  **Description**: This examines whether the geographic, network, and organizational distribution patterns of open resolvers are consistent between IPv4 and IPv6 address spaces. We analyze distribution by Autonomous System (AS), /8 network blocks, /16 network blocks, and country to understand if IPv6 resolver deployment follows existing IPv4 infrastructure patterns.
> 
> > [!question] Is the distribution the same, but in different groupings from the other type?
> >  **Description**: The Gini coefficient measures inequality but doesn't capture which specific entities dominate each address space. Two distributions might have identical Gini coefficients but different dominant players (e.g., AS X dominates IPv4 resolvers while AS Y dominates IPv6 resolvers).
> >  **Expectation**: We expect some overlap in dominant AS/networks but also expect different players to emerge as IPv6 leaders, particularly cloud providers and newer infrastructure operators.
> >  **Implication**:
> > 
> > 4. If the same entities dominate both spaces, it suggests established players are leading IPv6 adoption, providing infrastructure continuity but potentially concentrating control.
> > 5. If different entities dominate each space, it indicates diversification of DNS infrastructure, which could improve resilience but might also fragment resolver ecosystems.
> > 
> > **Result**:
> > 
> > > [!info] Explanation of result (Why)
> 
> > [!question] Why are the resolvers grouped in certain areas; What groupings have the most resolvers and why?
> >  **Description**: This requires analyzing the actual data to identify which geographic regions, network operators, or organizational types host the highest concentrations of open resolvers. Understanding these patterns helps explain the underlying infrastructure, business, and policy drivers.
> >  **Expectation**: We expect to see high concentrations in major cloud providers (AWS, Google, Cloudflare), large ISPs, and regions with advanced internet infrastructure (North America, Europe, parts of Asia).
> >  **Implication**:
> > 
> > 6. If certain cloud providers or ISPs dominate, it creates dependency risks where service outages could significantly impact global DNS resolution capabilities.
> > 7. If resolvers are more evenly distributed, it suggests healthier infrastructure diversity and improved resilience against targeted attacks or infrastructure failures.
> > 
> > **Result**:
> > 
> > > [!info] Explanation of result (Why)
> 
> **Expectation**: We expect the distribution to be similar based on the AS that host the resolvers (similar Gini coefficients). We also expect the groups (AS, /8, and /16 networks) with the most IPv4 resolvers to also have substantial IPv6 resolver presence.
> **Implication**:
> 
> 1. If the IPv6 distribution is worse (higher Gini coefficient), then we have greater concentration of resolvers in fewer networks. This creates single points of failure and makes DNS amplification DDoS attacks more effective by targeting fewer, more critical infrastructure points.
> 2. If the IPv6 distribution is better (lower Gini coefficient), it suggests more distributed and resilient IPv6 DNS infrastructure, potentially making the internet more robust against targeted attacks and infrastructure failures.
> 
> **Result**:
> 
> > [!info] Explanation of result (Why)

> [!question]- Do AS provide as many DNSSEC resolvers as DNS resolvers?
>  **Description**: Of the 29,720 open resolvers identified, this question examines how many support DNSSEC validation. DNSSEC validation is crucial for DNS security but requires additional processing and infrastructure investment, so adoption rates may differ from basic DNS resolution.
>  **Expectation**: We expect to have fewer resolvers with DNSSEC validation capabilities due to the additional complexity, computational overhead, and configuration requirements.
>  **Implication**:
> 
> 1. If there are significantly fewer DNSSEC-validating resolvers, it suggests security gaps in DNS infrastructure where many clients may not benefit from DNSSEC protection, potentially exposing them to DNS spoofing attacks.
> 2. If there are roughly as many DNSSEC-validating resolvers as regular DNS resolvers, it indicates good security adoption but also means more resolvers are capable of generating large DNSSEC responses, potentially increasing DNS amplification attack risks.
> 
> **Result**:
> 
> > [!info] Explanation of result (Why)

> [!question]- Does any AS (ISP) have no DNS(SEC) resolver?
>  **Description**: This examines whether there are Autonomous Systems in the IPv4/IPv6 address space that host no open resolvers at all. Understanding resolver-less AS helps identify potential infrastructure gaps and security implications for networks that must rely entirely on external DNS resolution.
>  **Expectation**: We expect to see many AS with no open resolvers since there are only ~30,000 open resolvers but tens of thousands of AS globally. However, they may have private resolvers that only respond to queries from within their network (which can only be measured with distributed vantage points).
>  **Implication**:
> 
> - If an AS doesn't have any open DNS resolvers, it reduces that AS's attractiveness as a target for DNS amplification DDoS attacks launched from within that network. However, if traffic analysis shows that AS serves as a major transit point for DNS traffic to other resolvers, it could still be a critical infrastructure point vulnerable to disruption.
> 
> **Result**:
> 
> > [!info] Explanation of result (Why)

> [!question]- Is the distribution of DNSSEC resolvers similar to that of all DNS resolvers (IPv4 and IPv6 combined)?
> **Description**: This compares the Gini coefficient and distribution patterns between all open DNS resolvers and the subset that supports DNSSEC validation. It examines whether DNSSEC deployment follows existing DNS infrastructure patterns or shows different concentration/distribution characteristics across networks and regions. 
> **Expectation**: We expect DNSSEC resolver distribution to be somewhat more concentrated than general DNS resolvers, as DNSSEC deployment typically requires more technical expertise and resources, favoring larger, more sophisticated operators. 
> **Implication**:
> 
> 1. If DNSSEC resolvers are more concentrated (higher Gini coefficient), it suggests security infrastructure is controlled by fewer entities, creating potential single points of failure for DNS security but also making security updates and coordination easier.
> 2. If DNSSEC resolvers are more distributed (lower Gini coefficient), it indicates broader adoption of DNS security practices across diverse network operators, improving overall internet security resilience. 
> 
> **Result**:
> 
> > [!info] Explanation of result (Why)

> [!question]- Which are the most common AS/hops in all traceroutes to all open resolvers? 
> **Description**: This experiment uses multiple vantage points (personal computer and proxy) to perform traceroutes to all 29,720 resolvers. The goal is to identify the most frequently traversed intermediate hops (IP addresses) and their corresponding Autonomous Systems, excluding source and destination networks. This reveals critical transit infrastructure for DNS traffic. 
> **Expectation**: We expect to see major internet backbone providers (Tier 1 ISPs) and large cloud providers as the most common transit AS, with certain geographic Internet Exchange Points (IXPs) appearing frequently in traceroutes. 
> **Implication**:
> 
> 4. If a small number of AS appear in most traceroutes, it reveals critical infrastructure dependencies where outages or attacks on these transit providers could severely impact global DNS resolution capabilities.
> 5. If transit paths are more diverse, it suggests more resilient internet routing for DNS traffic, reducing the impact of single points of failure but potentially making network troubleshooting more complex.
> 
> **Result**:
> 
> > [!info] Explanation of result (Why)

> [!question]- How many nameservers are also configured as open resolvers? 
> **Description**: This examines the overlap between authoritative nameservers (which should only answer queries for domains they're authoritative for) and open recursive resolvers (which perform lookups for any domain). Nameservers configured as open resolvers represent a security misconfiguration that increases attack surface. 
> **Expectation**: We expect to see relatively few nameservers configured as open resolvers, as this represents a security misconfiguration. Well-managed DNS infrastructure should separate authoritative and recursive functions. 
> **Implication**:
> 
> 6. If many nameservers are also open resolvers, it indicates widespread DNS misconfigurations that increase vulnerability to DNS amplification attacks, cache poisoning, and other DNS-based attacks.
> 7. If few nameservers are open resolvers, it suggests better DNS security practices and proper separation of authoritative and recursive DNS functions.
> 
> **Result**:
> 
> > [!info] Explanation of result (Why)

# other

## 📘 What’s Missing (To Be Written by You or with My Help)

| Section                   | Description                                                                                            | Who should do it / Notes                                                              |
| ------------------------- | ------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------- |
| **Abstract**              | Needs to be written last once results/discussion are finalized. I’ve included a draft below.           | You can use the draft provided below.                                                 |
| **Discussion**            | Should answer the research questions using your data. See my draft below.                              | I've written a first version you can refine with new statistics/plots.                |
| **Results (expanded)**    | Needs more detail (e.g., number of DNSSEC resolvers, domain-level DNSSEC adoption, plots).             | Insert charts, bar graphs, pie charts, Gini plots. I can generate text explanations.  |
| **Related Work (full)**   | Annotated well, but still in note format. Each paper should have a paragraph discussing its relevance. | Use your Zotero annotations as foundation, I can help phrase them into academic tone. |
| **Conclusion**            | Needs synthesis of the contributions + call for future work.                                           | I can help write this once all results are finalized.                                 |
| **Figures/Tables**        | Insert Gini coefficient plots, bar charts for AS distribution, heatmaps for country stats.             | Share screenshots or CSVs, I can describe and contextualize each.                     |
| **Domain-level Analysis** | (.gov vs. porn sites, DNSSEC correctness, recursion in nameservers)                                    | You've noted this, but we need to finish the DNSSEC verification analysis on those.   |

---

## 📊 Discussion: Explanation of Key Results

### Open Resolvers and DNSSEC Support (Table Interpretation)

**Finding**: Of 21,713 open resolvers, only 921 support DNSSEC validation.

* **Interpretation**: This supports the hypothesis that DNSSEC is not widely deployed, especially among open resolvers.
* **Implication**: While this reduces the attack surface for DNSSEC-based amplification attacks, it also indicates lack of validation for users relying on these resolvers — compromising DNS integrity.
* **Policy Suggestion**: ISPs should either restrict access to resolvers (make them closed) or implement DNSSEC if open.

---

### Centralization (Gini Coefficient)

**Gini for AS**:

* Open Resolvers: **0.7582** (Highly centralized)
* DNSSEC Resolvers: **0.3930** (More evenly distributed)

**Interpretation**: A few ASes dominate open resolver availability (e.g., OVH, Unified Layer, Hetzner). But DNSSEC-capable resolvers are more scattered.

**Implication**: Centralization makes certain providers attractive targets. A DDoS attack on one AS could eliminate a disproportionate number of resolvers.

**Attack Strategy Insight**: An attacker doesn’t need a large botnet — they just need to target a few high-value ASes to generate massive DNSSEC amplification.

---

### Traceroute Path AS Frequency

**Finding**: ASes like Cogent (AS174), Zayo (AS6461), Hurricane Electric (AS6939) appear frequently in paths to open resolvers.

* **Interpretation**: These are common transit ASes — likely tier-1 or tier-2 networks.
* **Implication**: Their failure (or targeting) could break access to large parts of the DNS infrastructure.
* **Security Takeaway**: CDN outages like Akamai’s (Xu et al., 2023) could be repeated if such ASes are DDoS’d.

---

### DNSSEC Adoption in Domains

**Preliminary Result**: From 31,609 .gov domains, \~8,122 had RRSIGs but only 5,748 showed the AD flag.

* **Interpretation**: Roughly 18% are fully DNSSEC-validating.
* **Implication**: Even in government domains — where DNSSEC should be default — there's a significant misconfiguration rate.
* **Future Analysis**: Match this against domain name server IPs to see who’s responsible for DNSSEC failures.

---

## 📄 Draft Abstract

> The Domain Name System (DNS) is a core component of the Internet, yet remains susceptible to abuse due to design decisions prioritizing performance over security. DNS Security Extensions (DNSSEC) provide authenticity guarantees, but also increase response sizes, making them ideal for exploitation in amplification-based Distributed Denial-of-Service (DDoS) attacks. In this thesis, we analyze the current state of open DNS resolvers and DNSSEC adoption using large-scale IPv4 scanning, traceroute-based AS-path mapping, and DNSSEC capability testing. Our results show strong centralization of open resolvers, sparse and uneven DNSSEC deployment, and potential misuse via misconfigured authoritative nameservers. We identify vulnerable network segments based on common AS path convergence, and propose targeted mitigation strategies including Source Address Validation (SAV) and open resolver restriction. This work updates the empirical understanding of DNS infrastructure security and provides recommendations for future DNS operational practices.


---

**References:**

[^1]: https://blog.cloudflare.com/deep-inside-a-dns-amplification-ddos-attack/
[^2]: https://archive.nanog.org/meetings/nanog45/presentations/Tuesday/Katz_reversetraceroute_N45.pdf
[^3]: https://lg.he.net/
[^4]: https://github.com/HSAnet/reverse-traceroute
[^5]: https://www.youtube.com/watch?v=uTTomnOjJ_o
[^6]: https://securepki.org/sec17.html
[^7]: Rossow, C. (2014). Amplification Hell: Revisiting Network Protocols for DDoS Abuse. In Network and Distributed System Security Symposium (NDSS).
[^8]: Chung, T., et al. (2017). A Longitudinal, End-to-End View of the DNSSEC Ecosystem. In USENIX Security Symposium.
[^9]: Moura, G. C. M., et al. (2020). Clouding up the Internet: How Centralized is DNS Traffic Becoming? In Internet Measurement Conference (IMC).