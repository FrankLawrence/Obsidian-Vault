---
Tags: Homework/Computer_Programming
Created: 2022-09-29 21:20
---
(Links:: [[Computer Programming]])
# Knight's Tour
## Pseudocode
```
bool findKnightsTour (board, start square, end square)
- if start is the same as end, return true (indicating success)
- loop over all possible knight's moves from start
	- if move directs to a valid square V (within board boundaries, not visited yet)
		- mark the board with V visited
		- success = findKnightsTour(board, V, end)
		- if success, return true
		- remove the mark of V being visited
- return false
```

___
References:
