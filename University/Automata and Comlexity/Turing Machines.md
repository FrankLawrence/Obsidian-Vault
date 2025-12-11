---
Tags: 
Created: 2025-03-19 12:47:01
---
(Links:: [[Automata and Complexity]])
Turing machines *read* and *write* an input word on an infinite tape (unlimited memory)
- at each step head reads symbol from tape, overwrites it and moves one place left or right
- there is a finite set of states $Q$ and alphabet $\Gamma$
- $\square$ denotes a **blank symbol**
- transition function $\delta$ has the form $$\delta: Q\times \Gamma\to Q\times \Gamma \times \{L,R\}$$
	- $\delta(q,a)=(q', b,X)$: when machine is in state $q$ and reads $a$ then overwrite $a$ with $b$, move 1 space left or right (if $X=L$, or $X=R$) and switch to state $q'$
- A deterministic Turing machine (TM) is a 7-tuple: $$(Q,\Sigma,\Gamma,\delta,q_{0},\square, F)$$
	- $Q$: finite set of states
	- $\Sigma\subseteq \Gamma\setminus \{\square\}$: finite input alphabet
	- $\Gamma$: finite tape alphabet
	- $\delta:Q \times \Gamma \to Q \times \Gamma \times \{L,R\}$: partial transition funciton
	- $q_{0}$: starting state
	- $\square \in \Gamma$: blank symbol
	- $F\subseteq Q$: set of final states
- **Assumption**: $\delta(q,a)$ is undefined for every $q\in F$ and $a\in \Gamma$
- A configuration $(q,c)$ of a TM consists of a state $q\in Q$ and a function $c:\mathbb{Z}\to \Gamma$, the **tape content**
	- The non-blank positions $\{z\in \mathbb{Z} \,\vert\, c(z)\neq \square\}$ are finite
	- The head of the machine stands on $c(0)$
- Let $n,m\in \mathbb{N}$ (exists for every configuration) such that $$\forall i< -n.c(i)=\square \quad \text{and}\quad \forall i>m.c(i)=\square$$ Then we denote the **configuration by the finite word** $$c(-n)c(-n+1) \cdots c(-1) \; {\color{#F08}q} \; c(0)c(1)\cdots c(m)$$
	- We write $w\approx v$ if $w$ and $v$ denote the same configuration

The computation steps $\vdash$ on configurations are defined by: 
$$\begin{align}
v{\color{#F08}q}{\color{orange}a}w &\vdash v{\color{orange}b}{\color{#F08}q'}w &&\text{if } \delta(q,a)=(q',b,R) \\
v{\color{#58F}c}{\color{#F08}q}{\color{orange}a}w &\vdash v{\color{#F08}q'}{\color{#58F}c}{\color{orange}b}w &&\text{if } \delta(q,a)=(q',b,L) 
\end{align}$$ 
where $v,w\in \Gamma^{*}$, $a,c\in \Gamma$ and $q\in Q$

> [!example]
> Assume that
> $$\begin{align}
> \delta(q_{0},a) &= (q_{0},a,R) \\
> \delta(q_{1},a) &= (q_{1},b,L) \\
> \delta(q_{0},\square) &= (q_{1},c,L)
> \end{align}$$
> Then we have:
> $$\begin{align}
> q_{0}aa \vdash aq_{0}a\vdash aaq_{0}\vdash aq_{1}ac \vdash q_{1}abc \vdash q_{1}\square bbc
> \end{align}$$

- a configuration $vqaw$ is a **halting state** if $\delta(q,a)$ is undefined
- The transition graph for a TMs contains $$\text{an arrow } \quad q\overset{a/b \; X}{\longrightarrow} q' \quad \text{whenever}\quad \delta(q,a)=(q',b,X)$$

> [!example]
> The turing machine $M=(Q,\Sigma,\Gamma,\delta,q_{0},\square,F)$ with $\Sigma=\{a,b\}$, $\Gamma=\{a,b,\square\}$, $Q=\{q_{0},q_{1},q_{2}\}$, $F=\{q_{2}\}$ and 
> $$\begin{align}
> \delta(q_{0},a) &= (q_{1},b,R) \\
> \delta(q_{0},b) &= (q_{0},a,R) \\
> \delta(q_{1},a) &= (q_{0},b,R) \\
> \delta(q_{1},b) &= (q_{1},a,R) \\
> \delta(q_{1},\square) &= (q_{2},\square, L)
> \end{align}$$
> can be visualised as ![[Pasted image 20250319135132.png|300]]

- The language $L(M)$ accepted by TM $M=(Q,\Sigma,\Gamma,\delta,q_{0},\square, F)$ is $$\{w\in \Sigma^{*} \,\vert\, q_{0}w \vdash^{*}uqv \text{ for some } q\in F,\; u, v\in \Gamma^{*}\}$$
- If $w\notin L(M)$ this can have two causes
	- the execution halts in a configuration $vqw$ with $q\notin F$, or
	- the execution is infinite (never halts)
- A language is **recursively enumerable** if it is accepted by a TM

> [!example]-
> We construct a TM $M$ with $L(M)=\{a^{n}b^{n}c^{n}\,\vert\, n\geq 1\}$ 
> **Idea**: stepwise replace one $a$ by 0, one $b$ by 1 and one $c$ by 2.
> - $\Sigma=\{a,b,c\}$ and $\Gamma\{a,b,c,0,1,2,\square\}$
> - $q_{0}$: Read $a$, replace by 0, move right and switch to $q_{1}$
> - $q_{1}$: Keep moving right until we read $b$. Replace $b$ by 1, move right and switch to $q_{2}$
> - $q_{2}$: Keep moving right until we read $c$. Replace $c$ by 2, move left and switch to $q_{3}$
> - $q_{3}$: Keep moving left until we read 0. Move right and switch back to $q_{0}$
> - If we read 1 in $q_{0}$, switch to $q_{4}$
> - $q_{4}$: Keep moving right to check whether there are $a$'s, $b$'s or $c$'s left. If not, then go to final state $q_{5}$
> $$\begin{align}
> &{\color{#F08}q_{0}}aabbcc \\
> \vdash \;&0{\color{#F08}q_{1}}abbcc \\
> \vdash \;&0a{\color{#F08}q_{1}}bbcc \\
> \vdash \;&0a1{\color{#F08}q_{2}}bcc \\
> \vdash \;&0a1b{\color{#F08}q_{2}}cc \\
> \vdash \;&0a1{\color{#F08}q_{3}}b2c \\
> \vdash \;&0a{\color{#F08}q_{3}}1b2c \\
> \vdash \;&0{\color{#F08}q_{3}}a1b2c \\
> \vdash \;&{\color{#F08}q_{3}}0a1b2c \\
> \vdash \;&0{\color{#F08}q_{0}}a1b2c \\
> \vdash^{*} \;&00{\color{#F08}q_{0}}1122 \\
> \vdash \;&001{\color{#F08}q_{4}}122 \\
> \vdash^{*} \;&001122{\color{#F08}q_{4}} \\
> \vdash \;&00112{\color{#F08}q_{5}}2 \\
> \end{align}$$

> [!question]- Construct a Turing Machine accepting all words of *odd* length over the alphabet $\Sigma=\{a,b\}$
> ```mermaid
> flowchart LR
> 	A@{ shape: sm-circ, label: "Small start" }
> 	B@{ shape: circ, label: "q<sub>0</sub>" }
> 	C@{ shape: circ, label: "q<sub>1</sub>" }
> 	D@{ shape: dbl-circ, label: "q<sub>2</sub>" }
> 	A --> B
> 	B -->|a/a R| C
> 	B -->|b/b R| C
> 	C -->|a/a R| B
> 	C -->|b/b R| B
> 	C -->|blank/blank R| D
> ```

# Extensions of Turing Machines
- **Multiple tapes** can be simulated using a single tape with polynomial overhead in time complexity
- **Nondeterministic Turing machines** have as transition function $$\delta: Q \times \Gamma \to 2^{Q \times \Gamma \times \{L,R\}}$$
- A nondeterministic TM can be simulated by deterministic TM using **breadth-first search** (all computations in parallel).
## Church-Turing Thesis
> **Church-Turing thesis**: Every computation of a computer can be simulated by a deterministic Turing machine.

- Alonzo Church is inventor of [[lambda calculus]]
- Both proved [[Undecidability and Incompleteness of Predicate Logic|undecidability of validity in predicate logic]]
## Not all languages are recursively enumerable
- A set $A$ is countable if there is a [[Functions#Injections and surjections|surjective]] function $f:\mathbb{N}\to A$
- There are countably many TMs over an input alphabet $\Sigma$
- There are uncountable many languages over $\Sigma$
- ==TODO==
## Universal Turing Machine
- A computer can execute any program on any input
- A TM is called **universal** if it can simulate every TM
- A universal TM gets as input A turing machine $M$ and input word $u$ and then executes $M$ on $u$

> [!info] THEOREM
> There exists a universal Turing machine

---
References: