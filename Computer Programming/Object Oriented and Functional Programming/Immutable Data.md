---
Tags: 
Created: 2023-10-24 22:01:23
Links: "[[Object Oriented and Functional Programming]]]"
---
Immutable data is data that can only be created, not changed. 
> [!question] Why do we use immutable data? 
> # Positive
> - It is easier to reason about
> - It is more flexible
> - There are less bugs
> - It is thread safe
> # Negative
> - There is a performance hit
> - It may be less intuitive

You can import mutable and immutable data structures in scala
```scala
scala.collections.mutable
scala.collections.immutable
```

| Mutable                          | Immutable                        |
| -------------------------------- | -------------------------------- |
| Array                            | Vector                           |
| ArrayBuffer                      | `scala.collection.mutable.Set`   |
| `scala.collection.mutable.Set`   | `scala.collection.mutable.Map`   |
| `scala.collection.mutable.Map`   | `scala.collection.mutable.Queue` |
| `scala.collection.mutable.Queue` |                                  |

In scala mutable sets have the same interface as immutable Sets, but with more methods for mutations.
Immutable code can be easier to reason about. When you create the set, you cannot change any of the values, so you know they will be the same later on. 

---
References: