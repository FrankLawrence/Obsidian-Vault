---
Tags: 
Created: 2023-03-13 00:34:34
---
(Links:: [[Information Management for CS]])
# Introduction to Petri Nets
## Modeling an Elevator
- *firing* a transition moves a *token* from one state to another
	- the direction of the *directed arcs* determine the token flow
- rules for transitions:
	- a place $p$ is an input place of a transition $t$ if there is an arc from $p$ to $t$
	- a place $p$ is an output place of a transition $t$ if there is an arc from $t$ to $p$
	- a transition can fire only if there is at least one token in each of its input places
- a transition that fires takes one token from each of its input places and puts one token in each of its output places
- a petri net contains zero or more places
	- each place has a name: *place label*
	- each transition has a name: *transition label*
- we can describe the places of a Petri net by the set $P$ of place labels - places are named as nouns, adjectives, or adverbs
- we describe the transitions of a Petri net by the set $T$ of transition labels - transitions are named as verbs to express action
- arcs are represented by an ordered pair
	- the set of arcs is a binary relation
	- $R_1 \subseteq P \times T$ contains all arcs connecting to input places
	- $R_0 \subseteq T \times P$ contains all arcs connecting to output places
	- $R_1 \cup R_0$ represents all arcs of a Petri net: *flow relation*

> [!definition] Petri net
> 1. $P$ is a finite set of *places*
> 2. $T$ is a finite set of *transitions*
> 3. $F \subseteq (P \times T) \cup (T \times P)$ is a *flow relation*

- a place $p$ is an input place of a transition $t$ if $(p,t) \in F$
- the set $^\textbullet t=\{p|(p,t)\in F\}$ defines all input places of a transition $t$
  $^\textbullet t$ is the *preset of t*
- a place $p$ is an output place of a transition $t$ if $(t,p) \in F$
- the set $t^\textbullet =\{p|(t,p)\in F\}$ defines all output places of a transition $t$
  $t^\textbullet$ is the *postset of t*
# The Behavior of Petri Nets
- the behavior is defined by the net structure, distribution of tokens over the places, and firing of transitions
- *marking* of a Petri net: determined by distribution of tokens over the places
- transitions change the marking through firing
- a transition must be *enabled* (at least one token in each of its input places) to be fired
- firing *consumes* (removes) from an input and *produces* (adds) a token in an output place
- *terminal marking*: reach a marking that does not enable any transitions
- marking $m'$ is *reachable* if from marking $m$ there is a series of firings that lead to marking $m'$

> [!definition] Marking
> A *marking* of a Petri net $(P,T,F)$ is a function $m:P\rightarrow \Bbb N$, assigning to each place $p \in P$ the number $m(p)$ of tokens at this place. The set $M$ of all *markings* of this net is the set of all such functions.

> [!definition] Enabledness
> In a Petri net $(P,T,F)$, a transition $t \in T$ is *enabled* at *marking* $m:P\rightarrow \Bbb N$ if and only if for all $p \in$ $^\bullet t, m(p)>0$

> [!definition] Transition firing
> For a Petri net $(P,T,F)$, let $w$ be the weight function and $m:P\rightarrow \Bbb N$ be the current marking. A transition $t \in T$ can *fire* if and only if it is enabled at $m$. The firing of $t$ yields a new marking $m':P\rightarrow \Bbb N$ where for all places $p\in P,m'(p)=m(p)-w((p,t))+w((t,p))$

> [!definition] Petri net system
> A *Petri net system* $(P,T,F,m_0)$ consists of a Petri net $(P,T,F)$ and a distinguished marking $m_0$, the *initial marking*

# Representing Petri Nets as Transition Systems
- state space $S$ consists of all possible distribution of tokens over the places $P$
- initial state $s_0$ defined by initial marking $m_0$
- "transition $t$ is enabled at marking $m$": $\forall x \in \,^\bullet t:m(p)>0$
- "the firing of transition $t$ in marking $m$ yields marking $m'$": $\forall p\in P:m'(p)=m(p)-w((p,t))+w(t,p)$
- An arbitrary Petri net system $(P,T,F,m_0)$ defines the following transition system $(S,TR,s_0)$:
	- $S=M=P\to \Bbb N$
	- $\begin{align}TR = &\{(m,m')\in S\times S \;|\; \exists t\in T:(\forall p\in \, ^\bullet t:m(p)>0 \\ &\land (m'(p)=m(p)-w((p,t))+w((t,p)))) \}\end{align}$
	- $s_0=m_0$

---
References: