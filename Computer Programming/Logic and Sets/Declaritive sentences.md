---
Tags: 
Created: 2022-11-02 23:33:44
---
(Links:: [[Logic and Sets]])
> [!summary]+ propositional logic
> A language based on *propositions*, or *declarative sentences* which can argue as being true or false.

> [!example]+
> The sum of the numbers 3 and 5 equals 8.
> Jane reacted violenty to Jack's accusations.
> Every even natural number > 2 is the sum of two prime numbers.

# Propositional logic
$\lnot$: the *negation* of $p$ is denoted by $\lnot p$ and expresses 'I did **not** win the lottery last week,' or equivalently 'It is **not** true that I won the lottery last week.'

$\lor$: Given $p$ and $r$ we may wish to state that *at least one of them is true*: 'I won the lottery last week, **or** I won last week's sweepstakes;' we denote this declarative sentence by $p \lor r$ and call it the *disjunction* of $p$ and $r$.

$\land$: Dually, the formula $p \land r$ denotes the rather fortunate *conjunction* of $p$ and $r$: 'Last week I won the lottery **and** the sweepstakes.'

$\to$: Last, but definitely not least, the sentence '**If** I won the lottery last week, **then** I purchased a lottery ticket.' expresses an *implication* between $p$ and $q$, suggesting that $q$ is a logical consequence of $p$. We write $p \to q$ for that. We call $p$ the *assumption* of $p \to q$ and $q$ its *conclusion*.
## *Binding priorities*
$\lnot$ binds more tightly than $\lor$ and $\land$, and the latter two bind more tightly than $\to$. Implication $\to$ is *right-associative*: expressions of the form $p \to q \to r$ denote $p \to (q \to r)$.


---
References: