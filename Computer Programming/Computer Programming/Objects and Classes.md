---
Tags: 
Created: 2022-10-03 10:08:56
---
(Links:: [[Computer Programming]])
# Objects: Introduction
## Grouping things into objects
- higher-level objects to group lower-level items
- a program consists of items like variables and functions
- an **objects** is a grouping of data (variables) and operations that can be performed on that data (functions)
## Abstraction / information hiding
- **Abstraction** means to have a user interact with an item at high-level, with lower-level internal details hidden from the user
- An **abstract data type (ADT)** is a data type whose creation and update are constrained to specific well-defined operations
# Using a class
## Classes intro: Public member functions
- a **class** construct defines a new type that can group data and functions to form an object
- A class' **public member functions** indicate all operations a class user can perform on the object
- The user need only understand how each public member function behaves
## Using a class
- A programmer can create one or more objects of the same class
- Declaring a variable of a class type creates an **object** of that type
	- Ex. `Restaurant favLunchPlace;` declares a Restaurant object named favLunchPlace
- The "." operator, known as the **member access operator**, is used to invoke a function on an object
	- Ex. `favLunchPlace.SetRating(4)` calls the SetRating() function on the favLunchPlace object, which sets the object's rating to 4
# Defining a class
## Private data members
- class definitions have **private data members**: variables that member functions can access but class users cannot
## Defining a class' public member functions
- functions are *declared* after the word "public:" in the class definition
- A **function declaration** provides the function's name, return type, and parameter types, but not the function's statements.
- A **function definition** provides a class name, return type, parameter names and types, and the function's statements
- A member function definition has the class name and two colons (::), known as the **scope resolution operator**, preceding the funtion's name
```cpp
class MyClass {
	public:
		void Fct1();

	private:
		int numA;
};
void MyClass::Fct1() {
	numA = 0;
}
```
## Example
```cpp
#include <iostream>
#include <string>
using namespace std;

class Restaurant {                          // Info about a restaurant
   public:
      void SetName(string restaurantName);  // Sets the restaurant's name
      void SetRating(int userRating);       // Sets the rating (1-5, with 5 best)
      void Print();                         // Prints name and rating on one line
   
   private:
      string name;
      int rating;
};

// Sets the restaurant's name
void Restaurant::SetName(string restaurantName) {
   name = restaurantName;
}

// Sets the rating (1-5, with 5 best)
void Restaurant::SetRating(int userRating) {
   rating = userRating;
}

// Prints name and rating on one line
void Restaurant::Print() {
   cout << name << " -- " << rating << endl;
}

int main() {
   Restaurant favLunchPlace;
   Restaurant favDinnerPlace;
   
   favLunchPlace.SetName("Central Deli");
   favLunchPlace.SetRating(4);
   
   favDinnerPlace.SetName("Friends Cafe");
   favDinnerPlace.SetRating(5);
   
   cout << "My favorite restaurants: " << endl;
   favLunchPlace.Print();
   favDinnerPlace.Print();
   
   return 0;
}
```
# Inline member functions
- A member function's definition may appear within the class definition -> **inline member function**
```cpp
class MyClass {
	public:
		void Fct1() {
			numA = 0;
		}
	private:
		int numA;
};
```
## Exceptionto variables being declared before used
> Normally, items like variables must be declared before being used, but this rule does not apply within a class definition. Ex: Above, SetRating() accesses rating, even though rating is declared a few lines after. This rule exception allows a class to have the desired form of a public region at the top and a private region at the bottom: A public inline member function can thus access a private data member even though that private data member is declared after the function.
# Mutators, accessors, and private helpers
## Mutators and accessors
- A **mutator** function may modify ("mutate") a class' data members -> **setter** function
- An **accessor** function accesses data members but does not modify a class' data members -> **getter** function
- Accessor functions are defined as cont to make clear that data members won't be changed: `string GetName() const;
## Private helper functions
- A programmer commonly creates private functions, known as **private helper functions**, to help functions carry out tasks
- Any member function (public or private) may call a private member function
```cpp
class MyClass {
	public: 
		void Fct1();
	private:
		int numA;
		int FctX();
};

void MyClass:Gct1() {
	numA = FctX();
	...
}

int MyClass:FctX() {
	...
}
```
# Initialization and constructors
A *good practice* is to initialize all variables when declared
```cpp
class Restaurant {
   public:
      void SetName(string restaurantName);
      void SetRating(int userRating);
      void Print();
   
   private:
      string name = "NoName";  // NoName indicates name was not set
      int rating = -1;         // -1 indicates rating was not set
};
int main() {
   Restaurant favLunchPlace;  // Initializes members with values in class definition
   return 0;
}
```
## Constructors
- C++ has a special class member function, a **constructor**, *called automatically* when a variable of that class type is declared, which can initialize data members
- A constructor has the same name as the class.
- A constructor has no return type, not even void: `Restaurant::Restaurant() {...}`
- Compiler *implicitly* defines a default constructor having no statements, if there's no programmer-defined constructor
- Use a constructor when it's too complicated to initialize in the class definition
# Separate files for classes
## Two files per class
Programmers typically put all code for a class into two files, separate from other code.
- **ClassName.h** contains the class definition, including data members and member function declarations
- **ClassName.cpp** contains member function definitions

___
References: