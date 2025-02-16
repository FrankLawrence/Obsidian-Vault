---
Tags: Math
Created: 2025-02-07 14:08:47
---
(Links:: [[Automata and Complexity]])
# Deterministic Finite Automata
> [!definition] DFAs
> A **deterministic finite automaton** consists of: 
> - a finite set $Q$ of **states**
> - a finite **input alphabet** $\Sigma$
> - a **transition function** $\delta : Q\times \Sigma\to Q$
> - a **starting state** $q_{0}\in Q$
> - a set $F\subseteq Q$ of **final states**


> [!example] DFA
> Let $M=(Q,\Sigma, \delta, q_{0},F)$ with $Q=\{q_{0},q_{1}\}, \Sigma = \{a,b\}, F=\{q_{0}\}$, $$\delta (q_{0}, a)=q_{0}\qquad \qquad \delta(q_{1}, a)=q_{1}$$ $$\delta(q_{0}, b)=q_{1} \qquad \qquad \delta(q_{1},b)=q_{0}$$
> We can also write the transition functions all in table notation:
> 
> $$\begin{array}{c|lcr} 
> \delta & q_{0} & q_{1} \\ 
> \hline 
> a & q_{0} & q_{1} \\ 
> b & q_{1} & q_{0} \\ 
> \end{array}$$
> 
> or even as a **transition graph**:
> - *states* are the *nodes* of a graph
> 	- *starting state* indicated by an *extra incoming arrow*
> 	- *final states* indicated by *double circle*
> - *arrows* with labels from $\Sigma$: $$q\overset{a}{\to} q' \text{ if } \delta(q,a)=q'$$
> ```mermaid
> flowchart LR
> 	A@{ shape: sm-circ, label: "Small start" }
> 	B@{ shape: dbl-circ, label: "q<sub>0</sub>" }
> 	C@{ shape: circ, label: "q<sub>1</sub>" }
> 	A --> B
> 	B -- b --> C
> 	C -- b --> B
> 	B -->|a|B
> 	C -->|a|C
> ```
^607939

A **configuration** of a DFA is a pair $(q,w)$ with a state $q$ and word $w \in \Sigma^{*}$. With this, the automaton is in state $q$ and reads the word $w$. The **step relation** $\vdash$ of $M$ describes an execution of the automaton: $$(q,aw)\vdash (q', w)\quad \text{if } \delta(q,a)=q'$$
With this we can also define $\vdash^{*}$ as the **reflexive transitive closure of** $\vdash$.

> [!example]-
> Take the [[#^607939|previous example]], then: $$(q_{0}, abba) \vdash (q_{0}, bba)\vdash (q_{1},ba)\vdash(q_{0},a)\vdash (q_{0},\lambda)$$
> We can also simply write $$(q_{0},abba)\vdash^{*}(q_{0},\lambda)$$

> [!question] Determine $M=(Q,\Sigma, \delta, z_{0},F)$
> ```mermaid
> graph LR
> 	A@{ shape: sm-circ, label: "Small start" }
> 	B@{ shape: dbl-circ, label: "z<sub>0</sub>" }
> 	C@{ shape: dbl-circ, label: "z<sub>1</sub>" }
> 	D@{ shape: circ, label: "z<sub>2</sub>" }
> 	A --> B
> 	B -->|b| D
> 	B -->|a| C
> 	C -->|a,b| D
> 	D -->|a| C
> 	D -->|b| B
> ```
> > [!info]- Solution
> > - states $Q = \{z_{0},z_{1},z_{2}\}$
> > - alphabet $\Sigma = \{a,b\}$
> > - transition function $\delta : Q\times \Sigma \to Q$: $$\begin{array}{c|lcr} 
> >   \delta & z_{0} & z_{1} & z_{2} \\ 
> >   \hline 
> >   a & z_{1} & z_{2} & z_{1} \\ 
> >   b & z_{2} & z_{2} & z_{0} \\ 
> >   \end{array}$$
> > - starting state $z_{0}$
> > - final states $F=\{z_{0},z_{1}\}$

## Paths in DFAs
For a word $w=a_{1}\cdots a_{n}, n\geq 0$, we write $$q_{0}\overset{w}{\twoheadrightarrow}q_{n}$$ if there are states $q_{1},...,q_{n-1}$ such that $$q_{0}\overset{a_{1}}{\to}q_{1}\quad q_{1}\overset{a_{2}}{\to}q_{2} \quad\cdots\quad q_{n-1}\overset{a_{n}}{\to}q_{n}$$
# Regular Languages

---
References: