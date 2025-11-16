---
Tags: 
Created: 2025-11-07 17:48:30
---
(Links:: [[Security Engineering]])
# Motivation and objectives
- Requirements are specified (usually) in plain text, but must be translated to models
- In smaller environments, it's more common to `code-and-fix`, but code quality and structure is worse
- Usual system design process does not have specific step to address security aspects
	- Ad hoc integration has a negative impact on security

> [!example]+ Meeting Scheduler
> # Functional Requirements: 
> System should maintain a *list of users* and *records of meetings*. A meeting has an *owner*, a *list of participants*, a *time*, and a *place*. Users may carry out operations on meetings such as *creating*, *reading*, *editing*, and *deleting* them. A user may also *cancel* a meeting, which deletes the meeting and notifies all participants by email.
> 
> # Security Requirements:
> 1. **All users** can create new meetings and read all meeting entries
> 2. **Only meeting owner** may change the meeting’s data, cancel, or delete the meeting.  
> 3. However, a **supervisor** can cancel any meeting 

> [!question]
> - How to formalize requirements?
> - How to structure requirements in *multi-tier architectures*?
> - How to implement using *modern standards/technologies*?
> - How to keep requirements implementation consistent when technologies change?

To approach these challenges, we use **Specialized Model Driven Architecture**.

## Components of MDS
- **Models**:
	- Modelling languages combine *security* and *design* languages.
	- Models specify *security* and *design* aspects.
- **Security Infrastructure**: code and/or existing mechanisms
	- Assertions, configuration data, calls to library functions, ...
- **Transformation**: *parameterized* by component standard/technology
	- Examples: J2EE/EJB, .NET, CORBA, ...

# Background
- MDS makes use of [[UML Diagrams| Unified Modeling Language]] and is extensible with domain specific languages to **generate code**
## Model Driven Architecture
- A model presents a system view useful for *conceptual understanding*.
- When the models have **semantics**, they constitute formal specifications and can also be used for (rigorous) *analysis*, and *refinement*


<svg viewBox="0 0 150 210" width="100%" height="30vh" xmlns="http://www.w3.org/2000/svg">
	<defs> <marker id="arrowRed" markerWidth="5" markerHeight="5" refX="0" refY="2.5" orient="auto">
		<path d="M 0 0 L 5 2.5 L 0 5 z" fill="var(--color-red,red)" />
	</marker> </defs>
	<defs> <marker id="arrowWhite" markerWidth="5" markerHeight="5" refX="0" refY="2.5" orient="auto">
		<path d="M 0 0 L 5 2.5 L 0 5 z" fill="var(--text-normal,grey)" />
	</marker> </defs>
	<polyline points="120,180 -30,180 -30,110" style="fill:none; stroke:var(--text-normal,grey); stroke-width:2.5" marker-end="url(#arrowWhite)"/>
	<text x="-60" y="195" fill="var(--text-normal,grey) !important">MDA Process</text>
	<rect width="150" height="50" x="120" y="150" rx="10" ry="10"   style="fill:var(--background-secondary);stroke:var(--text-normal);stroke-width:2.5" />
	<text x="195" y="183" dominant-baseline="middle" text-anchor="middle">Deployment</text>
	
	<path d="M 210 125 q 35 -5 45 14" stroke="var(--color-red,red)" stroke-width="2" fill="none" marker-end="url(#arrowRed)"/>
	<text x="250" y="120" fill="var(--color-red,red) !important">Code</text>
	<rect width="150" height="50" x="60" y="115" rx="10" ry="10"   style="fill:var(--background-secondary);stroke:var(--text-normal);stroke-width:2.5" />
	<text x="135" y="148" dominant-baseline="middle" text-anchor="middle">Testing</text>
	
	<path d="M 150 90 q 35 -5 45 14" stroke="var(--color-red,red)" stroke-width="2" fill="none" marker-end="url(#arrowRed)"/>
	<text x="190" y="85" fill="var(--color-red,red) !important">Code (+ Business Logic)</text>
	<rect width="150" height="50" x="0" y="80" rx="10" ry="10"   style="fill:var(--background-secondary);stroke:var(--text-normal);stroke-width:2.5" />
	<text x="75" y="113" dominant-baseline="middle" text-anchor="middle">Implementation</text>
	
	<path d="M 90 55 q 35 -5 45 14" stroke="var(--color-red,red)" stroke-width="2" fill="none" marker-end="url(#arrowRed)"/>
	<text x="130" y="50" fill="var(--color-red,red) !important">Code (Platform Infrastructure)</text>
	<rect width="150" height="50" x="-60" y="45" rx="10" ry="10"   style="fill:var(--background-secondary);stroke:var(--text-normal);stroke-width:2.5" />
	<text x="15" y="78" dominant-baseline="middle" text-anchor="middle">Design</text>
	
	<path d="M 30 20 q 35 -5 45 14" stroke="var(--color-red,red)" stroke-width="2" fill="none" marker-end="url(#arrowRed)"/>
	<text x="70" y="15" fill="var(--color-red,red) !important">Mostly Text</text>
	<rect width="150" height="50" x="-120" y="10" rx="10" ry="10"   style="fill:var(--background-secondary);stroke:var(--text-normal);stroke-width:2.5" />
	<text x="-45" y="40" dominant-baseline="middle" text-anchor="middle">Analysis</text>
</svg>

The difference with this model is that much of **transformation is automated**.
## MDA: the Role of Standards
- MDA is an *O*bject *M*anagement *G*roup standard
	- Standards are political, not scientific, constructs
	- They are valuable for building *interoperable* tools and for the widespread *acceptance* of tools and notations used
- MDA is based on standards for
	- **Modeling**: the *U*nified *M*odeling *L*anguage, for defining graphical, view-oriented models of requirements and designs
	- **Metamodeling**: the *M*eta-*O*bject *F*acility, for defining modeling languages, like UML
## Unified Modeling Language
[[UML]] belongs to a family of graphical languages designed for object-oriented-modelling, where each language is tailored for a *specific view* of the system, with their own **abstract syntax** for building blocks, and a more **concrete syntax** for display.
UML includes the *O*bject *C*onstraint *L*anguage, which uses first-order logic/[[Predicate logic]] for more expressive statements. OCL is widely adopted, and tool support is extensive, however, it is not yet a *formal method*.

[[Class Diagram|Class Diagrams]] specify a set of objects with common *services* (methods), *properties* (attributes) and *behaviors* (associations).
## Extensibility and Domain Specific Languages
We can extend conventional UML by defining new [[#Profiles Extending Core UML|profiles]], or at the level of [[#Metamodels|metamodels]].
### Profiles: Extending Core UML
- UML is defined by a metamodel: *core UML*
- Core UML can be extended by defining a **UML profile**: Introduce modeling primitives by declaring *stereotypes* and *tagged values* that subtype and extend core UML types, and *OCL constraints* can be used to formalize syntactic well-formedness restrictions.

> [!example]
> A class with stereotype `<<Entity>>` represents business objects with an associated persistent storage mechanism (e.g. table in relational database)

- Profiles are useful for light-weight specialization, but substantial changes require metamodels to define languages directly

> [!example] UML Profile Examples
> ![[UML Profile Examples.svg|600]]

### Metamodels
- A **metamodel** defines the (abstract) syntax of other models. Its elements, *metaobjects*, describe *types* of model objects
- MOF is a standard for defining metamodels

| Meta level | Description                                   | Example elements                |
| ---------- | --------------------------------------------- | ------------------------------- |
| M3         | MOF model                                     | MOF Class, MOF Attribute        |
| M2         | Metamodel, defines a language                 | Entity Attribute                |
| M1         | Model, consisting of instances of M2 elements | Entities "Meeting" and "Person" |
| M0         | Objects and data                              | Persons "Alice" and "Bob"       |

> [!info]- MOF appearances
> ![[Metamodeling.svg|600]]

- Abstract syntax of metamodels defined using MOF.  
	- Metamodels may be defined using UML notation.
	- Supports OO-metamodels, using concepts like subtyping.
- Concrete syntax of DSL defined by a UML profile
- MOF/UML tools automatically translate models in concrete syntax into models in abstract syntax for further processing

## Code generation
- In the EJB standard, beans are
	- *Server-side components* encapsulating application business logic
	- *Java classes* with appropriate structure, interfaces, methods, ... + *deployment information* for installation and configuration
- Feature implementations that can be automated are transactions, persistence, concurrency control, logging, remote method invocation, naming and directory services, security ...
- Generation rules map each model element into a part of EJB
- The translation produces *Java code* and *XML deployment descriptors*

> [!example] MDA Generation
> - **Entity** -> EJB component with implementation class, interfaces (local, home, remote, ...), factory method create, finder method `findByPrimaryKey`, ...
> - **Entity Attribute** -> getter/setter methods
>   ```java
>   date getStart() {return start;}
>   void setStart(date start) {this.start = start;}
>   ```
> - **Entity Method** -> method stub
>   ```java
>   void notify() {}
>   ```
> - **Association Ends** -> schema for maintaining references
>   ```java
>   Collection getParticipants() { return participants; }
>   void addToParticipants(Person participant) {participants.add(participant);}
>   void deleteFromParticipants(Person participant) {participants.remove(participant);}
>   ```
> - **Stereotype** -> parts of the deployment descriptor
# Secure Components
# Semantics
# Generating security infrastructures
# Privacy
# Experience and conclusions
# Appendix: Secure Controllers

---
References: [[model-driven.pdf]]