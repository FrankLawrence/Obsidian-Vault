---
Tags: 
Created: 2022-10-26 20:59:29
---
(Links:: [[Computational Thinking for Coders]])
# Evaluating a solution
1. Is it **correct**? Does it actually solve the problem you set out to solve?
2. Is it **efficient**? Is it as fast as it can be? Does it only use strictly necessary amounts of storage/memory?
3. Is it **elegant**? Is it simple yet effective?
4. Is it **usable**? Does it provide a satisfactory way for the target audience to use it?
## Correctnes
- Use a series of tests to check for edge-case scenarios
- The most essential property of a program
- Mathematical proof -> very complex -> the proof itself needs to be proven
- Testing can only prove the presence of defects, but never their absence (this is what we mostly do)
## Efficiency
Measured in terms of time and space
- **Time**: the duration of an algorithm's running time, from start to end. The duration can be measured in various ways such as the number of iterations, seconds or more.
- **Space**: the amount of memory storage required by an algorithm to do its work.
- Does a program (an algorithm) run as fast as possible for the program at hand?
- Does a program use only reasonable amounts of memory for its variables?
- Trade-off:
  => We can make an algorithm faster by storing more partial results
  => We can save on memory by repeating computations
## Elegance
> Simple can be harder than complex: You have to work hard to get your thinking clean to make it simple.
- Can a program (an algorithm) be easily understood by human readers?
- Each useful program needs continuous modification and extension due to changing requirements
- Unreadable code is not suitable for improvement or extension!
- Clean code
## Usability
How well does your solution cater to humans?
The ease of use of your solution is important.
- Does a program serve a purpose?
- Is the program easy to understand and use (by the users)?
## **Evaluation**
- Quantifiable properties:
  => Correctness (yes/no)
  => Efficiency -> in terms of **big-O notation**
- Non-quantifiable properties: (subjective)
  => Elegance
  => Usefulness
# Searching and algorithms
**Linear search** is a search algorithm that starts from the beginning of a list, and checks each element until the search key is found or the end of the list is reached.
- If our data is not sorted, linear search is our only option
- Linear search is an example of brute force: try out all possibilities, one after one…

**Runtime**: the time the algorithm takes to execute
# Binary search
**Binary search** is a faster algorithm for searching a list if the list's elements are sorted and directly accessible. Binary search first checks the middle element of the list. If the search key is found, the algorithm returns the matching location. If the search key is not found, the algorithm repeats the search on the remaining left sublist or the remaining right sublist.

Binary search halves the list in every iteration. The maximum number of steps required to reduce the search space to an empty sublist is $\log_2(N)$

- With each step you split the problem size by half 
- Much more efficient than linear search

**Max runtime of search**
- For a vector N elements
- Linear search: 
  -> Worst case, we must compare with all N elements in the vector
- Binary search:
  -> Worst case, we must divide the vector log2 N times
# Big O notation
Big O notation is a mathematical way of describing how a function generally behaves in relation to the input size.
Given a function that describes the running time of an algorithm, the Big O notation for that function can be determined using the following rules:
1. If f(x) is a sum of several terms, the highest order term (the one with the fastest growth rate) is kept and others are discarded.
2. If f(x) has a term that is a product of several factors, all constants (those that are not in terms of x) are omitted.

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

| Composite function | Big O notation    |
| ------------------ | ----------------- |
| c · O(f(x))        | O(f(x))           |
| c + O(f(x))        | O(f(x))           |
| g(x) · O(f(x))     | O(g(x)·O(f(x)))   |
| g(x) + O(f(x))     | O(g(x) + O(f(x))) |
___
References: