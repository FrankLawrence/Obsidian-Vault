---
Tags: 
Created: 2022-10-05 15:42:58
---
(Links:: [[Computational Thinking for Coders]])
# Module 4
## 3 Group Exercise
### 3.1 Bubble sort
```
{14, 27, 33, 35, 10}
{14, 27, 33, 10, 35}
{14, 27, 10, 33, 35}
{14, 10, 27, 33, 35}
{10, 14, 27, 33, 35}
```
- $O(n^2)$
- small space complexity
### 3.2 Selection sort
```
{10}
{10, 14}
{10, 14, 27}
{10, 14, 27, 33}
{10, 14, 27, 33, 35}
```
- $O(n^2)$
### 3.3 Insertion sort
```
{14}
{14, 33}
{14, 27, 33}
{14, 27, 33, 35}
{10, 14, 27, 33, 35}
```
- $O(n^2)$
### 3.4 Quick sort
```
{14, 10, 27, 35, 33}
{10, 14, 27, 35, 33}
{10, 14, 27, 33, 35}
```
- $O(n\log n)$
- The chosen pivot
	- if the pivot is always the biggest number, the time complexity is $O(n^2)$
### 3.5 Merge sort
```
{14} {33} {27} {35} {10}
{14, 33} {27} {10, 35}
{14, 27, 33} {10, 35}
{10, 14, 27, 33, 35}
```
- $O(n\log n)$
- FALSE, FALSE, FALSE
- Quicksort doesn't have as high of a space complexity as Mergesort
___
References: