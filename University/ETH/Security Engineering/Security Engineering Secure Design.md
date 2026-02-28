---
Tags: 
Created: 2025-12-16 18:02:11
---
(Links:: [[Security Engineering]])
> [!info] Saltzer nad Schroeder's eight principles:
> 1. Economy of mechanism: keep things simple
> 2. Fail-safe defaults: deny access by default
> 3. Complete mediation: every access request should be inspected
> 4. Open design: can have many reviewers / users can convice themselves
> 5. Separation of privilege: e.g., requires two separate keys to unlock
> 6. Least privilege
> 7. Least common mechanism: non-interference
> 8. Psychological acceptability: usable security
# Risk Management and Safeguards
- **Avoidance controls**  
	- Safeguards used to proactively minimize risk of exploits
	- Either reduce their likelihood or impact 
> [!example]
> - Encryption and authentication: PKIs, disk encryption  
> - System security architecture (DMZ, ZTA, MAC filtering...) 
> - Secure communication (end-to-end, [[Transport Layer Security|TLS]], IPSec, link-layer...) 
> - Physical security  
> - Interruption prevention
> - Procedural measures: information security
- **Assurance**
	- Tools and strategies to ensure the effectiveness of existing controls and safeguards
> [!example]
> - Application security reviews 
> - Testing with respect to recommendations (e.g., NIST 800-115)
> - Penetration testing and vulnerability assessments 
> - Periodic perimeter scans 
> - Regular updates/patches 
- **Detection**
- Techniques and programs to ensure early detection, interception, and response to security breaches 
> [!example]
> - intrusion detection systems
> - Also virus scanners and audits
- **Recovery**
- Planning and response services to rapidly restore a secure environment and investigate sources of a security breach
> [!example]
> - business continuity planning, crisis management, disaster recovery planning (e.g., see BSI Standard 100-4)
# Security design principles and best practices
- **Use proven patterns and principles**, e.g.
	- Avoid security by obscurity
	- Least privilege
	- Keep security mechanisms simple
	- Defense in depth

> [!tip] Design Principles
> 1. Use standards and best practices
> 2. Consistent security level
> 3. Take appropriate measures at right level
> 4. Use mature libraries
> 5. Use proprietary solutions as a last resort
> 6. Generate implementations to avoid programming errors
# Automatically generating security implementation
# Security design options
Countermeasures can be categorized as follows:
1. Integrate or configure existing security mechanisms 
   Utilize system security mechanisms
2. Implement security functions in application logic 
3. Refactor the software architecture
## Security mechanism integration
- Pros
	- Baseline protection (standard recommended measures, e.g., BSI)) 
	- No application support required, so good for black-box integration 
	- Usually well-understood by IT departments and administrators
- Cons
	- Application layer attacks cannot be stopped at the network level 
	- Nontrivial, e.g., deep packet inspection is complex ($\leadsto$ vulnerabilities) and causes bottlenecks
## Implement Security in Application
- Some countermeasures best implemented within application
    - I/O validation & application-specific checks (e.g. input credit card number)
    - Application-level encryption (for communication or of database content)  Access control
- Requires that security design is integrated into the development process
- Use of standard security APIs and modules is advisable
- Pros
    - Best fit to application (end-to-end security argument)  
    - No additional system components (e.g. network appliances), reduces costs for licensing and operation
- Cons
	- Expensive (depending on development method) 
	- Error prone, test intensive
## Refactor
- Reduce attack surface
- Split systems into highly-critical and less-critical parts
- Allocate security-sensitive functions to other systems
- Pros  
	- Simplifies security design (also cheaper and less error-prone)
	- Sometime the only way to achieve security goals 
- Cons
	- Significant impact on overall project  
	- May have negative impact on other functions 
	- Causes strongest resistance and conflicts
# Security design as an iterative process

---
References: