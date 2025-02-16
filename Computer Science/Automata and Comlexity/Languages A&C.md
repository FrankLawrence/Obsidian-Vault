---
Tags: Math
Created: 2025-02-06 23:20:52
---
(Links:: [[Automata and Complexity]])
**Words** are a finite sequence of **symbols** from an **alphabet** $\Sigma$, where it's elements consist of $a,b,c,...$. The letters $u,v,w,x,y,z$ are special characters which denote **words** (in programming known as strings), where $\lambda$ is the **empty word** (*not a letter of the alphabet*).

Programs consist of the bits 0 and 1 ($\Sigma=\{0,1\}$), and as such can be thought of as a *word*. Programs take in a word and give as output a word (e.g. `cat cat`).

> [!info] Operations on Words
> 1. **Concatenation**: If $v=a_{1}\cdots a_{n}$ and $w=b_{1}\cdots b_{m}$, then $$vw = a_{1}\cdots a_{n}b_{1}\cdots b_{m}$$ 
> 2. **Length**: If $v=a_{1}\cdots a_{n}$, then $|v|=n$. The length can be defined inductively: $$|\lambda| = 0 \qquad \qquad |va| = |v| + 1$$
> 3. **Power**: The power $v^{k}$ consists of $k$ concatenations of $v$'s: $$|v^{0}| = k \qquad \qquad v^{k+1}=v^{k}v$$
> 4. **Reverse**: The reverse of $a_{1}\cdots a_{n}$ is $$(a_{1}\cdots a_{n})^{R}=a_{n}\cdots a_{1}$$ The reverse can be inductively defined $$\lambda^{R}=\lambda \qquad \qquad (va)^{R}=a(v^{R})$$
# Languages
> [!definition] Formal language
> A formal language $L$ is a **set of words** and thus a subset of $\Sigma^{*}$, where $\Sigma^{*}$ is the set of all words over $\Sigma$. 

> [!example]-
> 1. The set of all parseable C programs form a language.
> 2. $\{ ab, aab, bbaaabb \}$ is a finite language over $\Sigma = {a, b}$
> 3. $\{ab^{n}a \;\vert\; n \geq 1\}$ is an infinite language over $\Sigma = \{a,b\}$: $$\{aba, abba, abbba, abbbba, ...\}$$
> 4. $\{a^{n}b^{n} \;\vert\; n \geq 0\}$ is an infinite language over Σ = {a,b}: $$\{\lambda, ab, aabb, aaabbb, aaaabbbb, ...\}$$

> [!info] Operations on Languages
> 1. **Set operations**: A language is a **set** of words. So the usual set operations have meaning for languages: $$\in, \subseteq, \cap, \cup, \setminus, ... $$
> 2. **Complement**: The complement $\overline{L}$ = all words that are not in the language $L$ $$\overline{L} = \Sigma^{*} \setminus L$$
> 3. **Reverse**: The reverse of a language $L$ is $$L^{R} = \{x^{R} \;|\; x \in L \}$$
> 4. **Concatenation**: The concatenation of languages $L_{1}$ and $L_{2}$ is defined as $$L_{1}L_{2}=\{xy \;|\; x \in L_{1} \land y \in L_{2}\}$$
> 5. **Power**: The $n$-th power of a language $L$ is defined by induction on $n$: $$L^{0}=\{ \lambda\}\qquad\qquad L^{n+1}=L^{n}L\qquad (n\geq 0)$$
>    > [!example]- $L=\{a,bb\}$
>    > $$L^{3}=\{aaa,aabb,abba,abbbb,bbaa,bbabb,bbba,bbbbb\}$$
>
>    > [!attention] $L^{2}=\{uv \;|\; u,v\in L\}\neq\{uu \;|\; u \in L\}$
> 1. **Kleene star**: $$\begin{align}L^{*} &= \bigcup_{i=0}^{\infty}L^{i}=L^{0}\cup L^{1}\cup L^{2}\cup L^{3}\cup \cdots \\ L^{+} &= \bigcup_{i=1}^{\infty}L^{i}= L^{1}\cup L^{2}\cup L^{3}\cup \cdots\end{align}$$
>    Thus $L^{*}=L^{+}\cup \{\lambda\}$. $L^{*}$ is simply a way of defining all words that can be built from the symbols in $L$.
>    > [!example]- $L=\{a,bb\}$
>    > $$L^{*}=\{\lambda, a,bb, aa, abb, bba, bbbb, aaa, aabb, abba, abbbb, ...\}$$

> [!example]- Example Set operations
> 2. $$ba \in \{a, aba, ba\} \qquad \qquad ab \notin \{a,aba,ba\}$$
> 3. $$\{a,ba\} \subseteq \{a, aba, ba\} \qquad \qquad \{a,b\}\nsubseteq\{a,aba,ba\}$$
> 4. $$\{a,aba,ba\}\cap \{a,ab,ba\}=\{a,ba\}$$
> 5. $$\{a,aba,ba\}\cup\{a,ab,ba\}=\{a,ab,aba,ba\}$$
> 6. $$\{a,aba,ba\}\setminus \{a,ab,ba\}=\{aba\}$$

Let 
- $\Sigma = \{a, b\}$
- $L= \{ab^{n}\; | \; n \geq 0\}$

> [!question]- Describe the following languages as sets: $L^{R}$ $\overline{L}$
> 1. $$L^{R}=\{b^{n}a \;|\; n\geq 0\}$$
> 2. $$\overline{L}=\{\lambda\}\cup \{bw \;|\; w\in \Sigma^{*}\} \cup \{awau \;|\; w,u \in \Sigma^{*}\}$$

As we can see, some sets become very hard to formulate. However there are also [[Finite Automata|alternative ways]] of describing languages.

---
References: