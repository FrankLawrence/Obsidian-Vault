---
Tags: 
Created: 2025-12-15 16:45:00
---
(Links:: [[Security Engineering]])
- Risk analysis is relevant for all phases
	- receives and provides input from/into all phases
	- *iterative activity*

> [!question] What are threats, vulnerabilities, and risks?

# Motivation and Goals

- key ideas and activities to guide on operational decisions
	- identity the organizational *assets* and their *valuation*
	- understand their *vulnerabilities*
	- identify the most probable *threats* to an organization
	- determine *risks* and suitable *countermeasures*

> [!info] Information security objectives
> 1. Maintain customer, **stockholder**, and taxpayer confidence in organization
> 2. Protect confidentiality, integrity, and availability of sensitive data
> 3. Avoid third party liability for illegal or malicious acts committed with organization's systems
> 4. Ensure that organization's computer, network, and data are not misused
> 5. Avoid fraud
> 6. Avoid expensive and disruptive incidents
> 7. Comply with pertinent laws and regulations

- How much do you spend?
	- You need to balance requirements, usability, costs and risks
	- Differentiate between relevant threats and theoretical ones
	- What would actually be the effect (*impact*) of the attack
# Assets, threats, vulnerabilities
- Things of value to an organization:
	- Information: data or intellectual property
	- Products
	- Buildings
	- Systems: hardware and software
	- People
	- Reputation
	- Trust of business partners
	- Potential political fallout

## Threats
> [!defniition] Threat
> Potential cause of an *unwanted event* that may harm the organization and its assets.
> - Event: exploit or attack causing harm
> - If source is human: accidental/intentional (motives)

> [!definition] Vulnerability
> A characteristic (including a weakness) of an asset that can be exploited by a threat.
> - System weakness that can potentially be exploited
> - Threat is an actual way of exploiting a vulnerability

> [!example]-
> - vulnerability: unvalidated input
> - threat: a malicious entity inputs a harmful string
> - unwanted event: access to customer data base

- For different stakeholders: determine how most important assets are threatened
- Sources of threats *don't always have to be based on malicious intent*: 
	- Humans (various motives and position wrt. the organization)
		- External hackers seeking a thrill of financial gain
		- External hackers (or nation states) with malicious intent
		- Insiders with malicious intent
		- Insiders out of negligence
	- Nature: Floods, earthquakes, storms,...
	- Environment: Equipment failure, power outage, plumbing failure, ...

## Impact
- Destruction: Facilities, data, equipment, communications, personnel
- Corruption or modification: data, applications
- Theft, removal, or loss: equipment, data, applications
- Unwanted disclosure of data
- Inappropriate use: Unlicensed software, repudiated or false data
- Interruption of services

## Countermeasures
- Means to detect, hear or deny attacks (look at mis-usecase diagrams)
	- Encryption, authentication
	- intrusion detection
	- auditing
- Countermeasures may have vulnerabilities and may be attacked!
- Countermeasures are *not free*!
	- Direct costs (buy software)
	- Indirect costs (training personnel)
	- Impact on system functional or non-functional behaviour (false positives, usability)
- Related to [[Security Engineering Secure Design | Secure Design]]
# Risk

![[Concepts and relationships of risk.svg|600]]

> [!definition] Risk
> Risk is the possibility to suffer harm or loss. Risk also is a measure of failure to counter a threat.
> It measures *expected loss* resulting from a threat successfully exploiting a vulnerability.

An organization's risks are a function of:

<ol>
<li>A <em>loss association with an event</em>, e.g., disclosure of confidential data, lost time, lost revenues</li>
<li>
<details>
<summary>The <em>probability/likelihood/frequency of event occurrence</em> </summary>
N.B. statisticians distinguish between probability and likelihood. In security, these terms often used interchangeably. Frequency is used when studying a specified time period and probability when studying a single observation.
</details>
</li>
<li> The <em>degree</em> to which the <em>risk outcome can be influenced</em></li>
</ol>

- By quantifying risk, we can justify spending money on controls: $$\text{RISK} = \text{LOSS (\$)} \times \text{PROBABILITY}$$

> [!example] Annual Loss Expectancy (ALE)
> $$\Sigma ~ \text{Loss} \times \text{Annual Rate of Occurrence (ARO)}$$
> - Disaster for damage of $3 Mil occurrence is every 50 years -> ARO = 0.02
> - ALE = \$3 million $\times$ 0.02 = $60.000
> - Financial loss is actually $3 million

## Risk enablers/vulnerabilities
- Software design flaws
- Software implementation errors
- System misconfiguration, e.g. firewalls, WLANS, ..
- Inadequate security policies or enforcement
- Poor system management
- Lack of physical protection
- Lack of employee training

> [!attention] Human errors are behind most risk enablers

## Strategies for reducing risk
- **Avoid** the risk, by changing requirements for security or other system characteristics (redesign/implementation phases)
- **Transfer** the risk, by allocating it to other systems, people, organization's assets or by buying insurance
- **Assume** the risk, by either *mitigating/reducing* it with available resources, or simply *accepting* it

# Qualitative and quantitative risk analysis and management
## Risk analysis and management
- **Risk analysis** is the process of examining a system and its operational context to determine possible exposures and the harm they can cause
- **Risk management** involves the identification, selection, and adoption of security measures justified by
	- the identified risk to assets
	- the degree by which the measures reduce these risks to acceptable levels
	- the cost of these measures

> [!info] Generic Procedure
> 1. Identify *assets* to be reviewed
> 2. Ascertain *threats* and the *corresponding vulnerabilities* regarding that asset
> 3. *Calculate* and *prioritize* the risk; Decide how to handle it
> 4. For assumed risks: Identify and implement *countermeasures* (that themselves do not contain vulnerabilities), controls or safeguards-or accept the risk
> 5. *Monitor* the effectiveness of the controls and assess them

## Quantitative risk analysis
- **Goal**: assign independently obtained, objective, *numeric values* to all components of a risk analysis
	- Asset value and potential loss
	- Safeguard effectiveness
	- Safeguard cost
	- Probability
- Pros:
	- Effort put into asset value determination and risk mitigation
	- Cost/benefit analysis
	- Numbers good for comparisons and communication
- Cons:
	- Costly
	- accuracy unclear
- **Rational**: Businesses want to measure risk in terms of money
- Difficult for many logical and intangible assets
	- In-house software
	- Customer goodwill and reputation
- Reliance on historical data: Nature of future attacks/attackers are, in principle, unpredictable
- Monetary values give a false impression of precision
## Qualitative risk analysis
- Impact ranked with *ordinal numbers*
- Instead of using probabilities, use categories (high, medium, low)
- Pros: 
	- simpler as need not determine exact monetary values of assets or probability of different threats succeeding
	- easy to involve different parties
- Cons:
	- even more subjective
	- no single number for decision support
	- no basis for cost-benefit analysis

# Example

---
References: