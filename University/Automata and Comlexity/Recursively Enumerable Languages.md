---
Tags: 
Created: 2025-03-25 22:45:45
---
(Links:: [[Automata and Complexity]])

- A language $L$ is **recursively enumerable** if $L$ is accepted by a (deterministic) [[Turing Machines|Turing machine]].
- Equivalently, a language $L$ is recursively enumerable if there exists a Turing machine that enumerates all words in $L$.
- TMs are recursively enumerable
	- From above, it follows that there is a recursive enumeration of all Turing machines: $$M_{1},M_{2},...$$

# Properties of Recursively Enumerable Languages
- If $L_{1}$ and $L_{2}$ are recursively enumerable languages, then so are $$L_{1}\cup L_{2} \qquad L_{1}\cap L_{2}$$
- We can never know if a word is not accepted by a language

---
References: