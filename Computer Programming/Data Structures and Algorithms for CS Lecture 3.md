---
Tags: lecture
Created: 2022-11-10 15:39:03
---
(Links:: [[Data Structures and Algorithms for CS]])
# Max-Heap intuition
- structure of an almost-complete binary tree
- every level is complete except for the last level, which is filled from left to right
- the numbers below a level are always smaller
# Heap-sort
- swap the root with the last element -> the last element is sorted, since it is the largest
  lock the last element (detach) so it can't be changed
- the root isn't the largest number anymore -> swap with the left or the right node if it is bigger
  do this until it is small enough (bubble down)

H[1...n] an array of integers
directly after building the heap: H.heap-size = H.length
```
Algirthm heapsort(H):
	buildMaxHeap(H)
	for i = H.length downto 2 do
		swap H[1] and H[i]
		H.heap-size := H.heap-size-1
		MaxHeapify(H,1)
```

---
References: