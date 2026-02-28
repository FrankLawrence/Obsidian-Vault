---
Tags:
  - Math
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

> [!example]- DFA
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
- For a word $w=a_{1}\cdots a_{n}, n\geq 0$, we write $$q_{0}\overset{w}{\twoheadrightarrow}q_{n}$$ if there are states $q_{1},...,q_{n-1}$ such that $$q_{0}\overset{a_{1}}{\to}q_{1}\quad q_{1}\overset{a_{2}}{\to}q_{2} \quad\cdots\quad q_{n-1}\overset{a_{n}}{\to}q_{n}$$

> [!info] THEOREM
> $$q \overset{w}{\twoheadrightarrow} q' \iff (q,w)\vdash^{*}(q',\lambda)$$
# Regular Languages
- The [[Languages A&C|language]] accepted by DFA $M=(Q,\Sigma, \delta, q_{0}, F)$ is 
  $$\begin{align*} L(M) 
  &= \{w\in \Sigma^{*}\mid \text{$M$ has an accepting run for $w$}\} \\
  &= \{w\in \Sigma^{*}\mid (q_{0},w) \vdash^{*} (q,\lambda) \text{ with } q\in F\} \\ 
  &= \{w\in \Sigma^{*}\mid q_{0} \overset{w}{\twoheadrightarrow} q \text{ with } q\in F\} \end{align*}$$
- A language $L$ is **regular** if there exists a DFA $M$ with $L(M)=L$
- **DFAs are deterministic**: For every state $q\in Q$ and every symbol $a\in \Sigma$, the state $q$ has **precisely one outgoing arrow** with label $a$ 
	- there is only one path from starting state through the transition graph.
- If $L$ is a regular language, then $\bar L$ is also regular (flip all accepting and non-accepting states)
- If $L_{1}$ and $L_{2}$ are regular, then $L_{1}\cup L_{2}$ is regular. (same for $L_{1}\cap L_{2}$ and $L_{1}\setminus L_{2}$)
- Every **finite** language $L$ is regular
# Nondeterministic Finite Automata
NFAs are defined like DFAs, except that NFAs allow for:
- **Multiple starting states**
- **Any number of outgoing arrows** with the same label
- **Empty steps**: arrows labelled $\lambda$ (do not consume input)

We use $2^{Q}$ to denote the set of all subsets of $Q$: $$2^{Q} = \{S \mid S \in Q\}$$

> [!definition] NFAs
> A **nondeterministic finite automaton** consists of: 
> - a finite set $Q$ of states
> - a finite input alphabet $\Sigma$
> - a transition function $\delta : Q\times (\Sigma \;{\color{#F08} \cup \{\lambda\}})\to {\color{#F08}2^Q}$
> - a set of starting states ${\color{#F08}S\subseteq Q}$
> - a set $F\subseteq Q$ of final states

- Let $M=(Q,\Sigma, \delta, S,F)$ be a NFA. The **step relation** $\vdash$ of $M$ is defined on configurations by $$(q,a w)\vdash(q',w) \quad \text{if ${\color{#F08}q' \in}\; \delta(q,a)$ with $a \in \Sigma \,{\color{#F08}\cup \{\lambda\}}$}$$

- For a word $w$, we write $$q\overset{w}{\twoheadrightarrow}q'$$ if $w=\alpha_{1}\cdots \alpha_{n}$ for some $\alpha_{1},\cdots,\alpha_{n}\in (\Sigma \cup \{\lambda\})$ and there are states $q_{1},...,q_{n-1}$ such that $$q\overset{\alpha_{1}}{\to}q_{1}\quad q_{1}\overset{\alpha_{2}}{\to}q_{2} \quad\cdots\quad q_{n-1}\overset{\alpha_{n}}{\to}q'$$
- The language accepted by NFA $M=(Q,\Sigma, \delta, S, F)$ is $$\begin{align*}L(M) &= \{w\in \Sigma^{*}\mid (q_{0},w) \vdash^{*} (q,\lambda) \text{ with } q_{0} \in S, q\in F\} \\ &= \{w\in \Sigma^{*}\mid q_{0} \overset{w}{\twoheadrightarrow} q \text{ with }q_{0}\in S, q\in F\} \end{align*}$$
- For every NFA $M$ there is an NFA $N$ such that $L(M)=L(N)$ and $N$ has a **single starting state**

> [!info]- Construction NFA single starting state
> Let $N=(Q,\Sigma, \delta, S, F)$ be an NFA. Define $M$ to be obtained from $N$ as follows
> - add a fresh state $q_{0}$
> - add transitions $q_{0}\overset{\lambda}{\to} q$ for every $q \in S$, and 
> - make $q_{0}$ the only starting state of $M$
> 
> Then $M$ has a single starting state and $L(N)=L(M)$

- A language $L$ is accepted by a NFA $\iff L$ is regular

### Construction: NFA to DFA (Powerset)
Let $M=(Q,\Sigma,\delta,S,F)$ be a NFA. 
**Idea**: state of DFA = set of all states the NFA can be in
We construct a DFA $N=(Q',\Sigma,\delta',q_{0}', F')$ where
$$\begin{align*}
Q' &= 2^{Q}=\{X\mid X\subseteq Q\}\\
\delta'(X,a) &= \{q'\in Q \mid q \overset{a}{\twoheadrightarrow} q' \text{ for some } q\in X\} \\
q_{0}' &= \{q' \in Q \mid q \overset{\lambda}{\twoheadrightarrow} q' \text{ for some } q\in S\}\} \\
F' &= \{X \subseteq Q \mid X\cap F \neq \emptyset\}
\end{align*}$$
For every $w \in \Sigma^{*}$ and $X\subseteq Q$ it holds that $$X\overset{w}{\twoheadrightarrow} X' \text{ in }N \quad \iff \quad X' = \{q' \mid q \in X, q\overset{w}{\twoheadrightarrow} q' \text{ in }M\}$$

### Construction reverse language

> [!info] THEOREM
> If $L$ is regular, then its reverse $L^{R}$ is regular

Let $L$ be a regular language. Then there is an NFA $M=(Q, \Sigma,\delta,S,F)$ with $L(M)=L$. Let $N$ be the NFA obtained from $M$ by 
- reversing all arrows (transitions)
- exchanging starting states $S$ and final states $F$

Then we have $$q \overset{w}{\twoheadrightarrow} q' \text{ in }M \iff q' \overset{w^{R}}{\twoheadrightarrow} q \text{ in }N$$
Since starting and final states are swapped, it follows that $$w \in L(M) \iff w^{R} \in L(N)$$

---
References: