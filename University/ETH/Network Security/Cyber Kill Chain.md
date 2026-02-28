---
Tags: lecture
Created: 2026-01-22 09:12:52
---
(Links:: [[Network Security]])
- Attackers try to make long operations short to break them
- What the attacker is trying to attack might not be what you find important -> weaker security
- Attacker needs to be right needs to be right *only once*, defender needs to be *always right*
# Cost of detecting errors
- Any detector has to balance inevitable *false alerts* and *missed detections*
- Impact of a detected attack is low (from attackers side), while cost of false positives *is high* for the defender 
  -> An asymmetry that gives attackers a **systematic advantage over defenders**

# Attack Types
> [!definition] Safety
> - Protects against *accidents*
> - The environment does not adapt to bypass safety measures


> [!definition] Security
> - Aims to protect against *deliberate action*
> - Deliberate attacks are driven by an adaptive attacker

- **Targeted attacks** specify a *singular person/entity* to attack
	- Actors have *clearly defined objectives* and targets that they pursue consistently
- **Opportunistic attacks** aim to attack *many people*, going after those which are found to be *vulnerable*
	- Use of automated software on indiscriminate personal and corporate targets

> An automated attack tool *does not know* how uninteresting you think you are!

- **Direct Attack**: Attacker directly attacks target
- **Indirect Attack**: Attacking the intended target through an intermediary
	- Attacker might be oblivious because of *trusted intermediary*

> [!info] Supply Chain Attack
> - Attacker targets a large group *through an intermediary* (supplier)
> - super-efficient and scalable
> - Compromising one supplier potentially *exposes all clients*
> - Exploit inherent trust relationship between supplier and client
# Attackers Perspective
- Attacker objectives (having hacked devices): 
	- Control hijacked devices
	- Avoid detection of compromise
	- Avoid identification of attacker
	- Persist to maximize profit
	- Scale
- Controlling hacked devices at scale
	- A [[DDoS Attack#(IoT) Botnets|botnet]] is a network of computers or devices that are infected and centrally controlled without the knowledge of the devices' owners
	- *Botnet resiliency*
		- Decentralized infrastructure, no single point of failure  
		- Botnets operate across international borders  
		- Different legal systems and jurisdictional issues impede investigation

> When users don't notice any problems, they assume their system is secure and unaffected

- Botnet takedown
	- Fighting botnets requires coordinated efforts
	- Law enforcement, cybersecurity experts, and international organizations 
	- E.G. Europol "Operation Endgame”
		- Coordination with 12+ countries  
		- Lead to 4 arrests and 16 location searches. 
		- Over 100 server and 2’000 domains seized
# Attack Phases (Cyber Kill Chain)
## Attacker objective

### Main Attack Phases

- Gaining initial access in target environment  -> Get-In
- Moving laterally and compromising additional systems  -> Get-Through
- Execute attack objective, then leave undetected -> Get-out

### Attack Preparation

- Passive information gathering without touching the target -> No detection
- Open Source Intelligence (OSINT) -> No detection

## Attack modelling

Categorize adversary behaviour as *tactics*, *techniques*, and *procedures* (TTPs) along the typical phases of an attack -> **Mitre ATT&CK Model**

- **TACTICS**: Adversaries' objectives across stages of the attack lifecycle (e.g. initial access, persistence, data exfiltration)
- **TECHNIQUES**: Specific methods to achieve tactics, such as phishing or credential dumping
- **PROCEDURES**: Detailed examples of how techniques are executed in real-world attacks.
## Phases of Attack
- Mitre ATT&CK techniques are mapped onto 14 phases of attack, which can be mapped to the attackers objectives *Get-In*, *Get-Through*, and *Get-Out*

## Get-In
- Initial Access  
- Command and Control 
- Execution  
- Persistence  
- Defense Evasion
## Get-Through
- Execution  
- Persistence  
- Privilege Escalation 
- Credential Access  
- Discovery  
- Lateral Movement
## Get-Out
- Collection  
- Exfiltration 
- Impact
# Attacker Classification
![[37871.png|600]]
## Nation-state actors
Nation-state actors operate under government mandates to serve national interests, engaging in cyber espionage, sabotage, and influence. Well-funded and highly skilled, they use advanced tactics and resources to *persistently* target victims over *extended periods*.
## Semi-state actors
Semi-state actors work closely with governments to advance state interests while maintaining unofficial ties. Operating in a gray area, they receive state support but allow plausible deniability, enabling them to undertake actions that states may avoid due to diplomatic risks.
## Cybercriminals
Cybercriminals, driven by financial gain, use advanced skills, black-market tools, and stolen data to fund operations. Operating in organized groups, they adapt tactics as the cybercrime landscape evolves.
Cyber criminals face the same challenges to secure their systems and operations as we do. E.g. Conti group suffered a data breach and had its internals leaked in 2022.
## Hacktivist
Hacktivists, driven by ideological, political, or social causes, target entities they oppose to disrupt or expose them. Their causes include internet freedom, anti-censorship, and social justice, using moderate to advanced technical skills.
# Take Home Message

- What determines your risk isn’t your own perspective, but how an attacker perceives you as a potential target.
- Attackers are early adopters; their agility, high risk tolerance, and freedom from bureaucracy or ethics let them exploit new technologies faster than defenders can adapt.
- Fixed assumptions about security effectiveness are obsolete. Continuously challenge and update any security model that relies on outdated limits of cost, availability, or capability.
- Attackers evolve — accidents do not. Distinguish clearly between safety measures (protection from random failures) and security measures (defense against intelligent adversaries).
- Security strategy must go beyond prevention to include early detection, rapid containment, and effective response to breaches that evade initial defenses.
- Cybercrime platforms like Ransomware-as-a-Service (RaaS) make sophisticated attacks accessible to those with minimal technical expertise, expanding the threat landscape.


---
References: