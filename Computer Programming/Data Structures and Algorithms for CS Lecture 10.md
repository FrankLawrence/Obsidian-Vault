---
Tags: lecture
Created: 2022-12-08 13:05:45
---
(Links:: [[Data Structures and Algorithms for CS]])
# Tree traversal
- preorder: visit first node and next its successors
```cpp
Algorithm preOrder(v):
	if v != NIL then 
		visit(v)
		if v.left != NIL then
			preOrder(v.left)
		if v.right != NIL then
			preOrder(v.right)
```
- postorder: visit all successors and next the node itself
```cpp
Algorithm postOrder(v):
	if v != NIL then
		if v.left != NIL then 
			postOrder(v.left)
		if v.right != NIL then
			postOrder(v.right)
		visit(v)
```
- inorder: visit left tnen node, then right
```cpp
Algorithm inOrder(v):
	if v != NIL then
		inOrder(v.left)
		visit(v)
		inOrder(v.right)
```
# Binary search
```cpp
ITERATIVE-TREE-SEARCH(x,k)
	while x != NIL and k != x.key
		if k < x.left
			x = x.left
		else x = x.right
	return x
```
- smallest key at the very left
- largest key at the very right
# Successor
```cpp
TREE-SUCCESSOR(x)
	if x.right != NIL
		return TREE-MINIMUM(x.right)
	y = x.p
	while y != NIL and x == y.right
		x = y
		y = y.p
	return y
```
- runs in time $O(h)$
# Insertion
```cpp
TREE-INSERT(T,z)
	y = NIL
	x = T.root
	while x != NIL
		y = x
		if z.key < x.key
			x = x.left
		else x = x.right
	z.p = y
	if y == NIL
		T.root = z             // Tree T was empty
	elseif z.key < y.key
		y.left = z
	else y.right = z
```
- runs in time $O(h)$
# Number of binary trees with n nodes
$$C_n = \frac{1}{n+1}\binom{2n}{n}$$

# BST Removal
- removal of node z
	- z has no children: replace with NIL
	- z has one child: z.p replaces z with z's child
	- z has two children: find successor y (in right subtree) -> z's left and right subtrees become y's subtrees
# Removal procedure intuition
- replace z with right child if no left chlid
- replace z with left child if no right child
- find succssor y in right subtree
	- if y = z.right, replace z and leave y's child
	- if y != z.right, replace y with y.right and then z with y
# Transplant 
- parent node changes either left or right pointer to given replacement node
- if new child node is not NIL -> update parent pointer
```cpp
TRANSPLANT(T,u,v)
	if u.p == NIL
		T.root = v
	elseif u = u.p.left
		u.p.left = v
	else v.p.right = v
	if v != NIL
		v.p = u.p
```
# Tree-delete 
- if y != z.right, replace it with its own right child and make y the right child of z, by making z.right point to y as a parent
```cpp
TREE-DELETE(T,z)
	if z.left == NIL
		TRANSPLANT(T,z,z.right)
	elseif z.right == NIL
		TRANSPLANT(T,z,z.left)
	else y = TREE-MINIMUM(z.right)
		if y.p != u
			TRANSPLANT(T,y,y.right)
			y.right = z.right
			y.right.p = y
		TRANSPLANT(T,z,y)
		y.left = z.left
		y.left.p = y
```

---
References: