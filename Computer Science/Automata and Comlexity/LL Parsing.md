---
Tags: 
Created: 2025-03-01 13:31:23
---
(Links:: [[Automata and Complexity]])
- **Top-down parsing** tries to derive the input word from the starting variable $S$
- **Simple leftmost strategy**:
	- Always expand the leftmost variable $A$ (Replace $A$ by $u$ if there is a rule $A\to u$)
	- Backtrack when a mismatch with the input string is found
	- **Disadvantage**: backtracking is expensive and difficult

> [!note] LL Parsing
> - Parsing **top-down** with a *leftmost* strategy
> - Backtracking is *not* allowed

- LL Parsing does not work for every context-free grammar
- Starting point is a context-free grammar $G=(V,T,S,P)$:
	- *without useless variables*
	- $\lambda$-productions and unit productions are allowed (elimination often increases the size of the grammar)
- Steps of LL parsing:
	- Determine [[Context-free Grammars#Removal of Useless variables|erasable variables]]
	- Construct sets $First(A)$ and $Follow(A)$ for every variable $A$
	- Construct a parsing table
	- Parse the input word using the parsing table

## $First(A)$
- We consider the first terminal letters derivable from a word: $$First(w)=\{a\in T\;\vert\; w \Rightarrow^{*}a\dots\}\cup\{\lambda \;\vert\; w\Rightarrow^{*}\lambda\}$$
- **Algorithm**: Let $PreFirst(w)$ be the smallest set such that:
	- $w\in PreFirst(w)$
	- $a\in PreFirst(w)$ if $av \in PreFirst(w)$
	- $B\in PreFirst(w)$ if $Bv \in PreFirst(w)$
	- $v\in PreFirst(w)$ if $Bv \in PreFirst(w)$ and $B$ erasable
	- $v\in PreFirst(w)$ for every $A\in PreFirst(w)$ and rule $A\to v$
- Then $First(w)$ consists of 
	- all terminal letters $a\in T$ such that $a\in PreFirst(w)$, and 
	- $\lambda$ if $w=A_{1}A_{2}\dots A_{n}$ for erasable variables $A_{1},...,A_{n}$

> [!example]-
> $$S\to AAc \qquad A\to Ba\;\vert\; \lambda \qquad B\to Ab \;\vert\; d$$
> The erasable variables ($V\Rightarrow^{+}\lambda$) are: $A$
> We determine $PreFirst(A)$, $PreFirst(B)$ and $PreFirst(S)$:
> $$\begin{align}
> PreFirst(A) &= \{A, \underbrace{Ba}_{\text{from }A}, \underbrace{\lambda}_{\text{from }A}, \underbrace{B}_{\text{from }Ba}, \underbrace{Ab}_{\text{from }B}, \underbrace{d}_{\text{from }B}, \underbrace{b}_{\text{from }Ab} \} \\
> PreFirst(B) &= \{B, \underbrace{Ab}_{\text{from }B}, \underbrace{d}_{\text{from }B}, \underbrace{b}_{\text{from }Ab}, \underbrace{A}_{\text{from }Ab} \}\cup PreFirst(A) \\
> &= \{A,Ba,\lambda,B,Ab,d,b\} \\
> PreFirst(S) &= \{S,\underbrace{AAc}_{\text{from }S}, \underbrace{Ac}_{\text{from }AAc}, \underbrace{c}_{\text{from }Ac}, \underbrace{A}_{\text{from } AAc}\} \cup PreFirst(A) \\
> &= \{S,AAc,Ac,c,A,Ba,\lambda,B,Ab,d,b\}
> \end{align}$$
> Thus we get $$First(A)=\{b,d,\lambda\} \quad First(B)=\{b,d\}\quad First(S)=\{b,c,d\}$$

## $Follow(A)$
- We consider the terminal letters that can follow a variable: $$Follow(A)=\{a\in T\;\vert\; S \Rightarrow^{*}\dots Aa\dots\}$$
- Intuition: $a\in Follow(A)$ if $A$ can be followed by $a$ in a derivation
- **Algorithm**: 
	- $Follow(S)\supseteq \{\$\}$
	- $Follow(A)\supseteq First(w) \setminus \{\lambda\}$ for every rule $B \to vAw$
	- $Follow(A)\supseteq Follow(B)$ for rules $B \to vAw$ with $\lambda \in First(w)$

> [!example]-
> If $C\to AB$, then:
> - $First(B)\subseteq Follow(A)$
>   Example: $C\Rightarrow AB \Rightarrow^{*} Aaw$ if $B\to aw$
> - $Follow(C)\subseteq Follow(B)$
>   Example: $S\Rightarrow Ca \Rightarrow^{*} ABa$ if $S\to Ca$
> - $Follow(C)\subseteq Follow(A)$ if $B\Rightarrow^{*}\lambda$
>   Example: $S\Rightarrow Ca \Rightarrow^{*} ABa \Rightarrow Aa$ if $S\to Ca$ and $B\to \lambda$

> [!example]-
> $$\begin{align}
> S \to Dc \qquad A\to Ba \;\vert\; \lambda \\
> D \to AA \qquad B \to Ab \;\vert\; d
> \end{align}$$
> We have 
> $$\begin{align}
> First(S) &= \{b,c,d\} \qquad \,First(A) = \{\lambda, b, d\} \\
> First(D) &= \{\lambda, b ,d\} \qquad First(B) = \{b,d\}
> \end{align}$$
> >[!question]- Determine $Follow(S)$, $Follow(D)$, $Follow(A)$, $Follow(B)$
> > $$\begin{align}
> > Follow(S) &\supseteq \{\$\} \\
> > Follow(D) &\supseteq \{c\} \\
> > Follow(A) &\supseteq (First(A) \setminus \{\lambda\}) \cup \{b\} \cup Follow(D) \supseteq \{b,c,d\} \\
> > Follow(B) &\supseteq \{a\}
> > \end{align}$$
## Parsing Table
The **$LL(1)$ parsing table** for a context-free grammar is a table with
- columns indexed by terminals $T \cup \{\; \$\; \}$, and 
- rows indexed by variables $V$

The cell in row $A\in V$ and column $a \in T\cup \{\$\}$ it contains all rules $A\to u$ for which
1. $a \in First(u)$, or
2. $\lambda \in First(u)$ and $a\in Follow(A)$

> [!tip]
> Create the $First(u)$ set for all $A \to u$. These sets indicate which columns should be used (and the associated rule)

> [!example]-
> $$\begin{align}
> 1:S &\to YZ \qquad  &&2:Y  \to aZaZY \\
> 3:Y &\to \lambda \qquad &&4:Z \to cc 
> \end{align}$$
> $$First(S) = \{c,a\} \qquad First(Y)= \{\lambda, a\} \qquad First(Z) = \{c\}$$
> $$Follow(S) = \{\$\} \qquad Follow(Y) = \{c\} \qquad Follow(Z)=\{a,c,\$\}$$
> Parse table:
> $$\begin{array}{c|c|c|c}
>   & a & c & \$ \\ \hline
> S & 1 & 1 &    \\ \hline
> Y & 2 & 3 &    \\ \hline
> Z &   & 4 &    \\
> \end{array}$$

> [!example]-
> $$\begin{align}
> 1:S &\to YX \qquad  &&2:S  \to aXYXS \qquad  &&3:X \to YgZX \\
> 4:X &\to \lambda \qquad &&5:Y \to XdZf \qquad &&6:Z\to ge
> \end{align}$$
> $$\begin{array}{c|c|c}
>         & S & X & Y & Z \\ \hline
>  First  & \{a,d\} & \{\lambda, d\} & \{d\} & \{g\} \\
>  Follow & \{\$\} & \{a,d,\$\} & \{a,d,g,\$\} & \{a,d,f,\$\} \\
> \end{array}$$
> Parsing table:  
> $$\begin{array}{c|c|c|c|c|c|c}
> & 1 & 2 & 3 & 4 & 5 & 6 \\
>  & YX & aXYXS & YgZX & \lambda & XdZf & ge \\ \hline
>  First & \{d\} & \{a\} & \{d\} & \{\lambda\} & \{d\} & \{g\}
> \end{array}$$
> $$\begin{array}{c|c|c|c}
>   & a &  d  & e & f & g & \$ \\ \hline
> S & 2 & 1   &   &   &   &    \\ \hline
> X & 4 & 3,4 &   &   &   &  4 \\ \hline
> Y &   & 5   &   &   &   &    \\ \hline
> Z &   &     &   &   & 6 &    \\
> \end{array}$$

> [!example]-
> $$\begin{align}
> 1:S &\to eZX \qquad  &&2:X  \to Yg \qquad  &&3:X \to \lambda \\
> 4:Y &\to XaSe \qquad &&5:Z \to YfXfY \qquad &&6:Z\to dfdb
> \end{align}$$
> $$\begin{array}{c|c|c}
>         & S & X & Y & Z \\ \hline
>  First  & \{e\} & \{\lambda, a\} & \{a\} & \{d,a\} \\
>  Follow & \{\$, e\} & \{a,e,f,\$\} & \{a,e,f,g,\$\} & \{a,e,\$\} \\
> \end{array}$$
> Parsing table:  
> $$\begin{array}{c|c|c|c|c|c|c}
> & 1 & 2 & 3 & 4 & 5 & 6 \\
>  & eZX & Yg & \lambda & XaSe & YfXfY & dfdb \\ \hline
>  First & \{e\} & \{a\} & \{\lambda\} & \{a\} & \{a\} & \{d\}
> \end{array}$$
> $$\begin{array}{c|c|c|c}
>   & a   & b & d & e & f & g & \$ \\ \hline
> S &     &   &   & 1 &   &   &    \\ \hline
> X & 2,3 &   &   & 3 & 3 &   & 3  \\ \hline
> Y &  4  &   &   &   &   &   &    \\ \hline
> Z &  5  &   & 6 &   &   &   &    \\
> \end{array}$$

### $LL(1)$ Grammars and Parsing
- A grammar is **$LL(1)$** if its parser table contains in every cell at most one production rule
- An $LL(1)$ parse reads form *L*eft to right, performs a *L*eftmost derivation, and looks always at *1* symbol of the input
- Given an $LL(1)$-grammar and parsing table. To parse $a_{1}\cdots a_{n}$, we start with ${\color{#F05} \langle S\$,a_1 \cdots a_n \$\rangle}$. From state ${\color{#F05}\langle v,w\rangle}$ we can do the following steps:
	- ${\color{#F05}\langle av', aw' \rangle}$ becomes ${\color{#F05}\langle v', w' \rangle}$
	- ${\color{#F05}\langle Bv', aw' \rangle}$ becomes ${\color{#F05}\langle uv', aw' \rangle}$ if ${\color{#F05}B\to u}$ at position ${\color{#f05}[a,B]}$
	- ${\color{#F05}\langle Bv', \$\rangle}$ becomes ${\color{#F05} \langle v', \$\rangle}$ if ${\color{#F05} B\to u}$ at position ${\color{#F05} [\$, B]}$
	- ${\color{#F05} \langle \$, \$ \rangle}$ results in **accept**
	- In all other cases, $\langle v, w\rangle$ results in **reject**!

> [!example]-
> $$S \to aSb \;\vert\; \lambda$$
> The parser table is:
> $$\begin{array}{c|ccc}
> & a & b & \$ \\ \hline
> S & S\to aSb & S \to \lambda & S \to \lambda
> \end{array}$$
> $$\begin{align}
> \langle S\$, ab\$ \rangle &\to \langle aSb\$, ab\$ \rangle \\
> &\to \langle Sb\$, b\$ \rangle \\
> &\to \langle b\$, b\$ \rangle \\
> &\to \langle \$,\$ \rangle \quad {\color{#F05}\text{accept}} \\
> \langle S\$, abb\$ \rangle &\to \langle aSb\$, abb\$ \rangle \\
> &\to \langle Sb\$, bb\$ \rangle \\
> &\to \langle b\$, bb\$ \rangle \\
> &\to \langle \$,b\$ \rangle \quad {\color{#F05}\text{reject}} \\
> \langle S\$, aab\$ \rangle &\to \langle aSb\$, aab\$ \rangle \\
> &\to \langle Sb\$, ab\$ \rangle \\
> &\to \langle aSbb\$, ab\$ \rangle \\
> &\to \langle Sbb\$, b\$ \rangle \\
> &\to \langle bb\$, b\$ \rangle \\
> &\to \langle b\$,\$ \rangle \quad {\color{#F05}\text{reject}}
> \end{align}$$

### $LL(k)$ Grammars
- looks $k$ symbols ahead
- has $\leq 1$ rules in every cell
- $LL(k)$ is strictly contained in $LL(k+1)$
- **Disadvantage**: Size of the parse table grows exponential in $k$

> [!question]- Can ambiguous grammars be $LL(k)$ for some $k \geq 1$? 
> No

> [!question] Is the following grammar $LL(k)$ for some $k\geq 1$? $\qquad S \to aSa \;\vert\; \lambda$
### Left Factorisation
- rewrite rules ${\color{#F05}A\to uv \;\vert\; uw} \;({\color{#F05}u\neq \lambda})$ into $${\color{#F05}A\to uB} \qquad \text{and} \qquad {\color{#F05}B\to v\;\vert\; w}$$

> [!example]
> The grammar $S\to ab \;\vert\; ac$ is **not** $LL(1)$:
> $$\begin{array}{c|ccc}
> & a & b & c & \$ \\\hline
> S & \begin{array}{c}S\to ab\\ S \to ac\end{array} \\
> \end{array}$$
> After left factorisation:
> $$\begin{array}{c|ccc}
> & a & b & c & \$ \\\hline
> S & S\to aA \\
> A & & A \to b & A \to c 
> \end{array}$$

---
References: