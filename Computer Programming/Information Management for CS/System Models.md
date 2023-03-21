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
- state changes that take considerable time can be split into two state changes: start of state change and completion of state change
- transitions can be represented by an ordered pair

> [!definition] Transition
> An ordered pair $(x, y)$ in which $x$ and $y$ are elements of the state space $S$-that is, $x,y\in S$

> [!info] Transition relation $TR$
> - if we consider all possible transitions of a system, then we obtain a set of ordered pairs of states
> - you obtain the set of all ordered pairs of states of a state space $S$ by forming the cartesian product $S \times S$
> - not all elements correspond to a possible transition -> $TR$ is a subset of the Cartesian product: $$TR \subseteq S\times S$$
- the *initial state* of a system is the state in which a system starts its operation

> [!definition] Transition System
> A triple $(S, TR, s_0)$ where $S$ is a finite state space, $TR \subseteq S\times S$ is a transition relation containing all possible state changes, and $s_0 \in S$ is the initial state.

> [!definition] State-transition diagram
> A directed graph in which the nodes represent the states of the transition systemm and the directed edges represent the possible transitions

# Transition Sequences and the Behavior of a System

> [!definition] Reachable state
> A state that the system can reach from the initial state after zero or more transitions
> > [!tip]-
> > determine reachability by following the arrows in the state-transition diagram
- *transition sequence*: sequence of states reached by following the arrows: $\langle s_1, ..., s_n\rangle$
- the set of all possible transition sequences from an initial state specifies the *behavior* of the system
- *terminal state*: no further transition is possible
- *deterministic system*: from every state, at most one transittion is possible

---
References: