# If-else branches (general)
## Basics
In a program, a *branch* is a sequence of statements only executed under a certain condition. 
An *if* branch is a branch taken only ==IF== an expression is true.

## If-else branches
An *if-else* branch has two branches: The first branch is executed ==IF== an expression is true, ==ELSE== the other branch is executed.

## If-elseif-else branches
Commonly a programmer wishes to take one of multiple (three or more) branches. An if-else can be extended to an if-elseif-else structure. Each branch's expression is checked in sequence; as soon as one branch's expression is found to be true, that branch is taken. If no expression if found true, execution will reach the else branch, which then executes.
# Detecting equal values with branches
## Detecting if two items are equal using an if statement
Use the *equality operator* (*\==*) in the Braces of the if branch.
## Inequality operator
`!=`
## Comparing characters, strings, and floating-point types
The relational and equality operators work for integer, character, and floating-point built-in types.
Comparing characters compares their ASCII numerical encoding.
Floating-point types should not be compared using the equality operators, due to the imprecise representation of floating-point numbers.
# Detecting reanges with branches
## Detecting ranges using if-elseif-else
An if-elseif-else structure can detect number ranges with each branch performing a different action for each range. Each expression only needs to indicate the upper range part, if execution reaches an expression, the lower part is implicit from previous expressions being false. 
```
If age < 6: No teams
Else if age < 8: Play on U8 team
Else if age < 10: Play on U10 team
```
# Order of Evaluation
| Operator/Convention | Descrition                                                               | Explanation                                                                                                                                                           |
| ------------------- | ------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| ()                  | Items within parentheses are evaluated first                             | In  (a * (b + c)) - d, the + is evaluated first, then \*, then -.                                                                                                     |
| !                   | ! (logical NOT) is next                                                  | ! x \|\| y is evaluated as (!x) \|\| y                                                                                                                                |
| \*/%+-              | Arithmetic operators (using their precedence rules; see earlier section) | z - 45 \* y < 53 evaluates \* first, then -, then <.                                                                                                                  |
| < <= > >=           | Relational operators                                                     | x < 2 \|\| x >= 10 is evaluated as (x < 2) \|\| (x >= 10) because < and >= have precedence over \|\|.                                                                 |
| == !=               | Equality and inequality operators                                        | x == 0 && x >= 10 is evaluated as (x == 0) && (x >= 10) because < and >= have precedence over &&. == and != have the same precedence and are evaluated left to right. |
| &&                  | Logical AND                                                              | x == 5 \|\| y == 10 && z != 10 is evaluated as (x == 5) \|\| ((y == 10) && (z != 10)) because && has precedence over \|\|.                                            |
| \|\|                | Logical OR                                                               | \|\| has the lowest precedence of the listed arithmetic, logical, and relational operators.                                                                           |
