---
Tags: 
Created: 2024-06-24 12:54:09
---
(Links:: [[Software Engineering Processes]])
# Unified Process
- An **iterative** software development process for building **object oriented** systems
- Uses OOA/D concepts
- Promotes the use of [[UML Diagrams]]
- Very flexible and open
- Uses practices from other agile methods:
	- From [[SEP Lecture 2 SCRUM#Extreme Programming|XP]]: test-driven development, refactoring, continuous integration, ...
	- From [[SEP Lecture 2 SCRUM#SCRUM|SCRUM]]: daily meetings

> The UP combines commonly accepted best practices, such as an iterative lifecycle and risk-driven development, into a cohesive and well-documented process description

![[Iterative, Incremental, Evolutionary Development.png|500]]

- Iterations may be considered a series of structured build-feedback-adapt cycles
- Main objectives: Address changes and minimize risks
- The design and requirements instability lowers over time
![[Unified Process.png|500]]
## Iterations and their results
- The result of each iteration is an executable but incomplete system
- The result is not ready to deliver into production
- The system may not be eligible for production deployment until after many iterations e.g. 10 or 15 iterations or more...
- The result of an iteration is not an experimental or throwaway prototype
- Iterative development is not prototyping
- The result is a production grade subset of the final system
## Unified Process & Changes
- Each iteration focusses on a small subset of the requirements, and quickly designing, implementing, and testing
- In early iterations the choice of requirements and design may not be exactly what is ultimately desired
- But taking a small step, before all requirements are finalized, or the entire design speculatively defined, leads to rapid feedback from the users, developers and tests
## Benefits
- Less project failure
- Better productivity
- Lower defect rates
- Early rather than late mitigation of high risks
- Early visible progress
- Early feedback, user engagement, and adaptation, leads to a refined system that meets the real needs of the stakeholders
- Managed complexity - development team is not overwhelmed by "analysis paralysis" or very long and complex steps
- The learning within an iteration can be methodically used to improve the development process itself, iteration by iteration
## Agile UP
- Keywords: adaptability and lightness 
- Examples: 
	- Requirements and designs are not completed before implementation; they adaptively emerge through a series of iterations, based on feedback 
	- UML applied with agile modelling practices 
	- No detailed plan for the entire project
## 4 Phases
- **Inception**: approximate vision, business case, scope, vague estimates 
- **Elaboration**: refined vision, iterative implementation of the core architecture, resolution of high risks, identification of most requirements and scope, more realistic estimates 
- **Construction**: iterative implementation of the remaining lower risk and easier elements, and preparation for deployment 
- **Transition**: tests, deployment 
![[UP Iterations and Phases.png|500]]
## Disciplines
- Disciplines: a set of activities (and related artifacts) in one subject area (e.g. activities within requirements analysis)
- In UP, an artefact is the general term for any work product: code, web graphics, database schema, text documents, diagrams, models,...
![[UP disciplines.png|500]]
## Disciplines and Phases
- During one iteration work goes on in most or all disciplines
- The relative effort across these disciplines changes over time
- Early iterations focus on requirements and design
- Later iterations, as the requirements and code design stabilize through a process of feedback and adaptation, focus on implementation and testing
# Rational Unified Process
- A detailed refinement of UP
- The result of each iteration is an executable but incomplete system; it is not ready to deliver into production
- The system may not be eligible for production deployment until after many iterations; for example 10 or 15 iterations
- UML based analysis and modelling

---
References: