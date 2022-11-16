---
Tags: 
Created: 2022-10-31 21:56:59
---
(Links:: [[Logic and Sets]])
1.
(a) 
p: The sun shines today
q: It shines tomorrow
$p \rightarrow \lnot q$
(b)
p: Robert was jealous of Yvonne
q: Robert was not in a good mood
$p \lor q$
(c)
p: The barometer falls
q: It will rain
r: It will snow
$p \rightarrow (q\lor r)$
(d)
p: Alzheimer's disease will be cured
q: The cause of Alzheimer is determined
r: A new drug is found
$q \land r \rightarrow p$
(e)
p: No shoes are being worn
q: No shirt is being worn
r: No Service (for the customer)
$p \lor q \rightarrow r$

2.
(a) $(\lnot (p) \land q )\rightarrow r$
(b) $(p \rightarrow q) \land \lnot ((r \lor p) \rightarrow q)$
(c) $p \lor (\lnot (q) \rightarrow (p \land r))$
(d) Both $\land$  and $\lnot$ both have the same "binding strength" and as such can either be interpreted as $(p\lor q) \land r$ or $p \lor (q \land r)$

3.
(a) $\lnot(p \land q \land r) \rightarrow \lnot p \lor \lnot r$
(b) $(\lnot p \land (q \lor r) \rightarrow \lnot p) \lor \lnot r$

7.
$(\lnot (p \lor (q\land \lnot p)) \rightarrow r)$

8.
(a) False
(b) True

9.
| $p$ | $q$ | $\lnot p$ | $\lnot q$ | ($p\to \lnot q$) | ($(p\to \lnot q)\to \lnot p$) | ($((p\to \lnot q)\to \lnot p)\to q$) |
| --- | --- | --------- | --------- | ---------------- | ----------------------------- | ------------------------------------ |
| T   | T   | F         | F         | F                | T                             | T                                    |
| T   | F   | F         | T         | T                | F                             | T                                    |
| F   | T   | T         | F         | T                | T                             | T                                    |
| F   | F   | T         | T         | T                | T                             | F                                    |

-> Contingent formula

| $p$ | $q$ | $\lnot q$ | $p\to q$ | $p\to \lnot q$ | $p\to q \lor p\to \lnot q$ |
| --- | --- | --------- | -------- | -------------- | -------------------------- |
| T   | T   | F         | T        | F              | T                          |
| T   | F   | T         | F        | T              | T                          |
| F   | T   | F         | T        | T              | T                          |
| F   | F   | T         | T        | T              | T                          |

-> Tautology

---
References: