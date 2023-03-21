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
- 

---
References: