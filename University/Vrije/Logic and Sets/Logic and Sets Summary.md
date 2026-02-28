
> [!definition]+ 
> A **declarative sentence** (or **proposition**) is a statement that is *true* or *false*
# Syntax of Propositional Formulas
> [!important]+ Propositional logic
> A sentence in propositional logic consists of *variables p,q,v,…* and a few simple *connectives* like *and, or, not, implies* and so forth

**Connectives**
$$\begin{array}{c|c|c|c}
\lnot & \land & \lor & \to \\ \hline
\text{not} & \text{and} & \text{or} & \text{If $\dots$ then $\dots$}
\end{array}$$

> [!definition]+ Propositional Formula
> The set of *propositional formulas* is inductively defined as follows:
> 1. A propositional variable is a propositional formula. We call such a formula an *atomic formula* or simply *atom*.
> 2. If $\phi$ and $\psi$ are propositional formulas, then so are ($\lnot \phi$), ($\phi \land \psi$), ($\phi \lor \psi$), ($\phi \oplus \psi$), ($\phi \to \psi$) and ($\phi \leftrightarrow \psi$)

- **Priority Schema**: The precedence of logical operators are in the following order: 1. $\lnot$ 2. $\land , \lor$ 3. $\to , \leftrightarrow$
## Semantic equivalence
> [!definition]+
> Formulas $\phi$ and $\psi$ are *semantically equivalent*, notation $\phi \equiv \psi$, if they have identical columns in their truth tables

> [!summary]+ propositional logic
> A language based on *propositions*, or *declarative sentences* which can argue as being true or false.

> [!example]+
> The sum of the numbers 3 and 5 equals 8.
> Jane reacted violenty to Jack's accusations.
> Every even natural number > 2 is the sum of two prime numbers.

# Propositional logic
- $\lnot$: the *negation* of $p$ is denoted by $\lnot p$ and expresses 'I did **not** win the lottery last week,' or equivalently 'It is **not** true that I won the lottery last week.'

- $\lor$: Given $p$ and $r$ we may wish to state that *at least one of them is true*: 'I won the lottery last week, **or** I won last week's sweepstakes;' we denote this declarative sentence by $p \lor r$ and call it the *disjunction* of $p$ and $r$.

- $\land$: Dually, the formula $p \land r$ denotes the rather fortunate *conjunction* of $p$ and $r$: 'Last week I won the lottery **and** the sweepstakes.'

- $\to$: Last, but definitely not least, the sentence '**If** I won the lottery last week, **then** I purchased a lottery ticket.' expresses an *implication* between $p$ and $q$, suggesting that $q$ is a logical consequence of $p$. We write $p \to q$ for that. We call $p$ the *assumption* of $p \to q$ and $q$ its *conclusion*.
## *Binding priorities*
$\lnot$ binds more tightly than $\lor$ and $\land$, and the latter two bind more tightly than $\to$. Implication $\to$ is *right-associative*: expressions of the form $p \to q \to r$ denote $p \to (q \to r)$.

# Semantic Entailment
- Ignore the rows in the truth-table where the values are false (left-hand side always evaluates to false)
- When the left-hand-side is true but the right-hand-side is false, the entailment doesn't hold
- The consequence is always true, or the atiscident is false

> [!important]+ Deduction Theorem
> For all formulas $\phi_1 ,\dots ,\phi_n \; \text{and} \; \psi:$
> $$\phi_1 ,\dots ,\phi_n \vDash \psi \iff \phi_1,\dots,\phi_{n-1} \vDash \phi_n \to \psi$$

# Algebra of Sets
$$\begin{array}{c|c|c|c|c|c|c|c|c|c}
\text{Commutativity} & \text{Associativity} & \text{Complement} & \text{Identity} & \text{Domination} & \text{Involution}\\ \hline
\begin{array}{c}
A \cap B = B \cap A \\ 
A \cup B = B \cup A
\end{array}
& 
\begin{array}{c}
A \cap (B \cap C) = (A \cap B) \cap C \\
A \cup (B \cup C) = (A \cup B) \cup C
\end{array}
&
\begin{array}{c}
A \cap A' = \emptyset \\
A \cup A' = U
\end{array}
&
\begin{array}{c}
A \cap U = A \\
A \cup \emptyset = A
\end{array}
&
\begin{array}{c}
A \cap \emptyset = \emptyset \\
A \cup U = U
\end{array}
&
(A')' = A
\end{array}$$
$$\begin{array}{c|c|c|c|c}
\text{Idempotence} & \text{Definitions} & \text{De Morgan} & \text{Distributivity} \\ \hline
\begin{array}{c}
A \cap A = A \\
A \cup A = A
\end{array}
&
\begin{array}{c}
A \setminus B = A \cap B' \\
A \vartriangle B = (A \cup B) \cap (A \cap B)'
\end{array}
&
\begin{array}{c}
(A\cap B)' = A' \cup B' \\
(A\cup B)' = A' \cap B'
\end{array}
&
\begin{array}{c}
A \cap (B \cup C) = (A \cap B) \cup (A \cap C) \\
A \cup (B \cap C) = (A \cup B) \cap (A \cup C)
\end{array}
\end{array}$$

# Partial and Linear ordering relations
- A **partial order** on $V$ is a relation of type $V \times V$ that is reflexive ($xRx$) and transitive ($xRy\land yTz\to xTz$) and anti-symmetric ($(x\neq y\land xRy)\to \lnot yRx$, $xRy\land yRx\to x=y$)
  Ex: The relation $\leq$ is a partial order on $\Bbb N$
  Ex: The relation $\subseteq$ is a partial order on $\mathcal P(V)$
- The **power set** $\mathcal P(V)$
  Ex: $V:=\{1,2\}\Rightarrow \mathcal P(V)=\{\emptyset,\{1\},\{2\},\{1,2\}\}$
- $x,y\in V$ are **comparable** if $x\,R\,y$ or $y\,R\,x$
  $R$ is a **linear** (or **total**) **order** if all $x,y\in V$ are comparable
- A **strict partial order** $S$ corresponding to $R$ is defined by $$x\,S\,y \iff x\, R\, y \text{ and } x\neq y$$ -> **irreflexive**

# Hasse Diagrams
- **Hasse diagram**: omit arrows implied by reflexivity and transitivity
- **Algorithm** (HAsse diagram for a given partial order $R$ on $V$)
	- for all $x\in V \qquad G_x:=\{y:y\neq x \text{ and } x\,R\,y\}$
	- for all $x\in V \qquad H_x:=G_x\setminus \bigcup_{y\in G_x}\,G_y$
	- for all $x\in V \qquad$ draw an arrow from $x$ to every $y\in H_x$
	  $\bigcup_{y\in G_x}\,G_y \quad$ is the **union** of all sets $G_y$ with $y\in G_x$
# Cartesian Lexicographic Ordering
- **Lexicographic order**: (*A* takes priority over *B*)
  $$\langle a_1,b_1\rangle \leq \langle a_2,b_2\rangle\iff (a_1<_A a_2)\lor(a_1=a_2\land b_1 \leq_B b_2)$$
- **Cartsian order**(*A* and *B* treated equally)
  $$\langle a_1,b_1\rangle \leq \langle a_2,b_2\rangle \iff (a_1<_A a_2) \land (b_1 \leq_B b_2)$$
# Equivalence Relations
An equivalence relation in $A$ is a relation of type $A\times A$ that is reflexive, transitive and symmetric:
- the **equivalence class** of an element $a\in A$ is the set $[a]:=\{x\in A:x\equiv a\}$
	- all elements of an equivalence class relate to each other
	- elements of different equivalence classes are unrelated
- The equivalence classes together form a partition of the set $A$
  Example: $A:=\{0,1\}^3$
  $x\equiv y$ <-> x and y contain same number of 0's
  $\{[a]:a\in A\}=\{[000],[001],[011],[111]\}$ is a partition of $A$
- A **complete system or representatives** for $\equiv$ in $A$ is a set $S\subseteq A$ that contains precisely one element from each equivalence class
  Example: Congruence modulo 3 in $\Bbb N$
  $\{[n]:n\in \Bbb N\}=\{[0],[1],[2]\}$
  $S=\{0,1,2\}$
- To **prove** a relation is an equivalence relation, you must prove reflexivity, transitivity and symmetry

# Functions
- A function *$f:A\to B$* is a binary relation of type $A\times A$ such that every $x\in A$ relates to at most one $y\in B$
  *Notation:* $f(x)=y \quad\iff\quad \langle x, y \rangle \in F$
- *domain* A
- *codomain* B
- *domain of definition* $D_f := \{x\in A: f(x)=y \text{ for }y \in B\}$
- *range* $R_f := \{y\in B: y=f(x) \text{ for }x \in A\}$
- if $D_f = A$ then $f$ is *total*
- if $R_f = B$ then $f$ is *surjective*
## Injections and surjections
- $f:A \to B$ is **injective** if $$x_1 \neq x_2 \to f(x_1) \neq f(x_2)$$
  (only one way to get a value through the function f)
- $f:A \to B$ is **surjective** if $$R_f = B$$
  (all values in the codomain are reachable)
  
> [!example]
> $f:R\to\{y:y\geq 0\}$ with $f(x) = |3x-1|$
> - not injective
> - surjective
> 
> $g:\Bbb Z\to\Bbb N$ with $g(x) = |3x-1|$
> - injective
> - not surjective
## Composition of functions
The composition of $g\circ f$ of $f:A\to B$ and $g:B\to C$ satisfies $$f(x) = y \land g(y) = z \quad \Rightarrow \quad (g\circ f)(x) = g(f(x)) = z$$
**Note:** $D_{g\circ f} \subseteq D_f$ and it is possible that $D_{g\circ f} \neq D_f$
$R_{g\circ f} \subseteq R_g$ and it is possible that $R_{g\circ f} \neq R_g$
> [!example]
> $f: R\to R$ and $g:R\to R$
> $f(x) = 1-x^2 \quad D_f=R \quad R_f=\{y:y\leq 1\}$
> $g(x) = \sqrt{x} \quad D_f=\{x:x\geq 0\} \quad R_f=\{y:y\geq 0\}$
> $(g\circ f)(x)=g(f(x)) = \sqrt{1-x^2}$
> $D_{g\circ f}=\{x:-1\leq x \leq 1$ **$\neq D_f$**
> $R_{g\circ f}=\{y:0\leq y \leq 1$ **$\neq R_f$**
## Inverse functions
- The inverse of $f$ is a function if and only if $f$ in injective
  injective: *one-one*
  non-injective: *many-one*
- The inverse of a total injection $f$ is total if and only if $f$ is surjective
  $f: A \to B$ is *bijective* if
	- $f$ is total
	- $f$ is injective
	- $f$ is surjective
	  Bijections have a bijective inverse
## Cardinality
- The cardinality $|A|$ of $A$ is an abstract notion of the size of a set $A$
  $|A|\leq |B|$ <-> there exists a total injection $f:A\to B$
  $|A|=|B|$ <-> there exists a bijection $f:A\to B$
  ## Countable and uncountable sets
A set $A$ is called
- **countbly infinite** if there exists a bijection $f:N\to A$
- **countable** if $A$ is finite or countably infinite
- **uncountable** if $A$ is not countable
Example:
- $N\times N$ is countably infinite
  $\{\langle 0,0 \rangle ,\langle 0,1 \rangle ,\langle 1,1 \rangle ,\langle 1,0 \rangle ,\langle 2,0 \rangle ,\langle 2,1 \rangle ,...\}$
- The set $(0,1):=\{x:0<x<1\}$ cannot be enumerated

# Mathematical Induction
- Mathematical induction is a strategy of proving claims of the form $$S_n \text{ is true for every integer } n\geq a$$
- **Strategy:**
	- **base case:** prove $S_a$ is true
	- **inductive step:** prove $S_m \to S_{m+1}$ is true for any $m\geq a$
Example: *Claim* for all $n\in N, \; \sum_{k=0}^n 2^k=2^{n+1}-1$
*base case:*
- $\sum_{k=0}^0 2^k=2^0=1$
- $2^{0+1}-1=2^1-1=2-1=1$
  -> true for $n=0$
*induction step:* Let $m\geq 0$. Assume $\sum_{k=0}^m 2^k=2^{m+1}-1$ *IH*
$\sum_{k=0}^{m+1} 2^k=\sum_{k=0}^{m+1} 2^k+2^{m+1}=2^{m+1}-1+2^{m+1}$
$2^{(m+1)+1}-1=2\cdot 2^{m+1}-1=2^{m+1}-1+2^{m-1}$
Hence *IH* implies $\sum_{k=0}^{m+1}2^k=2^{(m+1)+1}-1$
## Well-ordering principle
- Every non-empty subset $A$ of $Z$ that has a lower bound has a smallest element (in the usual linear order on $Z$)
- In $Z$, well-ordering implies mathematial induction
- In $Z$, mathematical induction implies well-ordering
## Sequences
- A total function $f:N\to A$ defines the sequence: $$(f(n))_{n=0}^{\infty}=(f(0),f(1),f(2),...$$
  Example: factorials
  $(t_n)_{n=0}^{\infty}$ defined by $t_0:=1,t_{n+1}:=(n+1)\cdot t_n$
  Sequence $(1,1,2\cdot 1,3\cdot 2\cdot 1,...)$ so $t_n=n!$
## Transitive closure
$x \; R^n \; y \quad \Leftrightarrow$ there is a path of exactly n steps from x to y in the directed graph representation of R
- the transitive closure of a binary relation R in a set $A$ is the relation
  $$R^*:=\{\langle x,y\rangle :x\;R^n\;y\text{ for some }n\geq 1\}$$
  
# Propositional Logic
$$\begin{array}{c|c|c|c|c}
\text{Element} & \text{Subset} \\ \hline
\begin{array}{c}
a \in A : \text{$a$ is an element of the set $A$} \\
a \notin A : \text{$a$ is not an element of the set $A$} \\
\end{array}
&
\begin{array}{c}
A \subseteq B : \text{all elements of $A$ are elements of $B$}\\
A \nsubseteq B : \text{at least one element of $A$ is not in $B$}
\end{array}
\end{array}$$
$$\begin{array}{c|c|c}
\text{Equality} & \text{Empty Set} & \text{Union and Intersection} \\ \hline
\begin{array}{c}
A = B \\ \iff \\ A \subseteq B \text{ and } B \subseteq A
\end{array}
&
\emptyset \text{ or } \{\}:  \text{ unique set with no elements}
&
A \cup B := {s \in (: x \in A \text{ and } x \in B})
\end{array}$$