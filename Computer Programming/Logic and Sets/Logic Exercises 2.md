---
Tags: 
Created: 2022-11-10 23:56:25
---
(Links:: [[Logic and Sets]])
d)

| $p$ | $q$ | $q\to p$ | $p \lor (q\to p)$ | $p \lor \lnot q$ |
| --- | --- | -------- | ----------------- | ---------------- |
| 1   | 1   | 1        | 1                 | 1                |
| 1   | 0   | 1        | 1                 | 1                |
| 0   | 1   | 0        | 0                 | 0                |
| 0   | 0   | 1        | 1                 | 1                |
-> Semantic entailment holds

2.
a) $\phi$ is substituted with a tautology
b) If $p$ is false the consequence will be true in some cases and false in other cases.
$\vDash$ holds if in every case:
- the consequence is true
- the antecedent is false
c) $\phi$ is substituted by a contradiction

3.
a)
$\phi \vDash \phi$ - $\phi$ is true on left and right -> reflexive
$\phi \vDash \psi$ if $\psi$ is true and $\phi$ is false -> not symmetry
$\phi \vDash \psi , \psi \vDash \chi$ when $\phi$ is true then $\psi$ is true by(1) and also $\chi$ is true by(2) -> transitive
b)
$\phi \equiv \psi$ iff $\phi \equiv \psi$ and $\psi \vDash \phi$ -> reflexive
$\phi \equiv \psi \Longleftrightarrow \psi \equiv \phi$ -> symmetry
- transitive

4.
b) $(p+q)\cdot r= p \cdot r + q \cdot r$
$(q\cdot p) + r \neq (q+r)\cdot (p+r)$

5.
a) $\psi_1 = p$ $\phi$ - tautology
$\psi_2 = \lnot p$ $(\psi_1 \lor \psi_2)$ - tautology
$p \lor \lnot p$ - tautology
neither $p$ not $\not p$ is a tautology
b) When $\phi$ is $T$ then so until $\psi_1 \land \psi_2$. If $\psi_1 \land \psi_2$ is $T$ then so must be $\psi_1$ and $\psi_2$

7.
A - A is a truth-speaker
B - B is a truth-speaker
h - The harbor is straight ahead

$B \longleftrightarrow \lnot A \lor \lnot B$
$A \longleftrightarrow h$
$B \longleftrightarrow h$

| $A$ | $B$ | $h$ | $A \longleftrightarrow h$ | $B \longleftrightarrow h$ | $B \longleftrightarrow$ | $\lnot A \lor \lnot B$ |
| --- | --- | --- | ------------------------- | ------------------------- | ---------------------- | --------------------- |
| 0   | 0   | 0   | 1                         | 0                         | 0                      | 1                     |
| 0   | 0   | 1   | 0                         | 1                         | 0                      | 1                     |
| 0   | 1   | 0   | **1**                     | **1**                     | **1**                  | **1**                 |
| 0   | 1   | 1   | 0                         | 0                         | 1                      | 1                     |
| 1   | 0   | 0   | 0                         | 0                         | 0                      | 1                     |
| 1   | 0   | 1   | 1                         | 1                         | 0                      | 1                     |
| 1   | 1   | 0   | 0                         | 1                         | 0                      | 0                     |
| 1   | 1   | 1   | 1                         | 0                         | 0                      | 0                     |

---
References: