---
Tags: 
Created: 2023-03-13 00:33:55
---
(Links:: [[Information Management for CS]])
Information systems can be viewed as a discrete dynamic system whose behavior can be modeled as a transition system.
# Discrete Dynamic Systems
- systems posses a state that is subject to change

> [!warning] Dynamic systems depend on the conceptualization
> Continuous systems can be viewd in a discrete manner and vice versa

> [!important]+
> Systems that change states in discrete jumps are *discrete systems*
> > [!example]
> > Railroad network
> 
> Systems that change states in continuous jumps are *continuous systems*
> > [!example]
> > turning wheel

# State and State Space
> [!example] Contents of Medicine Cabinet
> Painkiller: 14
> Sleeping pills: 9
> Antipyretics: 8
> - gives a description of the state of the medicine cabinet at a particular moment

- only include information in a state diagram that is relevant for the system
- serveral ways to describe states of a system depending on needs of the users
- *state description*: represents all things that may change and whose change is relevant for the system
- *state space $S$*: set of all possible states of a system

> [!example] State space for medicine cabinet
> $S =\{(x,y,z)|x,y,z \in \{0,\dots,19\}\}$

# Transitions and Transition Systems
- *atomic* changes are abstracted since the transition is not important
- state changes that take considerable time can be split into two state changes: start of state change and completion of state change -> 

---
References: