---
Tags: 
Created: 2022-10-28 13:19:27
---
(Links:: [[Computational Thinking for Coders]])
# Vector
In C++ vectors have dynamic size, we can expand (`push_back()`) and shrink (`pop_back()`) them
This come at a price: e.g. `push_back()` is $O(n)$ but: `at()` is $O(1)$!
A vector is sort abstract data type; they are a powerful tool but are too low level at times both due to the random access using `at()`
Discussing the vector
Low level of abstraction, it directly exposes all its data elements (via `at()`)
Users often need to know how many items are in a vector, iteration over all elements is common

Abstract Data Types
An ADT is defined by its operations bla bla

# Stack
Ordered collection of items
Access is restricted to the end/top of the stack using `push()` and `pop()`
Reading the top element is via `top()`
Often there is also an `isEmpty()` or sometimes `size()` function
Implements a LIFO abstraction; last in first out
We need not know how many elements are on a stack, we can always push more; limited by the kind of implementation; memory of machines
Push can be $O(n)$ if the implementation uses a vector inside but also $O(1)$ if we use a linked list (Week 6)
Example: matching parentheses
Writing a function that can test whether an input stream containing properly matched opening and closing parentheses
E.g. “( ( ) )” good!
“( ( )” bad! “) ( )” bad!
Real life version: nested HTML tags 
<div> <span> </span> </div>

# Queue
Ordered collection of items FIFO
Access is restricted to back and front of the queue
`push()` at the back and `pop()` from the front
`front()` and `back()` return the items at the two end (no removal)
`push()` and `pop()` can be $O(1)$
Application:
Many applications in asynchronous settings e.g. where multiple programs communicates; many application areas of queues
E.g. an interactive program with GUI that must respond to user input is using an event queue:
The GUI inserts events(mouse click, window resize)
The event handler takes events out of the queues whenever the execution permits
In C++, `cin` and `cout` operate like queues:
For `cin`, the OS puts characters into the stream an the program removes them in the order in which it has been put into `cin`
For `cout`, the program puts characters into the stream ….bla bla 

Queues instead of cout
towers of Hanoi program
classes

# Deque
experts pronounce it “deck”
double-ended queue
works like a queue but works both ways:
`push_front()`
`push_back()`
`pop_front()`
`pop_back()` -> increases the size the deque
applications:
undo history of a program (or browser history) :
`push_back()` new operations
`pop_back()` for un-doing
`pop_front()` for cleaning the history (“remove everything older than …”),
or if the undo history is overflowing
a queue or a deque doesn’t have a notion of index between the top and the end
a deque, stack and queue are all a sort of ordered data structure that you either add to the front or the back, stack does have a notion of index, but a deque or a queue has a certain ordering

# Set
non-ordered data collection
defines a collection of unique elements, no order
some values of the base type are in the set, others are not
operations:
e = element
`insert(e)` – (ignore return value)
`erase(e)` – (ignore return value)
`count(e)` – returns 1 if e is in the set, 0 if not // prefer a function called “exist” or “contain” that has a boolean value 
`size()` – number of elements in the set
multi set: multiple instances of the same elements; complex object that is more than the key value that is put in there 
the concept of set is that you put it in there, remove it in there and check if it exist
Time complexity:
If based on a vector:
`insert(e)`, `erase(e)`, `count(e)`: $O(n)$
`size()`: $O(1)$
If based on a binary search tree:
`insert(e)`, `erase(e)`, `count(e)`: $O(\log n)$
`size()`: $O(n)$, $O(1)$ if kept separately
higher-level abstraction: look at the time complexity 
applications:
happy numbers
any unordered collection of items that share a property like “have been visited” or “are active”; important to check if an element is in there and not the number of iterations

# Map
a map is a lookup table from a type keys K to a type of values V
maps are also called “associative arrays”
a vector is (a bit) like a map from int to a value type
a set is like a map from a key type to bool
a map is an example of a template with two type parameters

## Functions
### `emplace()`
`emplace(key, value)` 
Associates key with specified value. If key already exists, the map entry is not changed.
```java
// map originally empty
exMap.emplace("Tom", 14);
// map now: Tom->14,  
exMap.emplace("John", 86);
// Map now: Tom->14, John->86
```

### `at()`
`at(key)`
Returns the entry associated with key. If key does not exist, throws an out_of_range exception.
``` java
// Assume map is: Tom->14, John->86, Mary->13
exMap.at("Mary") = 25; 
// Map now: Tom->14, John->86, Mary->25
exMap.at("Tom")  // returns 14 
exMap.at("Bob")  // throws exception
```

### `count()`
`count(key) `
Returns 1 if key exists, otherwise returns 0.
```
// Assume map is: Tom->14, John->86, Mary->13
exMap.count("Tom")  // returns 1 
exMap.count("Bob")  // returns 0
```

### `erase()`
`erase(key)` 
Removes the map entry for the specified key if the key exists.
```java
// Assume map is: Tom->14, John->86, Mary->13
exMap.erase("John");
// map is now: Tom->14, Mary->13
```

### `clear()`
`clear()`
Removes all map entries.
```java
// Assume map is: Tom->14, John->86, Mary->13
exMap.clear();
// map is now empty
```

## Map complexity
Typically implemented using binary search tree
`emplace()`, `erase()`, `count()`, `at()`: $O(log n)$
`clear()`: $O(n)$ if implemented with underlying vector
All is $O(n)$

## Map applications
Property lists, e.g. as in CSS style sheets:
Text size, font, border width, alignment, …
Everything where you need a lookup table where the common case Is that for many keys there is no value 

# Complexity
Inserting into a queue, by definition, can only happen at the back of the queue, similar to someone getting in line for a delicious Double-Double burger at In 'n Out. Assuming an efficient queue implementation, queue insertion is $O(1)$.

Deleting from a queue happens at the front of the queue. Assuming an efficient queue implementation, queue deletion is $O(1)$.

**Stack**: Push something on the top and pop something on the top. Lifo abstraction 
Implementation 
Push operation can be order of $O(n)$; $O(1)$; constant time; linked list 

**Queue**: a queue is an ordered collection of FIFO; first in first out 
Access to elements is restricted to the back and the front of the queue
Operations are called enqueue and dequeue; push and pop
Push works from the back and pop works from the front
Push and pop can be constant time; linked list

**Set**: unordered data collections, no order among the elements in rank; distinguishing between set and multi set; operations ignore the return van of insert and erase; give us information whether the element was already in the set or not; time complexity; vector: insert erase, order of n; vector underneath test whether an element with an particular key or not is in the vector; you have to go through the vector; you have to iterate through it

Higher level abstraction -> time complexity of the operations

**Map**: lookup table from a type of keys k to a type of values v; any key type to any value type; compare two values of that type
Binary search tree: $O(\log n)$
Underlying vector: $O(n)$

___
References: