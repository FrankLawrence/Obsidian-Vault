Big $O$ notation is a mathematical way of describing how a function generally behaves in relation to the input size.
Given a function that describes the running time of an algorithm, the Big $O$ notation for that function can be determined using the following rules:
1. If $f(x)$ is a sum of several terms, the highest order term (the one with the fastest growth rate) is kept and others are discarded.
2. If $f(x)$ has a term that is a product of several factors, all constants (those that are not in terms of $x$) are omitted.

- Determine how the complexity (number of operations computed) depends on the size of the problem.
- With $O$ notation, we care about how a program behaves when the input grows
- Usually, we aim at **worst-case behaviour**
  => Sometimes at **average case**

# Big-O notation of thumb
- Computations independent of the problem size N are constant: $O(I)$
- Loop: $c\times N = O(N)$
- Nested loop: $O(N^2)$
- Binary division $O(\log N)$
  => Deeper nesting leads to higher exponents
- Search space exploration (brute force, optimisation) -> $O(c^N)$
  => REALLY slow

# Is big-O always the right measure?
- For small problem sizes, algorithm with inferior $O$ complexity may still need fewer operations than an algorithm with better $O$ complexity
- If you optimise a program from running 4 hours down to 2 hours, you still save 2 hours!
  => If your problem size remains stable, also lowering the constant values can be important.

# Big-O for time and space
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

| Composite function    | Big O notation          |
| --------------------- | ----------------------- |
| $c \times O(f(x))$    | $O(f(x))$               |
| $c + O(f(x))$         | $O(f(x))$               |
| $g(x) \times O(f(x))$ | $O(g(x)\times O(f(x)))$ |
| $g(x) + O(f(x))$      | $O(g(x) + O(f(x)))$     |
___
References: