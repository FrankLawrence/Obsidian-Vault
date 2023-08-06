---
Tags: 
Created: 2023-07-29 22:45:29
---
(Links:: [[Computer Networks]])
# Fundamentals of Network Security
- **Confidentiality**: keep information away from unauthorised users
- **Integrity**: information received is the actual information and hasn't been tampered with
- **Availability**: preventing systems and services from becoming unusable due to crashes, overload situations, or deliberate misconfigurations
- **Authentication**: determining who you are talking to before revealing sensitive information
## Fundamental Security Principles
- guaranteeing security is hard -> we try to improve security as much as we can by applying a set of security principles
1. **Principle of economy of mechanism**: Complex systems have a higher possibility of containing bugs and vulnerabilities, complex systems are hard to understand -> simple systems are good systems
2. **Principle of fail-safe defaults**: Make explicit rules who should have access to information, not who should not have access.
3. **Principle of complete mediation**: Every access to resources should be checked for authority
4. **Principle of least authority**: Any system should have just enough authority to perform its task ^3358f8
5. **Principle of privilege separation**: split the system into multiple [[#^3358f8|POLA]]-compliant components than a single component with all privileges
6. **Principle of least common mechanism**: minimise the amount of mechanism common to more than one user and depended on by all users
7. **Principle of open design**: the design should not be secret, cryptosystems should be secure even if everything about the system except the key is public knowledge
8. **Principle of psychological acceptability**: it should be clear why the rules and mechanisms are necessary
## Fundamental Attack Principles
1. **Reconnaissance**: Gather as much information on the target as possible
2. **Sniffing and Snooping**: intercept network packets
3. **Spoofing**: impersonating someone else

---
References:
