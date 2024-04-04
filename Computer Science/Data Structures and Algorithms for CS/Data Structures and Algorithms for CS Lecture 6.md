---
Tags: lecture
Created: 2022-12-01 10:51:46
---
(Links:: [[Data Structures and Algorithms for CS]])
# Priority Queue
- queue where **most important** element is served first
- A priority queue is a data structure for maintaining a set $S$ of elements, each with an associated value called a **key**
- *insert* in $O(\log n)$
- *remove* in $O(\log n)$
- *maximum* in $O(1)$
# Lower bound on sorting
- we consider **decision trees**
	- A decision tree is a full binary tree that represents the comparisons between elements that are performed by a particular sorting algorithm operating on an input of a given size
- each **node** contains a comparison $a < b$
- a **leaf** corresponds to a permutation of $\{1,\dots n\}$
- every comparison-based sorting algorithm has for every number of inputs a decision tree
	- The execution of the sorting algorithm corresponds to tracing a simple path from the root down to a leaf
	- every possible permutation (of total $n!$) must occur
	- every permutation must be reachable
- Any correct sorting algorithm must be able to produce each permutation of its input -> each of the $n!$ permutations on $n$ elements must appear as one of the leaves of the decision tree
Ex: 
![[Data Structures and Algorithms for CS Lecture 6 2022-12-01 11.06.42.excalidraw]]
## Analysis of height of decision tree
- the length of the longest simple path from root to any reachable leaf represents the worst-case number of comparisons -> height of tree is worst-case number comparisons
- $l$: reachable leaves
- $h$: height of tree
- $n$: input size
- because each of the $n!$ permutations of the input appears as some leaf -> $n! \leq l$
- a binary tree of height $h$ has no more than $2^h$ leaves
$$\Rightarrow n!\leq l\leq 2^h$$
$$\begin{align}\to h&\leq \lg(n!)\\&=\Omega(n\lg n)\end{align}$$
-> **Any comparison based sorting algorithm uses $\Omega(n\log n)$ comparisons in the worst case**
-> Heapsort and mergesort are asymptotically optimal
# Counting sort
*assumption*: each of the $n$ input elements is an integer in the range $\{0,\dots,k\}$ for some integer $k$
**algorithm idea**: count the number of occurences of each $i$ from $\{0,\dots,k\}$
**time complexity**: in $\Theta(n+k)$ for an input-array of length $n$
**drawback**: fixed range, and requires additional counting array $C$ and output array $B$
**counting sort is a [[Sorting#Stable Sorting|stable sorting]] algorithm**

---
References: