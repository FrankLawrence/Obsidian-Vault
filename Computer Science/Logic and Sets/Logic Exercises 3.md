---
Tags: 
Created: 2022-11-15 14:18:33
---
(Links:: )
3.
(a) $p\to q \equiv \lnot p \lor q \equiv (p\mid p)\lor q \equiv ((p\mid p)\mid(p\mid p))\mid (q\mid q)$
(b) $p\leftrightarrow q \equiv (p\land q)\lor (\lnot p \land \lnot q)\equiv \lnot(\lnot(p\land q)\land \lnot(\lnot p\land \lnot q))\equiv\lnot(p\land q)\mid \lnot(\lnot p\land \lnot q)\equiv (p\mid q)\mid(\lnot p\mid \lnot q)\equiv (p\mid q)\mid((p\mid p)\mid(q\mid q))$
(c) $p\land \lnot p \equiv (p\mid \lnot p)\mid (p\mid \lnot p)\equiv (p\mid (p\mid p))\mid (p\mid (p\mid p))$ (contradiction)
$p\land \lnot p \to \lnot(p\land \lnot p) \equiv p\mid \lnot p\equiv p\mid(p\mid p)$ (tautology $= \lnot$contradiction)

5.
(a)
(1) IMPL-FREE $(\lnot q \land r)\to(p\land \lnot q)$
(2) de Morgan $\equiv \lnot(\lnot q\land r)\lor(p\land\lnot q)$
(3) DISTRIBUTE $\equiv (q\lor\lnot r)\lor(p\land\lnot q)$
$\equiv(q\lor \lnot r\lor p)\land(q\lor\lnot r\lor \lnot q)$


---
References: