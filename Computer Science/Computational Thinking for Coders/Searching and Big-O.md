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
## Correctness
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
## Evaluation
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

# [[Big O notation]]