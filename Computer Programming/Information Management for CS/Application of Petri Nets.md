---
Tags: 
Created: 2023-03-13 00:34:43
---
(Links:: [[Information Management for CS]])
# Modeling with Petri Nets
- to model a production system we must represent products machines, persons, and information flows
## The Role of a Token
- *physical object*: a product, a part, a drug, or a person
- *information object*: a message, a signal, or a report
- *collection of objects*: a truck with products, a warehouse with parts, or an address file
- *indicator of a state*: the indicator of the state in which a business process is or the state of an object, such as a traffic light
- *indicator of a condition*: indicates whether a certain condition if fulfilled
## The Role of a Place
- passive elements
- *communication medium*: a telephone line, a middleman, or a communication network
- *buffer*: a depot, a queue, or a post bin
- *geographic location*: a place in a warehouse, in an office, or in a hospital
- *possible state or state condition*: the floor where an elevator is or the condition that a specialist is available
---
- token represents physical object or information object -> places represent medium, buffer of geographic location
- token indicates state -> places correspond to possible states
- token indicates where condition if fulfilled -> place represents condition
## The Role of a Transition
- *event*: starting an operation, death of a patient, a season change, turning of a traffic light from red to green
- *transformation of an object*: repairing a product, updating a database or stamping a document
- *transport of an object*: transporting goods or sending a file
# Typical Network Structures
- **Causality**
	- relationship between two events in a system must take place in a certain order
- **Concurrency and Synchronization**
	- two tranistions x and y can fire independently of each other -> events happen in arbitrary order
	- to model an even tthat occurs before two concurrent events, we must *split* the flow of tokens into two branches
	- model sychronization in a Petri net as a transition with at least two input places
	- If transitions $x_1,x_2,...,x_k$ should happen synchronously, we must merge them into a single transition
- **Mutual Exclusion**
	- If transition $x$ fires, then transitions $y$ and $z$ are enabled, but only one of them will fire -> *choice* has to be made 
- **Place Capacities**
	- if place $p$ already contains $n$ tokens at a certain marking $m$, then on transition having place $p$ in its postset must be enabled at $m$ -> place $p$ has a *capacity* of $n$ tokens
- **Arc Multiplicities**
	- if there are $k$ arcs from a place to a transition, we consider this as an arc whose multiplicity if $k$

> [!definition] Petri net with arc mulltiplicites
> $(P,T,F,w)$ consists of a Petri net $(P,T,F)$ and a weight function $w:(P\times T)\cup (T\times P) \to N$. A transition $t\in T$ is *enabled at marking* $m:P\to N$ if and only if for all $p\in \, ^\bullet t, m(p)\leq w((p,t))$. An enabled transition $t$ can *fire* yielding a new marking $m':P\to N$, where for all $p\in P,m'(p)=m(p)-w((p,t))+w((t,p))$.

# Reachability Graphs
> [!question]
> - How many marking are reachable?
> - Which markings are reachble?
> - Are there any reachable terminal markings?

- from the initial marking we calculate the set of markings reachable form $m_0$ -> set represented as *reachability graph*
- nodes correspond to the reachable markings and its edges to the transitions moving the net from one marking to another
- each reachable marking is represented as a multiset 
- label each edge of the reahcability graph with the transition that fired in the correpsonding marking
- if a marking $m$ is reachable from the initial marking $m_0$, then the reachability graph has a path from the start node to the node representing marking $m$
- *run*: transition sequence
- run is finite if the path and hence the transition sequence is finite, otherwise *infinite*
- *coverability graph*: finite representation of the reachability graph

---
References: