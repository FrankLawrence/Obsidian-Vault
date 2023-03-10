---
Tags: [lecture]
Links: [[Computational Thinking for Coders]]
---
# Sorting
## *Stable Sorting*
- This is important for real-world data records that contain more information than the "number" we sort on..
- Examples:
	- sort submissions to a programming assignment by student name, but keep the order of multiple attempts by the same student

This is about list elements with identical values
A sorting algorithm is called stable if whenever there are two objects (R and S) with the same key, same value and R appears before S in the original, then R also should appear before S in the sorted list
This is important for real-world data records that more information than the “number” we sort on
Example: sort submissions to a pr assignment by student name but keep the order of 

Selection sort works by finding the minimum element and then inserting it in its correct position by swapping with the element which is in the position of this minimum element. This is what makes it unstable.

## *Quicksort*
- Quicksort repeatedly partitions a list into low and high parts (each unsorted) and then recursibely sorts these partitions
- base case: 1 or 0 elements left
- Two partitions left and right of the pivot 
	- Left partition is smaller than the pivot
	- Right partition is bigger than the pivot
- Time:
	- best case: $O(n\log n)$
	- worst case: $O(n^2)$
- Space: $O(n)$
## *Merge Sort*
- Time: $O(n\log n)$
- Space: $O(n)$
## *How to sort in practice?*
- Small data sets (fits in vectors in memory): call sort(), using quicksort
- Large data sets (files on disk): merge sort
___
References:

Created: 2022-09-27 15:55