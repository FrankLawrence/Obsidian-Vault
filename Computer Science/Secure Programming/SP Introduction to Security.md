---
Tags: 
Created: 2023-10-02 02:06:41
Links: "[[Secure Programming]]"
---
(Links:: [[Secure Programming]])
# What is security according to ISO?
**Information security** follow three rules specified by the [[CIA Security Triad]]: Preservation of confidentiality, integrity, and availability of information
## What is security in practice? 
- Protecting computer system against deliberate attacker
- **Threat model** specifies what attacker can do
# Examples
- ILOVEYOU: love letter sent via email and runs script on download
	- Security problems
		- curiosity is easily abused (social engineering)
		- no warning attachment will be executed
		- program from untrusted source can do sensitive operations
	- Impact: arbitrary execution on user's behalf
	- Threat model: script kiddie
		- anyone who can write VBScript could do this
- Heartbleed
	- OpenSSL implements crypto protocols and algorithms
	- widely used for secure HTTP, for example by Apache web server
	- new feature allowed attacker to obtain cryptographic key
	- Security problems
		- C is unsafe
		- Relies on programmer to check bounds
	- Impact: loss of confidentiality
	- Threat model: skilled hacker
- DDoS 
	- Servers are unable to handle all the requests
	- millions of devices such as modems, printers and cameras infected by botnet malware
	- Security problems: many embedded devices have default passwords, common passwords are tried
	- many attackers, hard to distinguish legitimate and malicious requests
	- Impact: loss of availability
	- Threat model: script kiddie
- Stuxnet
	- Abuses several previously unknown Windows bugs
	- Spreads over internet and through USB devices
	- Uses forged digital signatures
	- Hides itself from OS
	- Security problems: 
		- Various bugs and insecure configurations in Windows and other software
		- Security breach giving control over certificate signing key
		- Hard to install updates on industrial control systems (downtime unacceptable)
	- Impact: loss of availability 
	- Threat model: advanced persistent threat
## Threat model
- Defines what an attack can and cannot do
	- Level of technical ability?
	- Log in to system?
	- Intercept connections?
	- Physically control the hardware?
	- Insert backdoors ahead of time?
- A system is secure if CIA properties cannot be violated within the threat model
- Well-defined threat model helps find weakness and design effective defenses

Security is hard because developers need to find all weaknesses whilst attackers only need to find one weakness. Security can increase costs and worsen user friendliness. It is never 100% foolproof and finding weaknesses is hard until attacked. This all needs to be accomplished on many levels e.g. Hardware, OS, Application ...

---
References: