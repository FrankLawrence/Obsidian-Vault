---
Tags: lecture
Created: 2022-11-15 12:45:10
---
(Links:: [[Data Structures and Algorithms for CS]])
# Heaps
- largest number of nodes general case: $2^{h+1}-1$
  (When all layers are full)
- least number of nodes general case: $2^h-1+1=2^h$
  (When there is one leaf in the last layer)
- **height of a heap with $n$ elements is in $\Theta(\log n)$**
## Compute parent and children
- *Algorithm* parent(i): *return* $\lfloor i/2\rfloor$
- *Algorithm* left(i): *return* $2i$
- *Algorithm* right(i): *return* $2i+1$
# Heapsort
## Idea
- *max-heap:*
	- all paths are ordered decreasingly; the max is at the top
- *first part heapsort:*
	- turn the input-array into a max-heap: *procedure [[Data Structures and Algorithms for CS Lecture 4#buildMaxHeap|buildMaxHeap]]*
- *secong part heapsort:*
	- swap the key on the root (the max) with the key on the last node
	- exclude the last node from the heap(disconnecting), so decrease the heap-size
	- reconstruct the heap: *procedure [[Data Structures and Algorithms for CS Lecture 4#MaxHeapify|MaxHeapify]]*
## Pseudo-code heapsort
```cpp
Algorithm heapsort(H):
	buildMaxHeap(H)
	for i = H.length downto 2 do
		swap H[1] and H[i]
		H.heap-size := H.heap-size - 1
		MaxHeapify(H,1)
```
## buildMaxHeap
### Pseudo-code
```cpp
Algorithm buildMaxHeap(H):
	H.heap-size := H.length
	for i = floor(H.length/2) downto 1 do
		MaxHeapify(H,i)
```
### Correctness
**loop invariant**: at the start of the for-loop each node $i+1,...,n$ is the root of a max-heap
**init**: for $i = \lfloor\frac{n}{2}\rfloor$ the nodes $i+1,...,n$ are leaves hence max-heap
**loop**: children are max-heaps by induction; use correctness of MaxHeapify
**end**: for $i = 0$ the invariant gives correctness of the output

**buildMaxHeap** is in **$O(n)$**
## MaxHeapify
- node *i* has left-child *l* and right-child *r*
- determine max of labels of *i*, *l*, *r*
- if *i* has the largest label then done
- if *l* largest label: swap labels of *i* and of *l*, do MaxHeapify(H,l)
- if *r* largest label: swap labels of *i* and of *r*, do MaxHeapify(H,r)
### Pseudo-code MaxHeapify
```cpp
Algorithm MaxHeapify(A,i):
	l := left(i)
	r := right(i)
	if l <= A.heap-size and A[l] > A[i] then
		largest := l
	else
		largest := i
	if r <= A.heap-size and A[r] > A[largest] then
		largest := r
	if largest != i then
		swap(A[i],A[largest])
		MaxHeapify(A,largest)
```
### Worst-case time complexity
- **intuition**: worst-case time complexity of down-heap bubble determinded by height of the heap so in $\Theta(\log n))$


---
References: