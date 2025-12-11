---
Tags: 
Created: 2025-02-28 18:58:04
---
(Links:: [[Automata and Complexity]])

# Normal Forms
## Erasable variables
- A variable $A$ is called **erasable** if $A\Rightarrow^{*} \lambda$

### Construction
The set of erasable variables is the smallest set such that:

- If $A \rightarrow \lambda$ is a rule, then $A$ is erasable.
- If $A \rightarrow B_{1} \cdots B_{n}$ is a rule and $B_{1} , \ldots , B_{n}$ are erasable, then $A$ is erasable.

We can construct the set of erasable variables iteratively as follows:

1. Let $E = \emptyset$.
2. Repeat the following until there is nothing fresh to add:
	- If $A \rightarrow \lambda$ is a rule, then add $A$ to $E$.
	- If $A \rightarrow B_{1} \cdots B_{n}$ is a rule and $B_{1} , \ldots , B_{n}$ are in $E$, then add $A$ to $E$.
3. Finally, $E$ is the set of erasable variables.

> [!example]-
> Consider the following grammar:
> 
> $$
> \begin{align}
> S & \rightarrow A c B & A & \rightarrow C B C & B & \rightarrow a b B & C & \rightarrow c C d \\ & & & & B & \rightarrow \lambda & C & \rightarrow B B
> \end{align}
> $$
> 
> We have:
> 
> - $B$ is erasable because of the rule $B \rightarrow \lambda$,
> - $C$ is erasable because of the rule $C \rightarrow B B$ and $B$ is erasable, and
> - $A$ is erasable because of the rule $A \rightarrow C B C$ and both $B$ and $C$ are erasable.
> 
> So the variables $A , B , C$ are erasable.
## Removal of Lambda Productions
- A production rule $A \to \lambda$ is called $\lambda$-**production rule**.

> [!info] Theorem
> For every context-free language $L$ there exists a context-free grammar $G$ without $\lambda$-rules such that $L(G)=L \setminus \{\lambda\}$

### Construction
Let $G$ be a context-free grammar with $L(G)=L$.
1. Determine all erasable variables, that is variables $A$ such that $A\Rightarrow ^{*}\lambda$
2. For every rule $A\to xBy$ with $B$ erasable, add a rule $A\to xy$
3. Remove all $\lambda$-production rules

The resulting grammar $G$ has the property $L(G)=L \setminus \{\lambda\}$

> [!example]
> Consider the following grammar
> $$S\to ABaC \qquad A\to BC \qquad B \to b \mid \lambda \qquad C \to D \mid \lambda \qquad D \to d$$
> > [!question]- Which variables can produce $\lambda$?
> > - $B\Rightarrow ^{*}\lambda$ since $B\to \lambda$
> > - $C\Rightarrow ^{*}\lambda$ since $C\to \lambda$
> > - $A\Rightarrow ^{*}\lambda$ since $A\to BC$ and $B\Rightarrow ^{*}\lambda$ and $C\Rightarrow ^{*}\lambda$
> 
> The result of removing all $\lambda$-rules is:
> $$\begin{align}
> S &\to ABaC \mid BaC \mid AaC \mid ABa \mid aC \mid Ba \mid Aa \mid a \\
> A &\to BC \mid C \mid B \\
> B &\to b \\
> C &\to D \\
> D &\to d
> \end{align}$$
## Removal of Unit Productions
- A rule $A\to B$ where $A, B\in V$ is called **unit production rule**

> [!info] Theorem
> For every context-free language $L$ there is a context-free grammar $G$ without $\lambda$- and unit-productions with $L(G)=L \setminus \{\lambda\}$

### Construction
Let $G$ be context-free, without $\lambda$-rules, and $L(G)=L\setminus \{\lambda\}$
1. Determine all pairs $A\neq B$ with $A \Rightarrow^{*}B$
2. Whenever $A\Rightarrow^{+}B$ and for every rule $B\to y$, add a rule $A\to y$
3. Remove all unit production rules

The resulting grammar $G$ has no $\lambda$- and unit-productions and it has the property $L(G)=L\setminus \{\lambda\}$

> [!example]
> Remove all unit production rules from 
> $$S\to Aa \mid B \qquad A \to a \mid bc \mid B \qquad B \to A \mid bb$$
> Note that there are no $\lambda$-productions. So no need to first remove $\lambda$-productions.
> 1. We determine all pairs $A\neq B$ with $A\Rightarrow^{+}B$:
> 	1. $S\Rightarrow^{+}B$
> 	2. $A\Rightarrow^{+}B$
> 	3. $B\Rightarrow^{+}A$
> 	4. $S\Rightarrow^{+}A$
> 2. The detailed reasoning for $S$ is as follows:
> 	1. We have $S\Rightarrow^{+}A$ and $A\to a$, so we add $S\to a$
> 	2. We have $S\Rightarrow^{+}A$ and $A\to bc$, so we add $S\to bc$
> 	3. We have $S\Rightarrow^{+}A$ and $A\to B$, we already have $S\to B$
> 	4. We have $S\Rightarrow^{+}B$ and $B\to A$, we already have $S\to A$
> 	5. We have $S\Rightarrow^{+}B$ and $B\to bb$, we already have $S\to bb$
> 
> Applying this reasoning also to $A$ and $B$, we get the following rules:
> $$\begin{align}
> S &\to Aa \mid B \mid a \mid bc \mid A \mid bb \\
> A &\to a \mid bc \mid B \mid A \mid bb \\
> B &\to A \mid bb \mid a \mid bc \mid B
> \end{align}$$
> 1. Removing all unit production rules yields the final result:
> 2. $$S \to a \mid bb \mid bc \mid Aa \qquad A \to a \mid bb \mid bc \qquad B \to a \mid bb \mid bc$$
## Removal of Useless variables
A variable is *useless* if it cannot be part of a derivation from the starting symbol to a terminal word.

- A variable $A$ is **useless** for a context-free grammar if there exists **no** derivation of the form $$S\Rightarrow^{*}uAv \Rightarrow^{+}w\qquad \text{with }w\in T^{*}$$
- Removing production rules that contain a useless variable from a grammar does not change the generated language.

> [!example]-
> Consider the following grammar: $$S \to aSb \mid BC \mid \lambda \qquad A \to Sb \qquad B \to a \qquad C\to C$$
> Which variables are useless?
> - $A$ because there is no derivation $S\Rightarrow^{*}uAv$
> - $C$ because there is no derivation $C\Rightarrow^{*}w$ with $w\in T^{*}$
> - $B$ because $B$ can be reached only together with $C$
> 
> The resulting grammar is $$S\to aSb \mid \lambda$$

- A variable $A$ is called **productive** if $A \Rightarrow^{+}w$ with $w\in T^{*}$
- non-productive variables are useless
### Construction: Productive Variables
The set of productive variables is the smallest set such that:
- If $A\to y$ is a rule and all variables in $y$ are productive, then $A$ is productive

So the set of productive variables can be computed iteratively as follows:
1. Let $P=\emptyset$
2. If there is a rule $A\to y$ such that all variables in $y$ are in $P$, then we add $A$ to $P$
3. Repeat step 2 until there is nothing new to add

> [!example]-
> $$S \to aSb \mid BC \mid \lambda \qquad A \to Sb \qquad B \to a \qquad C\to C$$
> We determine the productive variables:
> - $P=\emptyset$
> - we add $S$ to $P$ because of the rule $S\to \lambda$; now $P=\{\;S\;\}$
> - we add $B$ to $P$ because of the rule $B\to a$; now $P=\{\;S,B\;\}$
> - we add $A$ to $P$ because of the rule $A\to Sb$ and $S\in P$; now $P=\{\;S,A,B\;\}$
> 
> There is nothing more to add, so the set of productive variables is $\{\;S,A,B\;\}$
### Construction: Useless Variables
First, we remove all rules that contain a **non**-productive variable. Second, we determine variables that are **reachable** using the *remaining* rules:
- $S$ is reachable
- If $A\to y$ and $A$ is reachable, then so are all variables in $y$

Third, we remove all rules that contain a **non**-reachable variable. A variable of the original grammar is *useless* if it does not occur in one of the remaining rules

> [!example]-
> $$S \to aSb \mid BC \mid \lambda \qquad A \to Sb \qquad B \to a \qquad C\to C$$
> The set of productive variables is $\{\;S,A,B\;\}$. So $C$ is the only non-productive variable.
> We remove all rules containing non-productive variables: $$S\to aSb\mid\lambda \qquad A\to Sb \qquad B\to a$$
> Which variables are reachable from $S$? 
> - only $S$ is reachable
> 
> We remove all rules containing non-reachable variables: $$S\to aSb\mid \lambda$$
> Hence only $S$ is useful, the variables $A,B$ and $C$ are not useful.

---
References: