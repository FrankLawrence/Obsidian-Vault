---
Tags: lecture
Created: 2022-12-01 12:39:35
---
(Links:: [[Data Structures and Algorithms for CS]])
# Radix sort
```ad-definition
collapse: none
Radix sort avoids comparison by creating and distributing elements into buckets according to their radix.
For elements with more than one significant digit, this bucketing process is repeated for each digit, while preserving the ordering of the prior step, until all digits have been considered.
```
- sorting numbers considered as tuples
- number of columns: (fixed) amount of digits used
- number of places: 10 for decimal numbers

**Intuition**: sort per dimension using a stable sorting algorithm
- sorting on the *least significatn digit* first
- using a stable underlying sorting algorithm
```
Radix-sort(A,d)
	for i = 1 to d
		use a stable sort to sort array A on digit i
```
# Bucketsort
- assumes that the input is drawn from a uniform distribution
- average-case running time: $O(n)$
- divides the interva \[0,1) into $n$ equal-sized subintervals and then distributes the $n$ input numbers into the buckets
```
Bucket-sort(A)
	let B[0..n-1] be a new array
	n = A.length
	for i = 0 to n - 1
		make B[i] an empty list
	for i = 1 to n
		insert A[i] into list B [[nA[i]]]
	for i = 0 to n - 1
		sort list B[i] with insertion sort
	concatenate the lists B[0],B[1],...,B[n - 1] together in order
```
# Stacks and queues
**Stack**: last-in, first-out (*LIFO*)
- If `S.top` = 0, the stack is empty
- If we attempt to pop an empty stack, the stack *underflows*
- If `S.top` exceeds $n$, the stack *overflows*

**Queue**: first-in, first-out (*FIFO*)
- the queue has a head and a tail
- an element is enqueued by taking its place at the tail (`Q.tail`)
- an element is dequeued by removing the element at the head (`Q.head`)
- If `Q.head` = `Q.tail`, the list is empty
- If `Q.head`= `Q.tail`+1, the queue is full
# Linkedlist
- objects are arranged in a linear order
- order is detemined by a pointer in each object (not indices)
- **doubly linked list**: each object has an attribute *key* and two pointer attributes *next* and *prev*
- if `x.next = NIL` -> element `x`has no successor -> *tail*
- if `L.head = NIL` -> list is empty
- **circular list**: tail.next points to head, and head.prev points to tail
- **Insert an element in a doubly linked list**: let the next point to the head of a list -> $O(1)$
- **search for a key in a doubly linked list**: walk through the list until we find the key or exit the list -> $\Theta(n)$ worst-case
- **delete an element from a doubly linked list**: rearrange the pointers of the elements before and after -> $O(1)$ for element x
  If we are given the key and first have to search for the element x with the key: $\Theta(n)$
## Sentinels
```ad-definition
collapse: none
A dummy object that allows us to simplify boundary conditions
```
- represents `NIL` but has all attributes of other objects in the list
- regular doubly linked list turns into a **circular, doubly linked list with a sentinel** (sentinel lies between head and tail)

---
References: