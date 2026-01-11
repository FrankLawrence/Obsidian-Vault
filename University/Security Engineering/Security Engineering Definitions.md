# Security Engineering - Key Definitions

## Secure Coding & Implementation

### Buffer Overflow

A condition where data is written past the end of a buffer's allocated memory region, potentially overwriting adjacent memory areas including return addresses and control flow information.

### Format String Vulnerability

A security flaw where user-controlled input is used as the format string in functions like `printf()`, allowing attackers to read/write arbitrary memory locations using format specifiers like `%x` and `%n`.

### Race Condition

A situation where program results depend on the timing/scheduling of threads or processes, potentially allowing attackers to exploit the time gap between security checks and resource usage (TOCTOU - Time Of Check Time Of Use).

### SQL Injection

An attack where malicious SQL code is inserted into application queries through unvalidated user input, allowing unauthorized database access or manipulation. Prevention: use parameterized queries/prepared statements.

### Cross-Site Scripting (XSS)

An attack where malicious scripts are injected into web pages viewed by other users. Types include:

- **Persistent**: Malicious code stored on server (e.g., in database)
- **Reflected**: Malicious code reflected back from server immediately

### Cross-Site Request Forgery (CSRF)

An attack forcing authenticated users to execute unwanted actions on a web application by exploiting their existing session.

### Same-Origin Policy

A security policy preventing information flow between web pages from different origins (domain, protocol, and port must match).

### Content Security Policy (CSP)

A standard preventing XSS and code injection attacks by defining whitelists of trusted content sources in HTTP headers.

## Web Security

### Session Management

HTTP is stateless, so sessions are implemented using:

- **Cookies**: Persistently stored data on client (≤4KB)
- **Session Tokens**: Keys for server-side database containing session information

### Session Attacks

- **Session Hijacking**: Overhearing/stealing session tokens to impersonate users
- **Session Prediction**: Guessing session IDs based on predictable patterns
- **Session Fixation**: Attacker fixes an unauthenticated session ID and tricks victim into authenticating it

## Code Scanning & Testing

### Static Analysis

Analyzing source code without executing it, including type checking, style checking, and property verification.

### Dynamic Analysis

Analyzing programs by executing them, including testing and runtime monitoring.

### Fault Model

A description of a class of common programming errors/faults that testing should target.

### Code Coverage

The percentage of code elements (statements, branches, paths) executed by tests.

### Mutation Analysis

A test adequacy measure where the program is systematically modified (mutated) and tests are evaluated based on their ability to distinguish mutants from the original.

### Taint Analysis

Tracking how attacker-controlled input propagates through a program to identify potential security vulnerabilities.

### Fuzz Testing

Providing malformed, semi-valid inputs to a program to discover vulnerabilities, particularly those causing crashes or unexpected behavior.

## Testing Fundamentals

### Functional Requirements

Specifications describing what a system should do (goal and I/O-oriented).

### Non-Functional Requirements

Specifications describing constraints on how the system operates (security, performance, usability, etc.).

### Safety Property

A property stating that "something bad never happens" - can be refuted through finite testing.

### Liveness Property

A property stating that "something good eventually happens" - cannot be refuted through finite testing.

### Test Oracle

A mechanism for determining whether a program's output is correct for a given input.

### Fault vs Failure

- **Fault**: The cause of incorrect behavior (bug in code)
- **Failure**: Observable deviation from expected behavior

## Security Testing

### S-Tests (System Tests)

Tests that refute the hypothesis: System ⊢ SPEC (system satisfies its specification), independent of adversary model.

### E-Tests (Environment Tests)

Tests that refute: (Environment ∥ System ∥ Adversary) ⊢ EA (environmental assumptions hold), dependent on adversary capabilities.

### Attack Surface

All interfaces through which adversaries can interact with a system (network ports, APIs, file system, etc.).

### Closed-World Assumption (CWA)

The assumption that "what hasn't been considered doesn't matter" - a common source of security vulnerabilities.

## Requirements Engineering

### Use Case

A scenario describing how actors interact with a system to accomplish goals.

### Misuse Case

A scenario describing malicious or unintended uses of a system that threaten security.

### Actor

A role representing users or other systems that interact with the system being modeled.

### Security Requirements

Requirements related to confidentiality, integrity, availability, authentication, authorization, non-repudiation, etc.

### Security Rationale

The logical structure: EA, SPEC ⇒ REQ where:

- **REQ**: Security requirement about resources
- **SPEC**: System specification
- **EA**: Environmental assumptions

## Access Control

### Role-Based Access Control (RBAC)

Access control model where permissions are assigned to roles, and users are assigned to roles:

- AC = PA ∘ UA (with role hierarchies: PA ∘ ≥ ∘ UA)

### Least Privilege

Security principle: grant only the minimum access rights necessary for users to perform their tasks.

### Reference Monitor

A security enforcement mechanism that mediates access to resources based on authorization policies.

### Authorization Constraint

Additional conditions (beyond role membership) that must hold for access to be granted, often expressed as logical formulae over system state.

## Privacy & Data Protection

### Personal Data

Any information relating to an identified or identifiable natural person.

### Purpose Limitation

Privacy principle: personal data can only be used for limited, predefined, and legitimate purposes.

### Data Minimization

Privacy principle: collect only the personal data necessary for specified purposes.

### Consent

Permission from a data subject for processing their personal data for specific purposes.

### Purpose-Based Access Control

Access control extended to include purposes: which subjects can access which data for which purposes.

## Model-Driven Security

### Model-Driven Architecture (MDA)

Development approach where models are primary artifacts, with automatic transformation to implementation code.

### SecureUML

A modeling language combining UML with RBAC concepts for specifying security policies at design level.

### Dialect

The "glue" between a security modeling language and system design language, identifying protected resources and actions.

### Metamodel

A model that defines the abstract syntax of other models (operates at meta-level M2 above model level M1).

### UML Profile

A lightweight extension mechanism for UML using stereotypes, tagged values, and constraints.

## Security Design

### Defense in Depth

Security principle: use multiple layers of security controls so that failure of one layer doesn't compromise the entire system.

### Security by Obscurity

A flawed approach relying on keeping implementation details secret rather than sound security design (should be avoided).

### Threat Model

A structured representation of potential threats to a system, including adversary capabilities, attack vectors, and vulnerabilities.

### Risk Analysis

Systematic process of identifying, analyzing, and evaluating security risks based on likelihood and impact.

### Countermeasure

A security control or safeguard implemented to mitigate identified risks.

## System Architecture

### DMZ (Demilitarized Zone)

A network segment that separates internal trusted networks from untrusted external networks, typically containing public-facing services.

### Three-Tier Architecture

Application architecture with separate presentation, business logic, and data tiers.

### Security Gateway

A network component that inspects and controls traffic between network segments, providing intrusion detection and prevention.