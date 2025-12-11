---
Tags: 
Created: 2023-10-25 17:22:09
Links: "[[Object Oriented and Functional Programming]]"
---
# Pattern matching
Pattern matching is an alternative to [[Generics, Subtyping and Inheritance#Subtyping|subtype polymorphism]]. The positive thing is that it can **match deeply** (more on this later). Match is an expression and can match case classes and bind values to a variable.
> [!example]+ Linear Pattern
> When using match, every variable can only occur once:
> > [!failure]
> > ```scala
> > def simplifyTop(expr : Expr) : Expr = expr match { 
> > 	case BinOp(e, "+", e) => BinOp(Number(2),"*",e) 
> > 	case _ => expr
> > }
> > ```
> 
> We can use pattern guards to get around this hinderance:
> ```scala
> def simplifyTop(expr : Expr) : Expr = expr match { 
> 	case BinOp(l, "+", r) if l == r => BinOp(Number(2),"*",l) 
> 	case _ => expr
> }
> ```

If we don't care about the contents of an expression, we insert an `_`:
> [!example]+ Wildcards
> ```scala
> def describe(expr : Expr) : String = expr match { 
> 	case Var(_) => "A variable"  
> 	case Number(_) => "A number"  
> 	case UnOp(_,_) => "A unary operator"
> 	case BinOp(_,_,_) => "A binary operator" 
> }
> ```

We can use a variable which will match anything and is assigned the value, which can be used later.
```scala
expr match {
	case 0 => "zero"
	case somethingElse => "not zero: " + somethingElse
}
```

Usually you'd need a base case (`case _ => ...`) if the match doesn't match any subclasses. By prepending the class with the `sealed` operator, the compiler knows that the match will be exhaustive.
# Tail recursion
Tail-recursive means that the recursive call is directly after "return":
```scala
def approximate(guess : Double) : Double = {
	if(isGoodEnough(guess)) guess
	else approximate(improve(guess))
}
```
If for the implementation of factorial for example, the stack would increase on every method call. Tail recursion solves this problem and keeps the stack size at 1.
> [!example]- Tail recursive factorial
> ```scala
> def factorial(n : Int) : Int = {  
> 	def facTailRec(n : Int, accum : Int) : Int = n match {
> 		case 0 => accum
> 		case n => facTailRec(n-1, accum * n) // tail recursive 
> 	}
> 	facTailRec(n,1) 
> }
> ```
> An even better way would be to avoid tail recursion completely:
> ```scala
> def factorial(n : Int) = (1 until n).fold(1)(_*_)
> ```

---
References: