---
Tags: lecture
Created: 2022-12-01 18:00:20
---
(Links:: [[Data Structures and Algorithms for CS]])
# Direct address-tables
- **Universe of keys**: $U=\{0,...,m-1\}$ with small m
- array **T[0..m-1]** where each slot corresponds to a key in the universe $U$
```cpp
Direct-Address-Search(T,k)
	return T[k]
```
```cpp
Direct-Address-Insert(T,x)
	T[x.key] = x
```
```cpp
Direct-Address-Delete(T,x)
	T[x.key] = NIL
```
- Each operation in $O(1)$ **worst-case**
- x is **pointing to element** with key $x.key$
- **Con**:
	- keys are natural numbers
	- a lot of space for large universe
	- a lot of space for small subset of $U$
	  -> wasted space
# Hash tables
- **hash function h** computes slot from the key k
- release storage requirment to size of actually used keys
- problem: different keys may be hashed to the same slots (**collision**)
- An element with key k *hashes* to slot $h(k)$
- $h(k)$ is the *hash value* of key k
$$h:U\to\{0,1,...,m-1\}$$
- In a hash table in which collisions are resolved by chaining, an unnsuccessful search takes average-case time $\Theta(1+\alpha)$, under the assumption of simple uniform hashing
## Collision resolution by chaining
- put items that hash to the same value in a [[Data Structures and Algorithms for CS Lecture 8#Linkedlist| linked list]]
- create a list for each slot
```cpp
Chained-Hash-Insert(T,x)
	insert x at the head of list T[h(x.key)]
```
```cpp
Chained-Hash-Search(T,k)
	search for an element with key k in list T[h(k)]
```
```cpp
Chained-Hash-Delete(T,x)
	delete x from the list T[h(x.key)]
```
- worst-case insertion in $O(1)$
- worst-case search proportional to size of list
- worst-case delete in $O(1)$ if doubly-linked list
- **local factor $\alpha$**: average number of elements stored in a chain -> $\alpha =\frac nm$
- **simple uniform hashing**: any given element is equally likely to hash into any of the m slots, independently of where any other element has hashed to 
## Hash functions
- good: distributes keys uniformly and seemingly randomly
- **division method**:
	- key is hashed to *$k \mod m$*
	- easy to compute
	- not good for all values of m
	- take for m a prime not too close to a power of 2
- **multiplication method**: $h(k)=\lfloor m (kA \mod 1)\rfloor$ where $0<A<1$
## Open addressing
- alternative to chaining for solving collisions
- all elements occupy the hash table itself
  -> no pointers -> larger number of slots
- sequence of slots are computed by **probing**
- insertion: try slots of the probe sequence and take the first available one -> deletion difficult

---
References: