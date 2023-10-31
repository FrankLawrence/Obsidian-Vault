---
Tags: 
Created: 2023-10-25 18:49:46
Links: "[[Object Oriented and Functional Programming]]"
---
# Advanced subtyping
Type systems rule out classes of bugs by disallowing certain behaviour. Type inference automatically detects the type and reduces verbosity. Scala can infer types of *variables* and *function return types*. It doesn't support inferring *arguments* or return types of *recursive functions*.
# Traits
A Trait is like an abstract class. A class can extend **multiple** traits but only one class. Traits define types and they do not have a constructor. 
# Variance
Lists are covariant in their type argument (`List[+A]`).
```scala
List[A] <: List[B] iff A <: B
```
Arrays are invariant in their type argument (`Array[A]`)
```scala
Array[A] <: Array[B] iff A = B
```

___
Functions are contravariant in their arguments, but covariant in their results: `A => B <: C => D` iff `A :> C` and `B <: D`


---
References: