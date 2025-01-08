---
Tags: 
Created: 2024-11-24 17:13:16
---
(Links:: [[Concurrency and Multithreading]])
# Chapter 1
## 1.2
1. Safety: patrons are never skipped
2. -
3. -
4. -
5. -
6. Safety
7. Liveness
8. -
9. Safety
10. -
## 1.3
Let a string from Alice's house lead to a can on Bob's window-sill
- Alice waits until the can on her window-sill is down.
- She releases the pets
- When the pets return and the food is gone, she resets her can.
- She pulls her string to knock down the can on Bob's window-sill
- She keeps the pets inside until her can is down
- Bob waits until the can on his window-sill is down
- He resets his can
- He puts food in the yard
- He goes inside, and pulls his string to knock down the can on Alice's window-sill
# Chapter 16
## 16.2
$$MS_{1} (n)=2\times MS, (\frac{n}{2})+\Theta (n)$$
assume $n=2^{k}$
$$\begin{align}
MS_{1}(2^{k}) &=2 \times MS_{1} (2^{k-1})+\Theta (2^{k}) \\
&= 2^{2} \times MS_{1} (2^{k-2})+2\times \Theta (2^{k}) \\
& \;\;\vdots \\
&= 2^{k} \times MS_{1} (1)+k\times \Theta (2^{k})
\end{align}$$
$$MS_{1} (2^{k-1})=2\times MS_{1}(2^{k-2})+\Theta (2^{k-1})$$
$$\Theta (n \log(n))$$

---
References: