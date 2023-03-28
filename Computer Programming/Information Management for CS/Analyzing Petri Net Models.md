---
Tags: 
Created: 2023-03-26 17:38:38
---
(Links:: [[Information Management for CS]])
# Reachability Analysis
## Standard Petri Net Properties
- **Boundedness**: 
	- inspect every reachable marking that the place has a limited capacity $k$
	- A Petri net system is *k-bounded* if and only if no place p \in P contains more than $k$ tokens in any reachable marking
	- If $k$ is equal to one, the Petri net system is *safe*
	- bounded Petri nets always have a finite number of reachable markings
- **Terminating**: 
	- it always reaches a terminal marking for which no transition is enabled
	- if and only if every run is finite
- **Deadlock freedom**: 
	- terminal marking is a *deadlock*
	- if and only if at least one transition is enabled at every reachable marking
- **Dead transition**: 
	- if and only if $t$ is not enabled at any reachable marking
- **Liveness**: 
	- transition $t$ is *live* if and only if from every reachable marking $m$ there is a marking $m'$ reachable such that $t$ is enabled at $m'$
	- liveness and terminating exclude each other
	- petri net is live if every transition $t\in T$ is live
	- no dead transitions
	- a transition can be enabled from any marking
- **Home-marking**: 
	- marking can always be reached again
	- if and only if from any reachable marking we can reach $m$
	- *reversible* if inital marking is a home-marking
## Fairness
- transitions in a Petri net cannot fire infinitely often while being enabled infinitely often
- fairness is defined only on *infinite runs* of a Petri net
- **Impartial**: if and only if $t$ occurs infinitely often in every infinite run of the net
- **Fair**: if and only if $t$ occurs infinitely often in every infinite run of the net where $t$ is enabled infinitely often
- **Just**: if and only if $t$ occurs infiniteyl often in every infinite run of the net where $t$ is continuously enabled from a marking onward
- **Not fair**: if and only if $t$ is not just; that is, there is an infinite run of the net where $t$ is continuously enabled from a marking onward and does not fire any more
## Verifying Unbounded Petri Nets Using the Coverability Graph
- *abstract marking*: $\omega$ symbol denotes that place p2 is unbounded
- the reachability graph and the coverability graph of a bounded Petri net are equivalent
- the coverability graph of a Petri net is always finite
- A transition $t$ of a Petri net is dead if and only if it does not appear in the coverability graph
- A place $p$ of a Petri net is $k$-bounded if and only if $p$ does not contain more than $k$ tokens in any marking of the coverability graph
- Every run of a Petri net can be mimicked in the coverability graph
# Process Mining
## The Process Mining Spectrum
- concentrate on *events* that can be linked to relevant activites
	- order of events dictates actual business process
- *control-flow discovery*: automatically construct process model describing the casual dependencies between activities
	- given an event log, automatically construct a suitable process model describing the behavior seen in the log
- **Discovery**: no priori process model -> model constructed from event log ($\alpha$-algorithm)
- **Conformance**: priori process model -> model used to check whether reality as recorded in log conforms to the process model
	- we may detect deviations, locate and explain these deviations, and measure the severity of these deviations
- **Extension**: priori process model -> model extended with new aspect or perspective
	- calculate waiting time distributions for activities with time stamp logs
	- extends given process model with conditions for each decision
## Process Discovery
> [!definition] Event log
> Let $A$ be a set of activities. A sequence $\sigma \in A^*$ of events is a *trace*, and a multiset $L \in A^*\to N$ of traces is an *event log*
- $\alpha$-algorithm only cares about the presence and not frequency of particular patterns relevant
- look at business processes that are instantiated multiple times

> [!definition] Workflow net
> A Petri net $N=(P,T,F)$ is a *workflow net* (WF-net) if and only if:
> 1. *Object creation*: $N$ contains an input place $i$ (the source place) such that $^\bullet i=\emptyset$
> 2. *Ojbect completion*: $N$ contains an output place $o$ (the sink place) such that $o^\bullet=\emptyset$
> 3. *Connectedness*: Every node in $N$ is on a path from $i$ to $o$

> [!definition] Soudness
> Let $N=(P,T,F)$ be a WF-net with input place $i$ and output place $o$. $N$ is *sound* if and only if:
> 1. *Option to complete*: For any reachable marking $m$, it is possible to reach the marking $|o|$
> 2. *Proper completion*: The only reachable marking that contains a token in place $o$ is the marking $|o|$
> 3. *Absence of dead activites*: There are no dead transitions

---
References: