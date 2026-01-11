---
Tags: 
Created: 2025-12-16 01:11:20
---
(Links:: [[Security Engineering]])

> [!question] What must we know to assess a system’s security?
> 1. Organization’s assets, their value, and the system itself
> 2. Vulnerabilities of the assets that can be exploited 
> 3. Threats, their sources and impact  
> 4. Effectiveness of countermeasures  
> 5. Resulting risks

# Classical approaches to modeling faults
- Safety engineering and associated methods are better established 
	- **Failure Modes and Effects Analysis**: bottom-up, textual 
	- **Fault Tree Analysis**: top-down, graphical

## FMEA
- Consider each part of the entire system
- How may it or its subsystems fail? 
  **Fault**: Inability to function or an undesired functionality  
  **Failure**: Occurrence of a fault  
  **Failure mode**: manner which fault occurs, i.e., way element fails, 
  e.g., breakage, compromised integrity, ...
- Analysis based on historical data, expert opinions, ...
- Rank failure modes, e.g., 1–10 
  Occurrence: relative probability of malfunction occurring   
  Severity: Relative severity of worst possible outcome w.r.t. system functions (Non)Detectability: Probability that failure will be detected/corrected
- **Criticality** = Occurrence × Severity
- **Risk Priority Number** = *Occurrence* × *Severity* × *Detectability*
  E.g., RPN ∈ {1, ..., 1000}

> [!info]- FMEA Procedure
> 1. Define system to be analyzed
> 	- Mission, environment profiles  
> 	- Internal and interface functions, expected performance, constraints, failure definitions  
> 2. Construct [[block diagram|block diagrams]] of systems  
> 3. Identify potential item and interface definitions  
> 4. Evaluate and rank failures  
> 5. Identify possible causes and appropriate actions  
> 6. Corrective design: take actions to eliminate/reduce high-risks FMs 
> 7. Documentation

> [!example]- Car Headlight
> $$
> \begin{array}{|c|c|c|c|c|c|c|c|}
> \hline \text{Failure mode}&\text{Effect}&\text{Cause}&S&O&D&RPN&\text{Criticality}\\ \hline
> \text{Lights not on}&\text{Inoperable at night}& \begin{array}{} \text{Battery dead} \\ \text{Broken wire} \\ \text{Headlight out} \\ \text{Switch corroded} \\ \text{Switch broken} \end{array}&
> \begin{array}{} 8 \\ 8 \\ 8 \\ 8 \\ 8 \end{array}& \begin{array}{} 8 \\ 3 \\ 10 \\ 2 \\ 3\end{array}&6& \begin{array}{} 480 \\ 144 \\ 480 \\ 96 \\ 144 \end{array}& \begin{array}{} 80 \\ 24 \\ 80 \\ 16 \\ 24 \end{array} \\ \hline
> \text{Lights not off}&\text{Battery dead}& \begin{array}{} \text{Switch short-circuits} \\ \text{Operator error} \end{array}&10&\begin{array}{}2 \\ 8 \end{array}&6&\begin{array}{}120 \\ 480 \end{array}&\begin{array}{}20 \\ 80 \end{array} \\ \hline
> \end{array}
> $$
> - Redesign 1: Use two headlights and a visual ”light on” indicator in the console
> - Redesign 2: Add an audible indicator when doors are open and lights are on

- FMEA may indicate conditions that are not controllable by the system, e.g., security breach at one particular point
	- This gives rise to new requirements for the involved components
	- Closest analogy is data pathways where we aggregate requirements as we move from classes to components to systems (coming up!)
## FTA
![[Insulin Pump FTA.canvas|Insulin Pump FTA]]
- Goal: identify conditions leading to system failure (top level event)
- Aims at finding the *sources* of a system failure
- Deductive top-down method
- Quantitative and qualitative
- Graphical representation of causal relationships
### Events
- *Primary events* (leaves)
  *Basic events*: no precursor; probabilistic e.g., bits flipped by cosmic rays
  *Undeveloped events*: no major effect alone on the system e.g., indication lamp fails
  *External events*: expected to happen; not a fault
- *Intermediate events*: Link primary or intermediate events via AND/OR gates  
- *Expanded events*: Need a separate fault tree to explain

- A **cut** is a set of events that, taken together, lead to the top level event 
- A **minimal cut** is a cut that is no longer a cut if an element is removed 
- In general, there are many cuts and many minimal cuts
- FTA starts with an undesired top-level event representing a violated security requirement
	- Decomposing it into possible causes may lead to lower-level causes and suggest new security requirements
	- This is the basis of [[#attack trees]]
# Impact analysis using design models
> [!definition] Safety
> Failures arise from **faults** occurring.
> E.g., when a critical part breaks.

> [!definition] Security
> “failures” are the **unwanted events** that occur when a **threat agent** materializes a **threat** through an **attack** that exploits a system **vulnerability**

> [!question] How do we determine the relevant threats against a system? 
> We begin by showing how to identify the critical parts

- Requirements for understanding threats
	- Identify **relevant** threat agents
		- Network attacker vs. inside attacker  
		- Script kiddies vs. organized criminals  
		- Curious competitors vs. government-supported espionage
	- Which assets might they be after? How skilled are they?
	- Understand the system
		- Design and implementation  
		- Configuration, patch level, ...  
		- Organizational procedures, system usage, ...
	- Where might vulnerabilities arise?  
	- How might they be exploited, alone or in combination?
- Dimensions to consider
	- Systems/Channels (get from UML deployment diagrams)
	- Attack
	- Asset

To find "**host spots**" of the system, we construct **data pathways** that show where data/assets travel, and determine with the help of CIA properties, what security requirements are needed for components.
# Attack modeling
> Identifying threats requires creativity as many systems have unique requirements and therefore unique threats. One must look at each service and ask “If I were an attacker, how could I possibly exploit this security service?” Any answer constitutes a threat (OWASP)

- Consider using misuse cases as a starting point
## Attack Trees
- Nodes are attacks (threats)
- Top-level goal may be obtained from misuse cases
- Refine as needed
    - Alternative attacks  
    - Composite attacks (AND)
- Assign attributes to nodes
    - Probabilities  
    - Estimated impact  
    - Compute probabilities, impact, or risk of cut sets
- **Basically fault trees, plus special roles/motives of attackers**
- We use UML deployment diagrams (pathways) to determine the *impact* of threats in the diagram
# Experience with techniques

---
References: