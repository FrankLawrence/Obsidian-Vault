---
Tags: 
Created: 2022-10-26 19:46:08
---
(Links:: [[Computational Thinking for Coders]])
# MODULE 1

1 module per week:
WG assignments
Homework 
Homework grade: sum points / (number of homework *2) *10
Pass -> 6 and excellent -> 10
ZYbook: ch1-7 -> bonus point
Hand in the assignments in pdf version in Canvas
In week 3 -> some coding -> debugging
This course is intended to make a theoretical foundation

Computational thinking is the thought processes involved in formulating a problem and expressing its solutions in such a way that a computer – human or machine – can effectively carry out.
Analysing a problem -> formulating it -> expressing a fit solution

Logic is an important theoretical foundation for CS.
Socrates is a man -> all men are mortal -> hence Socrates is mortal.
You can reason only in one direction
When debugging programs (removing failures) logical thinking is the basis of spotting mistakes

Algorithmic thinking
Algorithm is a step-by-step description, “a recipe”, for solving a particular problem or for computing something that can be implemented in a computer program
State expressing variables
Selection (if – then - else)
Iteration (do something until some condition holds)
Pseudocode: any free-form description that explains precise enough which steps to perform and how state is computed

Devise an algorithm to compute the area of the given hexagon

Area = (3√3 s2)/ 2 where s is the length of a side of the regular hexagon.

Pseudocode: 
1. calculate bottom line
2. calculate height of hexagon
3. multiply the bottom line with the height of the hexagon
4. calculate width of hexagon
5. subtract bottom line length of the width of the hexagon and divide the answer by two.
6. multiply the answer of that with half of the height of the hexagon.
7. multiply this answer by 2
8. add the answers from lines 3 and 7 and you have the surface area
Calculation Surface area=(Xd-Xe)*(Ya-Ye)+(Xe-Xf)*(Ya-Yf)*2

(Ya - Yb)(Xd - Xe) + 0.5(Ya-Yb)(Xe-Fe) * 2
= (Ya - Yb)(Xd - Xe) + (Ya - Yb)(Xe - Fe)
= (Ya - Yb)(Xe - Xe + Xe - Fe)
= (Ya - Yb)(Xd - Fe)

A hexagon is a shape of 6 points with lines between them.

Problem solving strategies
Brute force: simply enumerate all possible solutions in the search for the best one (this can be SLOW)
Devide-and-conquer / decomposition
Devide the problem at hand into multiple, smaller problems that can be solved independently -> week 4; recursion
Patterns and generalisation 
Recognise solutions you have seen before and transfer them to the new problem

Goal: Create a Mental toolbox 

Devise a solution for the maximum profit problem.

We have n jobs, where every job is scheduled to be done from startTime[i] to endTime[i] , obtaining a profit of profit[i] . You need to output the maximum profit you can take, such that there are no two jobs in the subset with an overlapping time range. If you choose a job that ends at time x you will be able to start another job that starts at time x.

Time 1-3 and time 3-10

1-3 and 4-6 and 6-9

1) Divide each profit by the total time
2) set currentTime = 0
3) Start at time currentTime and choose the best profit/time job that has startTime >= currentTime and endTime <= totalTimeInDay
4) Set currentTime = endTime of the chosen job
5) Repeat step 2 until there's no jobs with startTime >= currentTime


1. List all jobs by start/end time
2. Sort from least to most hours
3. Iterate through the list left to right skipping overlapping hours
4. Sort by profit?

Brute force is the only way to know the absolute maximum profit and it’s possible because it’s a small problem.
All possible solutions and combinations. 
Abstraction
Abstraction is key to programming and problem solving
Data abstraction: build objects that are composed of a lower abstraction level
A vector of numbers
A date, consisting of day, month, year
Code abstraction: group re-usable sequences of code into functions/methods/procedures
Abstract data types: providing certain operations on some hidden (internal) state/data. 



# MODULE 2

What is a good solution?
- Is it correct?
- Is it efficient?
- Is it elegant?
- Is it useful?

**Correctness**
- The most essential property of a program
- Mathematical proof -> very complex -> the proof itself needs to be proven
- Testing can only prove the presence of defects, but never their absence (this is what we mostly do)

**Efficiency**
- Does a program (an algorithm) run as fast as possible for the program at hand?
- Does a program use only reasonable amounts of memory for its variables?
- Trade-off:
  => We can make an algorithm faster by storing more partial results
  => We can save on memory by repeating computations

**Elegance**
- Can a program (an algorithm) be easily understood by human readers?
- Each useful program needs continuous modification and extension due to changing requirements
- Unreadable code is not suitable for improvement or extension!
- Clean code

**Usefulness**
- Does a program serve a purpose?
- Is the program easy to understand and use (by the users)?

**Evaluation**
- Quantifiable properties:
  => Correctness (yes/no)
  => Efficiency -> in terms of **big-O notation**
- Non-quantifiable properties: (subjective)
  => Elegance
  => Usefulness

**Linear search** (a kind of search algorithm)
- If our data is not sorted, linear search is our only option
- Linear search is an example of brute force: try out all possibilities, one after one…

**Binary search**
- With each step you split the problem size by half 
- Much more efficient than linear search

**Max runtime of search**
- For a vector N elements
- Linear search: 
  -> Worst case, we must compare with all N elements in the vector
- Binary search:
  -> Worst case, we must divide the vector log2 N times

Devise an algorithm that plays a number guessing game:
- Your opponent secretly selects a positive integer number, 1 or larger, but **without any upper limit**.
- Your algorithm must guess this number with a minimal number of guesses.
	- For each guess, your opponent answers whether the guessed number is correct, too small, or too large.
	- Hint: use a (variation of) binary search!
The possibilities: 1 to infinity

Start guessing at 1, then double that number until they say it's lower, then start doing binary search from there -> opposite of binary search -> exponential doubling

**Big O notation**
- Determine how the complexity (number of operations computed) depends on the size of the problem.
- With O notation, we care about how a program behaves when the input grows
- Usually, we aim at **worst-case behaviour**
  => Sometimes at **average case**

**Big-O notation of thumb**
- Computations independent of the problem size N are constant: O(I)
- Loop: c*N = O(N)
- Nested loop: O(N^2)
- Binary division O(logN)
  => Deeper nesting leads to higher exponents
- Search space exploration (brute force, optimisation) -> O(c^N)
  => REALLY slow

What is the worst-case O-complexity?

Logarithmic: every iteration: a variable is being halved -> O(logN)

Look at the structure of the code -> don’t think about what it will compute

*Is big-o always the right measure?*
- For small problem sizes, algorithm with inferior O complexity may still need fewer operations than an algorithm with better O complexity
- If you optimise a program from running 4 hours down to 2 hours, you still save 2 hours!
  => If your problem size remains stable, also lowering the constant values can be important.

**Big O for time and space**
- Most often, we care about the runtime complexity of a program and use big O analysis
- But also the amount of memory
	- Time-space trade-off
	- Time: how long does it run?
	- Space: how much memory does it need

What is more important? -> it depends on the problem and on the device that’s going to solve the problem
- Time: when we care about how fast we get our results
- Space: when our dataset is very large or when our computer is a limited device e.g. a sensor in the field

Trading space for time
- Idea: storing partial results / common sub expressions in additional variables to avoid re-computation

Prefix maximum
In groups of 3-5 students, write some pseudo code to compute prefix maximum values:
Given some vector of numbers, e.g. [3, -5, 6, 7, 2, -3, 8]
input some index position i (from the user)
output the prefix maximum of the values in the vector in the index range from 1..i
then output all prefix maximum values in the index range from 1..n with n being the size of the vector
Compute each prefix maximum value no more than once!
```cpp
max = list[0]

for (i = 1; i < n; ++i)
	if list[i] > max
		max = list[i]
	end
end

print max

vector = []
declare maxvalue = vector[1];
input >> n

for (i = 1; i < n; ++i)

	if i > maxvalue 
	maxvalue = i

print maxvalue
```

1. Binary Search: O(log n)
2. Linear Search: O(n)
3. Quick Sort: O(n * log n)
4. Selection Sort: O(n * n)
5. Travelling salesperson : O(n!)

A good quality solution:
Is it correct? Does it actually solve the problem you set out to solve?
Is it efficient? Is it as fast as it can be? Does it only use strictly necessary amounts of storage/memory? 
Is it elegant? Is it simple yet effective? 
Is it usable? Does it provide a satisfactory way for the target audience to use it?
Often, efficiency is given in Big-O notation, which is covered later. Big O notation is a mathematical way of describing how an algorithm generally behaves in relation to the input size. For example, O(N) means that the algorithm will take at most c*N steps to execute, where N is the size of the input, and c is a constant value.

Correctness and efficiency are not subjective, elegancy and usability are.

The space-time trade-off describes an inverse proportionality between space-complexity and time-complexity. In other words, some solutions may use less memory, but require more time to compute and vice versa. 

Time-correctness trade-off

Cleverness-comprehensibility trade-off

Choosing the right trade-offs and the extent of these trade-offs is what distinguishes a mediocre developer from a great programmer.

An algorithm is a sequence of steps for accomplishing a task. Linear search is a search algorithm that starts from the beginning of a list, and checks each element until the search key is found or the end of the list is reached.

An algorithm's runtime is the time the algorithm takes to execute.

For a list with N elements, linear search thus requires at most N comparisons. The algorithm is said to require "on the order" of N comparisons.

Binary search is a faster algorithm for searching a list if the list's elements are sorted and directly accessible (such as an array). 

Log(base two) N + 1 = binary search rumtime

Big O notation is a mathematical way of describing how a function (running time of an algorithm) generally behaves in relation to the input size.

Merge sort and quicksort are examples of sorting algorithms that have a complexity of O(N log N).

The worst-case runtime of an algorithm is the runtime complexity for an input that results in the longest execution.

Constants are omitted in big-O notation, so any constant number of constant time operations is O(1).
The for loop iterates N times. Big-O complexity can be written as a composite function and simplified.
Constant time complexity -> executing a constant number of operations

Runtime analysis for nested loops requires summing the runtime of the inner loop over each outer loop iteration. The resulting summation can be simplified to determine the big-O notation.


Big-O notation omits the constant values, and the runtime is equal to the summation of the total inner loop iterations.



# Module 3
Call by reference is constant; doesn’t matter how big the vector is -> it always takes the same time

Big o of the loop assignment:
O(n^2)
The program is correct.

“Anything that can go wrong, will go wrong.” 

Types of errors

Compile-time errors
Syntax, type
Link-time errors -> missing main function
Some code is missing
Run-time errors
Detected by the computer, library, user code
Logic errors
Computer does/computes the wrong thing

Actual error messages differ by compiler

Catching type errors is one of the most useful features of a computer

Fixing compile-time errors:
Always only fix the first problem mentioned by the compiler -> the rest might be follow-up issues

Logic errors:
a. there is no output at all
b. the output is just plain wrong
Your program runs, but the problem is with the logic of your program

Estimation (apply “common sense” to your results)
Testing/debugging (be systematic and analytic)

Estimation
Apply common sense to verify a result produced by a computer, esp when:
It is your own program
It is your income tax statement
“back-of-the-envelope calculation” or a guesstimate
Estimation of an educated guess

Debugging
Debugging is the process of removing mistakes from a program’s code
The notion bug is used for all kinds of code mistakes

# Module 4
Sorting
- Programmers call “sort()”
- Computer scientists write sorting algorithms 
- Sorting is the process of converting a list of elements into ascending (or descending) order
- Humans often sort by looking simultaneously at many elements and moving them around
- Computer algorithms have only limited “view”

Comparison-based sorting
For general-purpose sorting, an algorithm must compare the list elements with each other
The naïve sorting algorithms compare “all-to-all”

Bubble sort
The most simple sorting algorithms you can think of
Pseudocode/idea:
for index = 1 up to size
list.at(index) <- index-smallest value]
Time complexity: two nested loops with the number of iterations depending on list size -> O(n^2)
Space complexity: Only constant amount of space, because no extra variable 

Selection sort, similar to bubble sort

Insertion Sort
Pseudocode:
for index = 1 up to size:
	put list.at(index) into partially sorted list
Time complexity: O(n^2)
Space: O(1)

Stable sorting
This is about list elements with identical values
A sorting algorithm is called stable if whenever there are two objects (R and S) with the same key, same value and R appears before S in the original, then R also should appear before S in the sorted list
This is important for real-world data records that more information than the “number” we sort on
Example: sort submissions to a pr assignment by student name but keep the order of 

Selection sort works by finding the minimum element and then inserting it in its correct position by swapping with the element which is in the position of this minimum element. This is what makes it unstable.

The power of binary subdivision
Compare:
Linear search: for 1 element, compare it with all others in a list -> O(n)
Binary search: check whether an element is in the left or the right half of a list, divide recursively log n times -> O(log n)
So far: for all n elements, compare each to all other elements -> O(n^2)
Idea: (recursively) divide the list in halves (log n phases), in each phase do a total of O(n) steps, leading overall to O(n log n)

Quicksort
Quicksort repeatedly partitions a list into low and high parts (each unsorted) then recursively sorts these partitions
Base case: 1 or 0 elements left 

Merge sort: divides the list into two halves -> sorts the two halves recursively -> binary search 

https://medium.com/karuna-sehgal/an-introduction-to-bucket-sort-62aa5325d124 

# Module 5

**Vector** 
In C++ vectors have dynamic size, we can expand (push_back() ) and shrink (pop_back() ) them
This come at a price: e.g. push_back() is O(n) but: at() is O(1)!
A vector is sort abstract data type; they are a powerful tool but are too low level at times both due to the random access using at()
Discussing the vector
Low level of abstraction, it directly exposes all its data elements (via at() )
Users often need to know how many items are in a vector, iteration over all elements is common

Abstract Data Types
An ADT is defined by its operations bla bla

**Stack** 
Ordered collection of items
Access is restricted to the end/top of the stack using push() and pop()
Reading the top element is via top()
Often there is also an isEmpty() or sometimes size() function
Implements a LIFO abstraction; last in first out
We need not know how many elements are on a stack, we can always push more; limited by the kind of implementation; memory of machines
Push can be O(n) if the implementation uses a vector inside but also O(1) if we use a linked list (Week 6)
Example: matching parentheses
Writing a function that can test whether an input stream containing properly matched opening and closing parentheses
E.g. “( ( ) )” good!
“( ( )” bad! “) ( )” bad!
Real life version: nested HTML tags “<div> <span> </span> </div>”

**Queue**
Ordered collection of items FIFO
Access is restricted to back and front of the queue
push() at the back and pop() from the front
front() and back() return the items at the two end (no removal)
push() and pop() can be O(1)
Application:
Many applications in asynchronous settings e.g. where multiple programs communicates; many application areas of queues
E.g. an interactive program with GUI that must respond to user input is using an event queue:
The GUI inserts events(mouse click, window resize)
The event handler takes events out of the queues whenever the execution permits
In C++, cin and cout operate like queues:
For cin, the OS puts characters into the stream an the program removes them in the order in which it has been put into cin
For cout, the program puts characters into the stream ….bla bla 

Queues instead of cout
towers of Hanoi program
classes

**Deque**
experts pronounce it “deck”
double-ended queue
works like a queue but works both ways:
push_front()
push_back()
pop_front
pop_back() -> increases the size the deque
applications:
undo history of a program (or browser history) :
push_back() new operations
pop_back() for un-doing
pop_front() for cleaning the history (“remove everything older than …”),
or if the undo history is overflowing
a queue or a deque doesn’t have a notion of index between the top and the end
a deque, stack and queue are all a sort of ordered data structure that you either add to the front or the back, stack does have a notion of index, but a deque or a queue has a certain ordering

**SET**
non-ordered data collection
defines a collection of unique elements, no order
some values of the base type are in the set, others are not
operations:
e = element
insert(e) – (ignore return value)
erase(e) – (ignore return value)
count(e) – returns 1 if e is in the set, 0 if not // prefer a function called “exist” or “contain” that has a boolean value 
size() – number of elements in the set
multi set: multiple instances of the same elements; complex object that is more than the key value that is put in there 
the concept of set is that you put it in there, remove it in there and check if it exist
Time complexity:
If based on a vector:
insert(e), erase(e), count(e): O(n)
size(): O(1)
If based on a binary search tree:
insert(e), erase(e), count(e): O(log n)
size(): O(n), O(1) if kept separately
higher-level abstraction: look at the time complexity 
applications:
happy numbers
any unordered collection of items that share a property like “have been visited” or “are active”; important to check if an element is in there and not the number of iterations

**MAP**
a map is a lookup table from a type keys K to a type of values V
maps are also called “associative arrays”
a vector is (a bit) like a map from int to a value type
a set is like a map from a key type to bool
a map is an example of a template with two type parameters

emplace()
emplace(key, value) 
Associates key with specified value. If key already exists, the map entry is not changed.
// map originally empty
exMap.emplace("Tom", 14); 
// map now: Tom->14,  
exMap.emplace("John", 86); 
// Map now: Tom->14, John->86


at()
at(key)
Returns the entry associated with key. If key does not exist, throws an out_of_range exception.
// Assume map is: Tom->14, John->86, Mary->13
exMap.at("Mary") = 25;  
// Map now: Tom->14, John->86, Mary->25
exMap.at("Tom")  // returns 14 
exMap.at("Bob")  // throws exception


count()
count(key) 
Returns 1 if key exists, otherwise returns 0.
// Assume map is: Tom->14, John->86, Mary->13
exMap.count("Tom")  // returns 1 
exMap.count("Bob")  // returns 0


erase()
erase(key) 
Removes the map entry for the specified key if the key exists.
// Assume map is: Tom->14, John->86, Mary->13
exMap.erase("John");  
// map is now: Tom->14, Mary->13


clear()
clear() 
Removes all map entries.
// Assume map is: Tom->14, John->86, Mary->13
exMap.clear();  
// map is now empty




Map complexity
Typically implemented using binary search tree
emplace(), erase(), count(), at(): O(log n)
clear(): O(n)
 if implemented with underlying vector
All is O(n)
Map applications
Property lists, e.g. as in CSS style sheets:
Text size, font, border width, alignment, …
Everything where you need a lookup table where the common case Is that for many keys there is no value 

Inserting into a queue, by definition, can only happen at the back of the queue, similar to someone getting in line for a delicious Double-Double burger at In 'n Out. Assuming an efficient queue implementation, queue insertion is O(1).


Deleting from a queue happens at the front of the queue. Assuming an efficient queue implementation, queue deletion is O(1).

Stack: Push something on the top and pop something on the top. Lifo abstraction 
Implementation 
Push operation can be order of O(n); O(1); constant time; linked list 

Queue: a queue is an ordered collection of FIFO; first in first out 
Access to elements is restricted to the back and the front of the queue
Operations are called enqueue and dequeue; push and pop
Push works from the back and pop works from the front
Push and pop can be constant time; linked list

Set: unordered data collections, no order among the elements in rank; distinguishing between set and multi set; operations ignore the return van of insert and erase; give us information whether the element was already in the set or not; time complexity; vector: insert erase, order of n; vector underneath test whether an element with an particular key or not is in the vector; you have to go through the vector; you have to iterate through it

Higher level abstraction -> time complexity of the operations

Map: lookup table from a type of keys k to a type of values v; any key type to any value type; compare two values of that type
Binary search tree: O(logn) 
Underlying vector: O(n) 

https://www.bigocheatsheet.com/

https://opencourses.emu.edu.tr/pluginfile.php/19102/mod_resource/content/1/AnalysisofAlgorithms_CS1037a.pdf



___
References: