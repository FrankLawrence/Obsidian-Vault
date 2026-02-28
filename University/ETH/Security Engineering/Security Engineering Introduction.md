---
Tags: 
Created: 2025-09-17 08:25:00
---
(Links:: [[Security Engineering]])
# Complexity of Secure Systems
- Are notoriously difficult due to the many *system states* it may take, along with massive code bases
- Modern systems are networked information systems (NIS)
- Systems usually make use of **commercial off-the-shelf components** (COTS) for quicker, feature-orientated software
- Industry average includes 50-60 defects per 1000 lines of code, and is reduced to 2-4 after applying secure development methods
# Security Engineering
- [[CIA Security Triad | CIA]] is concerned with **absence of abuse**, which usually leads to *restricting* system behavior.
	- It is more rewarding for adding features to software, and usually a direct interest for users -> **security is an after thought**
- Hackers are not typical users
	- Systems should work correctly in their intended operating environment
	- A system is **safe** if the environment prevents it from entering an unsafe state

> [!question] What is security engineering?
> $$\text{Security Engineering} = \text{Software Engineering} + \text{Information Security}$$

# Software Development
- Writing code in a one-off matter (*code-and-fix*) is not beneficial for larger projects
	- not transparent, without any clear guidelines
	- disastrous when programmer quits (technical dept)
- For this we make use of [[SEP Lecture 1#Software Engineering Processes|Software Engineering Processes]]

> [!info]- Penetrate-and-patch development
> 1. Release System
> 2. Beta-testing through pentesters
> 3. Vulneraiblities discovered which leads to either
> 	1. disclosure to developers
> 	2. exploitation and released in the world
> 	3. used by nation states in cyberwarfare
> 4. Race to develop an bug-fix
> 5. GOTO 2

- Popular [[SEP Lecture 1#Software Process Models|Software Process Models]] include the waterfall model
## Waterfall
- Development decomposed into 5 phases (*phase model*) which develop a document or program
- aka. **Systems Development Life-Cycle** (SDLC)
### Requirements Engineering
 - **Objective**: retrieve requirements from customer and analyze
 - Use **models** to better describe (non-)functional requirements
 - Activities: Analyze risks, determine priorities, study feasibility, make business case
 - *Security* requires determining critical data to develop access control (authorization)
### Design
- Categories requirements into soft- and hardware requirements
- Design of subsystems (recursively)
- Determine algorithms and data structures
- Again use structured design methods like models (e.g. [[UML]])
- *Security* requirements map to key features: Encryption, access control, [[PKI]], ...
### Implementation
- Different subsystems require further design choices to be implemented
- Use of libraries and frameworks -> speeds up development but propagates errors
- *Security* requires understanding of impact of application and use of security tools (programming language, static analyzers, ...)
### Validation and verification
- Line-by-line team code review
- Use static analysis programs to find bugs and *vulenrabilities*
- Formal verification methods
- black- and white-box testing
	- test different "sized" units (smallest to largest)
	- *security* tests using penetration testing

> [!question] How do objectives/guarantees of these activities differ?
### Operation and maintenance
- Installation, patches, improvements, technology upgrades,...
- Secure distribution of code
- configuration and setup, e.g. PKI
- backup and failure recovery methods
- User education on using software
- Help desk support and emergency response
### Advantage
- clearly structured process
> [!info] Waterfall model process
> | **Activity**             | **Result**                             |
> | ------------------------ | -------------------------------------- |
> | Requirements analysis    | Feasibility study, requirements sketch |
> | Requirements definition  | Requirements plan                      |
> | System specification     | Functional specification               |
> |                          | Test plan                              |
> |                          | Development of user documentation      |
> | Architecture development | Architecture specification             |
> |                          | System test plan                       |
> | Interface development    | Interface specification                |
> |                          | Integration test plan                  |
> | Detailed development     | Specification, unit test plan          |
> | Programming              | Program                                |
> | Unit test                | Report                                 |
> | Module test              | Report                                 |
> | Integration test         | Report, final user documentation       |
> | System test              | Report                                 |
> | Acceptance test          | Report, documentation                  |
> 
>  **The output of one phase is the input to the next**
### Disadvantage
- Requirements must be known from the start, before design -> no flexibility later on when requirements change
- Design of system is purely hypothetical, and the implementation should fit 

> [!failure] The assumptions are too strong!
> Requirements are usually imprecise and develop naturally

- A first iteration of the model is only visible after development which often does not meet the requirements of the customer

> [!caution] Late development hides many risks
> 1. Technological: misunderstanding how systems work
> 2. Conceptual: misunderstanding what customers want
> 3. Personnel: developer team leaving
> 4. Users always *hate* what they get after waiting -> testing only at the end

- unidirectional process pushes problems ahead instead of resolving them in a prior step
- allowing feedback for improvement from later phases removes any sense of checkpoint
## V-Model
- Used by german military and administrative projects
- Built from different submodels including system development, configuration management, project management
- Heavyweight variant of waterfall
## Rational Unified Process
- Waterfall model, with incremental adjustments (mini projects) -> minimizes risk
	- *"plan a little, design a little, code a little"* (hard parts first)
- This integrates all participants simultaneously (testers, integrators, documenters, ...)
- **Phases**:
	- Inception: Rough system definition for initial costing
	- Elaboration: Mitigate key risk items or redesign/cancel project
	- Construction: build first release
	- Transition: improving through beta testing and training users
## Agile Methods
- lightweight model that puts software first
- less documentation, more implementation in concrete phases
- interaction with the customer for testing
- scrum, extreme programming, ...
## DevOp
- Time spent using the system, not development
- Sysadmin appraoch
	- Adapting to runtime events as they come
	- **Easy to implement**, but scales poorly due to increase in system's complexity and traffic
	- Usually incentives of developers (features) and sysadmins (availability) are contradictory
- DevOps approach
	- requires good engineers; responsibilities include managing availability, performance, change management, monitoring, emergency response ...
	- **scalability** achieved through automation
---
> [!question] Where does security fit in?
> There is no standard development process that explicitly supports security. Security usually treated as an ad-hoc add-on at the end.

# Summary
- Methods and tools are needed to master the complexity of software produciton
- Security needs particular attention
	- security aspects are typically poorly engineered
	- systems usually operate in highly malicious environments

---
References: