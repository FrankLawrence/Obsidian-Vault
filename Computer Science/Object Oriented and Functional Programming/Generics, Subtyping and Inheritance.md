---
Tags: 
Created: 2023-10-25 03:39:24
Links: "[[Object Oriented and Functional Programming]]"
---
# Polymorphism
Polymorphism stems from the greek meaning "many forms". Polymorphic code is code that works for multiple types. The goal is to reuse code. Example:
```scala
def swap[A,B](a : (A,B)) : (B,A) = (a._2,a._1)

val x : (Int, String) = (42, "Hello")
val y : (String, Int) = swap(x)
```
## Class parametric polymorphism
```scala
class Stack[A] { // Type parameter A 
	private var elements: List[A] = Nil  
	def push(x: A) { elements = x :: elements } 
	def peek: A = elements.head  
	def pop(): A = {
		val currentTop = peek 
		elements = elements.tail 
		currentTop
	} 
}
```
```scala
// instantiate A with Int
val stack = new Stack[Int] 
stack.push(1)  
stack.push(2) 
println(stack.pop) // prints 2 
println(stack.pop) // prints 1
```
# Subtyping
If we have a D that is a subtype of A (`D <: A`), then D supports at least the interface of A and can be used anywhere A is expected. Subclasses are a subtype but there are also other types of subtypes, ex. functions or traits.
For example the variable v in `var v : A` can hold any value of a subtype of A.
In scala everything is an object. Each object is a subtype of Any:
![[Anyref.canvas|Anyref]]
> [!example]- Animals
> ![[Animals Inheritance class example.canvas|Animals Inheritance class example]]

## Null
Values cannot be **Null**, whilst Reference types can.
Null is a subtype of any user defined class. It only has one value: `null`. It can be used everywhere a reference type is required.
```scala
val v : Animal = <any subtype of Animal>
```
## Nothing
Nothing is a subtype of everything and has **no values**. It can be used anywhere where a value is required. It is often used to signal that a function cannot return normally:
```scala
def error(msg : String) : Nothing = throw new Error(msg)

def divide(x: Int, y : Int) : Int = {
	if(y == 0) error("Division by zero")
	else x / y
}
```
# Inheritance
A subclass automatically inherits all definitions of the superclass. If a method is not implemented (abstract), then the subclass must implement it. If it has already been implemented, subclasses must override the method to have their own version.
> [!example]- Animals
> Animal has declared two abstract methods (`speak` and `name`) and defined two methods (`kingdom` and `describe`).
> The subclass `Parrot` implements all abstract methods, and overrides the `describe` function. Since `kingdom` has already been defined in the superclass, it mustn't be implemented again.
> ```scala
> abstract class Animal {
> 	abstract def speak : String
> 	abstract def name : String
> 	def kingdom : String = "Animalia"
> 	def describe = "The " + name + " says " + speak
> }
> class Parrot(val line : String) extends Animal {
> 	def speak = line
> 	def name = "Parrot"
> 	override def describe = line + ", says the " + name
> }
> ```

The program will always use the deepest implementation of a function. This is called **dynamic dispatch**.
If you don't want your class to have subclasses or your methods to be overridden in subclass, prepend it with `final` modifier.
If you want to explicitly use the super classes implementation of a function, you use the `super` modifier.

___
If we create a List of different types, then it's type will be the **lowest common ancestor** of the types within. For a list `List(a,b,c) : List[A]`, A is the **least upper bound** of a,b and c.
> [!example]- Variable type
> If we have a variable which changes value, then it must be defined with the least upper bound of the different values.
> ```scala
> var x : lub(A,B) = v : A
> ...
> x = w : B
> ```
> When returning a value, the return type must be the least upper bound of the different outputs.
> ```scala
> (if(c) x : Int else y : String) : Any
> ```

Lub distributes over tuples. $$lub((A,B),(C,D))= (lub(A,C),lub(B,D))$$
If you have a value of which you are certain which type it will be, then you can use `asInstanceOf[type]` to give a result of the type. It is however usually avoided, and the program should be possibly redesigned.

> [!example]
> ```scala
> var s : Any
> ...
> if(s.isInstanceOf[String]) println("String: " s.asInstanceOf[String])
> else println("Not String:" + s.toString)
> ```


---
References: