---
Tags: 
Created: 2023-10-24 22:24:23
Links: "[[Object Oriented and Functional Programming]]"
---
A mathematical function $f:A\to B$ maps each value in $A$ to a single value $B$.
In computer programming, functions take in 0 or more arguments (which are then known) and side causes (which are affects from the outside which change the behaviour of the function). The function can then affect the outside world (**side affects**). These functions then return in the end 0 or more values (deterministic outputs).
Side affects can therefore be considered as hidden outputs, and side causes as hidden inputs. A **pure function** is one that has no side effects and no side causes. [[Functional programming]] supports the use of pure functions whilst [[Imperative programming]] uses more impure functions.

# Pure functions
**Advantages**:
- Easy to test
- Easier to reason about
- Compiler can optimize more aggressively
**Disadvantage**:
- We need side-effects & side-causes 

In some cases using impure constructors does not have to leak any data. Take for example the sort function:
```scala
def sort(n : List[Int]) : List[Int]
```
Even tho it might be imperative on the inside, it is functional to our eyes.
A feature of pure functions is **referential transparency**. If you can replace an expression with it's actual result without changing the meaning of the program, then it is referentially transparent. Replacing an impure function with its output will result in the side effects not being executed.

---
References: