---
Tags: 
Created: 2023-01-15 00:19:42
---
(Links:: [[Web Technology]])
# Syntax and variables
- variables aren't explicitly assigned a data type -> **dynamic typing** (varialbe type determined at run-time)
- uses [just-in-time compilation](https://en.wikipedia.org/wiki/Just-in-time_compilation)
# Arithmetic
- number + number = number
  number + string = string
  number \* number = number
  number \* string = number
- `parseInt()` and `parseFloat()` convert strings into numbers
# Conditionals
- *truthy*: non-Boolean value that evaluates to `true` in a Boolean context
  Ex: `if (32)`
  Truthy and Falsy values:
	- Non-zero number <-> Zero
	- Non-empty string <-> Empty string ("")
	- Object variable <-> Not a number / No object value (NaN/null)
	- Array variable <-> Variable that has not been assigned a value (undefined)
- Conditional operator: the conditional operator has three operands separated by a question mark and colon
  `condition ? expression1 : expression2`
# Functions
- Function expression: function is assigned to a variable; the function name may be omitted
  Ex: `let square = function(num){return num*num}`
- Arrow function: anonymous function that uses an arrow to create a compact function
  `(parameter1, parameter2, ...) => { statements; }`
  Ex: `let square = x => x * x`

# Scope and the global object
- variables declared inside a function have a **local scope** -> only the function has access to the local variable
- variables declared outside a function have **global scope** -> all functions have access to global variable
- `var` variables declared inside functions are accessible anywhere within the function
  `let` variables declared inside functions are accessible only within the enclosing pair of braces
- unassigned variables are by default global and accessible outside of the function
# Arrays
| Method                                                | Description                                                                                   | Example                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ----------------------------------------------------- | --------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `push(value)`                                         | Adds a value to the end of the array                                                          | `let nums = [2, 4, 6]; nums.push(8); //nums = [2, 4, 6, 8]`                                                                                                                                                                                                                                                                                                                                                                               |
| `pop()`                                               | Removes the last array element and returns the element                                        | `let nums = [2, 4, 6];`<br> `let x = nums.pop(); // returns 6, nums = [2, 4]`                                                                                                                                                                                                                                                                                                                                                             |
| `unshift(value)`                                      | Adds a value to the beginning of the array                                                    | `let nums = [2, 4, 6];`<br> `nums.unshift(0); // nums = [0, 2, 4, 6]`                                                                                                                                                                                                                                                                                                                                                                     |
| `shift()`                                             | Removes the first array element and returns the element                                       | `let nums = [2, 4, 6];`<br> ` let x = nums.shift(); // returns 2, nums = [4, 6]`                                                                                                                                                                                                                                                                                                                                                          |
| `splice(startingIndex, numElemToDelete, valuesToAdd)` | Adds or removes elements from anywhere in the array and returns the deleted elements (if any) | `let nums = [2, 4, 6, 8, 10];`<br><br> `//Deletes all elements from index 3 to the end` <br> `nums.splice(3); // nums = [2, 4, 6]` <br><br> `Deletes 2 elements starting at index 0` <br> `nums.splice(0,2); // nums = [6]` <br><br> `// Adds 3, 5 starting at index 0`<br>`nums.splice(0, 0, 3, 5); // nums = [3, 5, 6]` <br><br> `// Adds 7, 9, 11 starting at index 2`<br>`nums.splice(2, 0, 7, 9, 11); // nums = [3, 5, 7, 9, 11, 6]` |

- looping through an array with **for-of loop**: `for (let item of groceries){console.log(item);}`
- looping through an array with **forEach()**: 
  ```
  let groceries = ["bread", "milk", "peanut butter"];
  groceries.forEach(function(item, index) {
    console.log(index + " - " + item);
  })
  ```
- A function may modify an array argument's elements
# Objects
- unordered collection of properties
- properties consist of name-value pairs, with an identifier (name) and a data type (value)
- object properties may be assigned an anonymous function
	- methods have access to object's properties useing keyword `this`
- `get` and `set` keywords define getters and setters for a property
	- getter function is called when object's property is retrieved
	  `get property() {return someValue;}`
	- setter function is called when object's property is set to a value
	  `set property(value) {...}`
---
References: