---
Tags: lecture
Created: 2022-11-15 15:30:14
---
(Links:: [[Data Structures and Algorithms for CS]])
# Quicksort
## idea
- split the array to be sorted into a part with *small* and a part with *larger* elements
- sort the part with *small* elements recursively
- sort the part with the *large* elements recursively
- combine the sorted parts

**Divide and conquer**
- divide: *partition* the input-array A into two parts: smaller-than-or-equal-to the pivot left of index q, larger-than the pivot right of index q
pivot at index q
- conquer: *sort recursively* the two sub-arrays
- combine: *combine* the two partial solutions into one sorted array 
  here there is nothing to do!

**Splitting**
- take an element $x$ of the array
- put the other elements $e$ in the *small part* if $e\leq x$ and the *large part* if $e>x$
  -> combine into *sorted small elements*, x, *sorted larger elements*

**Partition**
- while the list contains more than one element:
	- take an element $x$ from the list, called the *pivot*
	- index $i$ indicates the last element of the small-ones-so-far
	- index $j$ indicates the first element to be compared with the pivot ("our eyes")
	- if $j$ finds a key smaller than pivot, swap that key with the one at $i+1$
- pseudo-code
```
Algorithm partition(A,p,r):
	x := A[r]
	i := p - 1
	for j = p to r - 1 do
		if A[j] <= x then
			i : = i + 1
			exchange A[i] with A[j]
		exchange A[i+1] with A[r]
		return i + 1
```
- **partitioning is in $O(n)**
## Worst-case running time 
- running time of partition in $\Theta(n)$
- worst-case running time if no "small ones" or no "big ones"
- worst-case running time of quicksort described by: 
  $T(n)=T(0)+T(n-1)+\Theta(n)$ with $T(0)\in\Theta(1)$ and $T(1)\in\Theta(1)$
## Solving recurrence equation
## Best-case running time
- running time of partition in $\Theta(n)$
- best-case running time as many "small ones" or no "big ones"
- best-case running time of quicksort described by: 
  $T(n)=2\cdot T(\frac{n}{2})+\Theta(n)$ with $T(0)\in\Theta(1)$ and $T(1)\in\Theta(1)$
- $T(n)\in \Theta(n\cdot \log n)$

---
References: