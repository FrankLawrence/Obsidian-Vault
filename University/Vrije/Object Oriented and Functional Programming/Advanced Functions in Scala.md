---
Tags: 
Created: 2023-10-25 02:02:43
Links: "[[Object Oriented and Functional Programming]]"
---
# Local functions
Local functions can access the variables of the surrounding function:
```scala
def processFile(fileName : String, width : Int) = {
	def processLine(line : String) = {
		if(line.length > width) println(fileName + ": " + line.trim)
	}
}
```
With this, we don't need to pass all the arguments again. It also hides the function from the outside making it in some sort private.
# First class functions & Lambdas & Closures
First class functions allow us to use functions as values. They can be 
- the value of a variable, 
- used as arguments in other functions, 
- the result of a function

This gives use much more flexibility than otherwise. They are usually used as arguments for [[#Commonly used higher order functions]]
Lambdas (also known as anonymous functions or function literals) don't have a name and can be used directly:
```scala
def main(argv : Array[String]) = {
	val increase : Int => Int = (x : Int) => x + 1 // function literal
	println(increase(10))              // prints 11
	println(((x : Int) => x + 1)(10))  // prints 11
	println((x => x + 1)(10))          // prints 11
	println((_ + 1)(10))               // prints 11
}
```

The runtime object for the function value is called a **closure**. The definition of the function + bindings of the free variables. Name comes from "closing the function": providing bindings for the free variables.
# Higher order Functions
These functions take functions as arguments and/or return functions. As an example, a function could take in a function and return a function which does twice of the first:
```scala
def twice[A](f : A => A) : A => A = {
	(x : A) => f(f(x))
}
```
Function composition:
```scala
def compose[A,B,C](g : B => C, f : A => B) : A => C = 
	(x : A) => g(f(x))

def andThen[A,B,C](f : A => B, g : B => C) : A => C = 
	(x : A) => g(f(x))
```
## Commonly used higher order functions
With higher order functions we can write more concise code. 
**Map** returns the same data structure given with a function applied to each element. **Filter** checks each element against a predicate and returns a new data structure with the elements that pass the predicate. **DropWhile and takeWhile** do the same but either start or stop extracting when the predicate holds.
A for loop can be rewritten as a `foreach` function with possibly a `filter` function. For yield loops can be rewritten as a combination `map`, `filter`, `flatten` and `flatMap`:

> [!tip] Common translations
> ```scala
> for(x <- c1; y <- c2; z <-c3) {...}
> // is translated into
> c1.foreach(x => c2.foreach(y => c3.foreach(z => {...})))
> ```
>```scala
> for(x <- c1; y <- c2; z <- c3) yield {...}
> // is translated into
> c1.flatMap(x => c2.flatMap(y => c3.map(z => {...})))
> ``` 
> ```scala
> for(x <- c; if cond) yield {...}
> // is translated on Scala 2.7 into
> c.filter(x => cond).map(x => {...})
> // or, on Scala 2.8, into
> c.withFilter(x => cond).map(x => {...})
> ```
> ```scala
> for(x <- c; y = ...) yield {...}
> // is translated into
> c.map(x => (x, ...)).map((x,y) => {...})
> ```

`foldLeft(z,f,List(1,2,3,4,5,6,7))` applies a function `f` to `z` and the leftmost element in the List, takes the result and applies the function to the result and the next element until there is none. `foldRight` does the same but starting from the right side.
# Nested Classes
Like nested functions, we can organize our code by nesting classes. These classes can still however have access to methods/fields of the enclosing object.
# Currying
This is a different way of writing functions with 2 or more arguments. For a function $f(x,y)$ we would write $f(x)(y)$. 
```scala
// type (Int, Int) => Int
def plus1(x : Int, y : Int) : Int = x + y
// curried variant type Int => (Int => Int)
def plus2(x : Int) : Int => Int = y => x + y
// also curried, with additional argument list, also type Int => (Int => Int)
def plus3(x : Int)(y : Int) : Int = x + y
```

---
References: https://stackoverflow.com/questions/1052476/what-is-scalas-yield