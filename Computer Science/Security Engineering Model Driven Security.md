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
- UML is defined by a [[#Metamodels|metamodel]]: *core UML*
- Core UML can be extended by defining a **UML profile**: Introduce modeling primitives by declaring *stereotypes* and *tagged values* that subtype and extend core UML types, and *OCL constraints* can be used to formalize syntactic well-formedness restrictions.

> [!example]
> A class with stereotype `<<Entity>>` represents business objects with an associated persistent storage mechanism (e.g. table in relational database)

- Profiles are useful for light-weight specialization, but substantial changes require metamodels to define languages directly

![[UML Profile Examples.svg|600]]

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
> ![[ComponentUML Example.svg|400]]
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
> 
> ```java
> <entity>
> 	<ejb-name>Meeting</ejb-name>
> 	<home>scheduler.MeetingHome</home>
> 	<remote>scheduler.MeetingRemote</remote>
> 	<ejb-class>scheduler.Meeting</ejb-class>
> 	<persistence-type>Container</persistence-type>
> 	<prim-key-class>java.lang.String</prim-key-class>
> 	<reentrant>False</reentrant>
> 	</entity>
> ...
> <method-permission>
> 	<role-name>Supervisor</role-name>
> 	<method>
> 		<ejb-name>Meeting</ejb-name>
> 		<method-intf>Remote</method-intf>
> 		<method-name>cancel</method-name>
> 		<method-params/>
> 	</method>
> <method-permission>
> ...
> ```

# Secure Components
## Context: Models and Languages
- A *Security Design Language* glues two languages together
- Each language is equipped with an *abstract* and *concrete syntax*, a *semantics*, and a technology-dependent *translation function*

![[Security Design Language.svg|600]]
- Dialect bridges design language with security language by identifying which design elements are protected resources.
- UML employed for 
	- **Metamodeling**: Object oriented def. of language syntax (MOF)
	- **Notation**: Concrete language syntax for security design models
## Role-Based Access Control (RBAC)
- **Access control policies** specify which subject have rights to read/write which objects 
	- enforced by a *reference monitor* -> checks if *authenticated* users are *authorized* to perform actions
- There are two types of access control:
	- *Declarative* (RBAC): $$u \in \text{Users has } p\in \text{Permissions} :\Longleftrightarrow (u,p) \in AC$$
	- *Programmatic*: Uses assertions at relevant point in code and system environment to make decision
	- Both are often combined: "A user in the role customer may withdraw money from an account when he is the owner and the amount is less than 1,000 SFr"
- Declarative access control relies on *relations* to specify authorization

> [!example]-
%%> ![[Declarative Access Control.svg|600]]%%
> $$
> \begin{array}{ccc}
> \begin{array}{|c|}
> \hline \text{User} \\
> \hline
> \text{Alice} \\
> \text{John} \\
> \text{Bob} \\
> \hline
> \end{array}
> 
> \quad
> 
> \begin{array}{|c|c|}
> \hline \text{User} & \text{Permission} \\
> \hline
> \text{Alice} & \text{read file a} \\
> \text{Alice} & \text{write file a} \\
> \text{Alice} & \text{start application x} \\
> \text{Alice} & \text{start application y} \\
> \text{John} & \text{read file a} \\
> \text{John} & \text{write file a} \\
> \text{John} & \text{start application x} \\
> \text{Bob} & \text{read file a} \\
> \text{Bob} & \text{write file a} \\
> \text{Bob} & \text{start application x} \\
> \hline
> \end{array}
> 
> \quad
> 
> \begin{array}{|c|}
> \hline \text{Permission} \\
> \hline
> \text{read file a} \\
> \text{write file a} \\
> \text{start application x} \\
> \text{start application y} \\
> \hline
> \end{array}
> \end{array}
> $$

- Role-Based Access Control decouples users and permissions by roles, representing jobs or functions
- Formally, we can specify the following: Given
	- $\text{UA} \subseteq \text{Users} \times \text{Roles}$, and
	- $\text{PA} \subseteq \text{Roles} \times \text{Permissions}$

$$\text{AC} := \text{PA} \circ \text{UA}$$
i.e. $$\text{AC} \; := \; \big\{ (u,p) \in \text{Users} \times \text{Permissions}\; \vert \;\exists r \in \text{Roles} \;:\; (u,r) \in \text{UA} \land (r,p) \in \text{PA} \big\}$$

> [!example]-
> $$
> \begin{array}{ccc}
> \begin{array}{|c|c|}
> \hline \text{User} & \text{Role} \\
> \hline
> \text{Alice} & \text{User} \\
> \text{Alice} & \text{Superuser} \\
> \text{Bob} & \text{User} \\
> \text{John} & \text{User} \\
> \hline
> \end{array}
> 
> \quad
> 
> \begin{array}{|c|}
> \hline \text{Role} \\
> \hline
> \text{User} \\
> \text{Superuser} \\
> \hline
> \end{array}
> 
> \quad
> 
> \begin{array}{|c|c|}
> \hline \text{Role} & \text{Permission} \\
> \hline
> \text{User} & \text{read file a} \\
> \text{User} & \text{write file a} \\
> \text{User} & \text{start application x} \\
> \text{Superuser} & \text{start application y} \\
> \hline
> \end{array}
> \end{array}
> $$

### Extensions
1. Role hierarchy (for $\geq$ a partial order): $$\text{AC} := \text{PA} \; \circ \; \geq \;\circ\; \text{UA}$$
   Larger roles inherit permissions from all smaller roles
2. Hierarchies on users ($\text{UA}$) and permissions ($\text{PA}$)
3. *Authorization constraints*: formulae used to make stateful access control decisions.
   Example: a user in the role customer may withdraw money from an account *when he is the owner and the amount is less than 1,000 SFr*.

## Generalization to SecureUML
### SecureUML
- Uses MOF for its abstract syntax
- Uses a [[#Profiles Extending Core UML|UML profile]] for its concrete syntax
- **Key Idea**:
	- An access control policy formalizes the permissions to perform *actions* on *resources*
	- We leave the actions and resources open as *types* whose elements are not fixed
	- Elements specified during combination with design language

![[SecureUML.svg|900]]

- SecureUML uses [[#Role-Based Access Control (RBAC)|RBAC]] via `User`, `Role` and `Permission` classes
- Permissions allow a Role to perform *actions* on *resources*
	- `Resource`: base class of all model elements representing protected resources (e.g. "Class", "State", "Action")
	- `Actions`: Belong to the "Class" (like "Read", "Write", etc.)
- **Hierarchies** are specified using UML-aggregations
	- `UserHierarchy`: Users (and groups) are organized in groups
	- `RoleHierarchy`: Roles can be in an inheritance hierarchy
	- `ActionHierarchy`: E.g. "FullAccess" is a super-action of "Read"
	- `ActionDerivation`/`ResourceDerivation`: Details technical & omitted
- **Authorization constraints** restrict permissions, such that an *assertion* (additional condition) must hold in order to grant access.
  Example conditionals:
	- The *state of the resources* of the assigned actions
	- *Properties of method arguments* (name of the calling user)
	- *Global system properties* (time, date)

To define system users and roles we deploy the following:
- Users, Roles and Groups are defined by stereotyped classes
- Hierarchies defined using inheritance
- Relations defined using stereotyped associations

==TODO: Insert diagram==

> [!note]-
> User administration is *not* a design-time issue and in practice is *not* part of a design model (declared after system deployment)

- Modeling **permissions** require that actions and resources are defined
- Permissions allow many actions to be associated to a resource
- Concrete syntax could directly reflect abstract syntax
- Alternatively, use an *association class* to specify one [[Ternary association.svg|ternary relation]]
	- Attributes of association relate permissions with actions
	- Actions identified by resource name and action name

> [!example]
> ![[SecureUML Permissions.svg|900]]
> - When permissions are represented as an association class, they connect a *role* to a *resource* (**model anchor**)
> - Permissions (**action references**) assign actions to (1) the model anchor or (2) its sub-elements
> - The authorization constraints are built using [[#Unified Modeling Language|OCL]] with features such as
> 	- constant symbols: `self` and `caller` (authenticated entity of the caller)
> 	- attributes and *side-effect free* methods
> 	- navigation expressions (association ends)
> 	- logical (`and`, `or`, `not`) and relational (`=`,`>`,`<`,`<>`) operators
> 	- existentially quantified expressions (since OCL uses [[Predicate logic]])
> - Example: `caller = self.owner`
## Component modeling and combination
- **ComponentUML**: a class-based language for data modeling

![[ComponentUML.svg|600]]

- Example implementation: ![[ComponentUML Example.svg|400]]

Now let's try and [[#Context Models and Languages|combine our two languages]] (SecureUML and ComponentUML) by defining the **dialect**:
1. Both language metamodels  are combined using [[#Unified Modeling Language|OCL]] for *notation and well-formedness rules* (mostly automated)
2. Identify protected resources (using *subtyping*)
3. Identify resource actions
	- Defined using *named dependencies* from resource types to action classes (either `AtomicAction` or a subtype of `CompositeAction`)
	- 
4. Define action hierarchy

# Semantics
# Generating security infrastructures
# Privacy
# Experience and conclusions
# Appendix: Secure Controllers

---
References: [[model-driven.pdf]]