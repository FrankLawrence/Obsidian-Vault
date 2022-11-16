Status:
Tags: #programming #learning 
Links: [[Computer Programming]]
___
# How to write clean code?
## Lessons learnt from "The Clean Code"
There are two things- Programming and Good Programming. Programming is what we have all been doing. Now is the time to do good programming. We all know that even the bad code works. But it takes time and resources to make a program good. Moreover, other developers mock you when they are trying to find what all is happening in your code. But it’s never too late to care for your programs.

This book has given me a lot of knowledge on what are the best practises and how to actually write code. Now I feel ashamed of my coding skills. Though I always strive to better my code, this book has taught a lot more.

Now, you are reading this blog for two reasons. First, you are a programmer. Second, you want to be a better programmer. Good. We need better programmers.

Characteristics of a Clean code:

It should be elegant — Clean code should be pleasing to read. Reading it should make you smile the way a well-crafted music box or well-designed car would.
Clean code is focused —Each function, each class, each module exposes a single-minded attitude that remains entirely undistracted, and unpolluted, by the surrounding details.
Clean code is taken care of. Someone has taken the time to keep it simple and orderly. They have paid appropriate attention to details. They have cared.
4. Runs all the tests

5. Contains no duplication

6. Minimize the number of entities such as classes, methods, functions, and the like.

How to write Clean code?

Meaningful Names

Use intention revealing names. Choosing good names takes time but saves more than it takes.The name of a variable, function, or class, should answer all the big questions. It should tell you why it exists, what it does, and how it is used. If a name requires a comment, then the name does not reveal its intent.

Eg- int d; // elapsed time in days.

We should choose a name that specifies what is being measured and the unit of that measurement.

A better name would be:- int elapsedTime. (Even though the book says elapsedTimeInDays, I would still prefer the former one. Suppose the elapsed time is changed to milliseconds. Then we would have to change long to int and elapsedTimeInMillis instead of elapsedTimeInDays. And for how long we’ll keep changing both the data type and name.)

Class Names — Classes and objects should have noun or noun phrase names like Customer, WikiPage, Account, and AddressParser. Avoid words like Manager, Processor, Data, or Info in the name of a class. A class name should not be a verb.

Method Names —Methods should have verb or verb phrase names like postPayment, deletePage, or save. Accessors, mutators, and predicates should be named for their value and prefixed with get, set.

When constructors are overloaded, use static factory methods with names that describe the arguments. For example,

Complex fulcrumPoint = Complex.FromRealNumber(23.0); is generally better than Complex fulcrumPoint = new Complex(23.0);

Pick One Word per Concept —Pick one word for one abstract concept and stick with it. For instance, it’s confusing to have fetch, retrieve, and get as equivalent methods of different classes. How do you remember which method name goes with which class? Likewise, it’s confusing to have a controller and a manager and a driver in the same code base. What is the essential difference between a DeviceManager and a Protocol- Controller?

Functions


The first rule of functions is that they should be small. The second rule of functions is that they should be smaller than that. This implies that the blocks within if statements, else statements, while statements, and so on should be one line long. Probably that line should be a function call. Not only does this keep the enclosing function small, but it also adds documentary value because the function called within the block can have a nicely descriptive name.

Function arguments

A function shouldn’t have more than 3 arguments. Keep it as low as possible. When a function seems to need more than two or three arguments, it is likely that some of those arguments ought to be wrapped into a class of their own. Reducing the number of arguments by creating objects out of them may seem like cheating, but it’s not.

Now when I say to reduce a function size, you would definitely think how to reduce try-catch as it already makes your code so much bigger. My answer is make a function containing just the try-catch-finally statements. And separate the bodies of try/catch/finally block in a separate functions. Eg-

This makes the logic crystal clear. Function names easily describe what we are trying to achieve. Error handling can be ignored. This provides a nice separation that makes the code easier to understand and modify.

Error Handling is one thing — Function should do one thing. Error handling is one another thing. If a function has try keyword then it should be the very first keyword and there should be nothing after the catch/finally blocks.

Comments

If you are writing comments to prove your point, you are doing a blunder. Ideally, comments are not required at all. If your code needs commenting, you are doing something wrong. Our code should explain everything. Modern programming languages are english like through which we can easily explain our point. Correct naming can prevent comments.

Legal comments are not considered here. They are necessary to write. Legal comments means copyright and licenses statements.

Objects and Data Structures

This is a complex topic so pay good attention to it. First we need to clarify the difference between object and Data Structures.

Objects hide their data behind abstractions and expose functions that operate on that data. Data structure expose their data and have no meaningful functions.

These 2 things are completely different. One is just about storing data and other is how to manipulate that data. Consider, for example, the procedural shape example below. The Geometry class operates on the three shape classes. The shape classes are simple data structures without any behavior. All the behavior is in the Geometry class.

Consider what would happen if a perimeter() function were added to Geometry. The shape classes would be unaffected! Any other classes that depended upon the shapes would also be unaffected! On the other hand, if I add a new shape, I must change all the functions in Geometry to deal with it. Again, read that over. Notice that the two conditions are diametrically opposed.

Now consider another approach for the above scenario.

Now we can easily add new Shapes i.e. data structures as compared to previous case. And if we have to add perimeter() function in only one Shape, we are forced to implement that function in all the Shapes as Shape class is an interface containing area() and perimeter() function. This means:

Data structures makes it easy to add new functions without changing the existing data structures. OO code(using objects), makes it easy to add new classes without changing existing functions.

The complimentary is also true:

Procedural code(using data structures) makes it hard to add new data structures because all the functions must change. OO code makes it hard to add new functions because all the classes must change.

So, the things that are hard for OO are easy for procedures, and the things that are hard for procedures are easy for OO!

In any complex system there are going to be times when we want to add new data types rather than new functions. For these cases objects and OO are most appropriate. On the other hand, there will also be times when we’ll want to add new functions as opposed to data types. In that case procedural code and data structures will be more appropriate.

Mature programmers know that the idea that everything is an object is a myth. Sometimes you really do want simple data structures with procedures operating on them. So you have to carefully think what to implement also thinking about the future perspective that what will be easy to update. As for in this example, as any new shape can be added in the future, I will pick OO approach for it.

I understand it’s hard to write good programs given the timeline in which you have to do your tasks. But till how long you’ll delay? Start slow and be consistent. Your code can do wonders for yourself and mostly for others. I’ve started and found so many mistakes I’ve been doing all the time. Though it has taken some extra hours of my daily time limit, it will pay me in the future.

This is not an end to this blog. I will continue to write about new ways to clean your code. Moreover, I will also write about some basic design patterns which are must know for every developer in any technology.

In the mean time, if you like my blog and learnt from it, please applause. It gives me motivation to create a new blog faster :) Comments/Suggestions are welcomed as always. Keep learning and keep sharing.
## Stop writing Comments
There is usually a high correlation between bad code and code with a lot of comments. This is the most obvious sign of messy source code.

The goal of every programmer should be to write code so clean and expressive that code comments are unnecessary. The purpose of every variable, function and class should be implicit in its name and structure. When you need to write a comment, it usually means that you have failed to write code that was expressive enough. You should feel a sharp pain in your stomach every time you write a comment.

When someone else reads your code, they should not have to read the comments to understand what your code is doing. Well named classes and functions should guide the reader through your code like a well-written novel. When the reader looks at a new class or function they should not be surprised by what they see inside. Remember, very little of a developer’s work time is actually spent writing code, much more time is spent on reading code and understanding what it does.

Comments Cover Up Failures

I often see comments above variable or function names describing what the code does (or is supposed to do). These comments make it clear that the programmer was not able to think of an expressive enough name or that their function is doing more than one thing.

Naming things in your code is extremely important. You should put a lot of effort into naming every piece of code accurately and precisely so that other developers can understand your code.

In this example, the name find was not descriptive enough, so the author of this function needed to leave behind a descriptive comment describing what the function does. When we see the find function called from another module, it is a mystery what it does. What is it finding? What exactly does finding mean? Is it returning what it finds? How is it finding whatever it finds? Like Uncle Bob says in his book Clean Code, if you need to write a comment you have failed to express yourself through your code.

We don’t want to have to examine the comment above each function to understand what it does.

Now it is obvious what this function does just by reading its signature, which makes the comment redundant. This brings me to the next way comments are failures.

Redundant Comments

These clutter up your code and are completely unnecessary. Adding many redundant comments trains the reader to skip over every comment, so when there is a comment that is important it will likely not be read.

The last example is mandated redundancy. Many organizations require this above every function and class. If your boss requires this, ask them not to.

Wrong Level of Abstraction

If you have a long function or need to document which part of your code does what, then you might be violating these rules:

Functions should do one thing.
Functions should be small.
Here is an example

If you have pieces of code that can be separated into their own functions, then refactor it. When you have successfully encapsulated each portion of logic into a separate function, the code should read like a description of what it does.

Refactored we have this:

Instead of commenting each part of your code, each chunk of logic should be nicely encapsulated in its own function.

First, this improves readability. Each chunk of code does not have to be read line by line. We can instead simply read the helper function name and understand what it does. If we want to know further details inside each function, we can always see the implementation.

Secondly, it improves testability. In our example above, we can make a separate test for each function. Without encapsulating these separate functions it is difficult to test each part of the larger function sendPromotionEmailToUsers(). Functions that do more than one thing are hard to test.

Lastly, it improves refactorability. By encapsulating each part of the logic into its own function, future changes will be easier to do and will be isolated to changing the behavior of that function only. When we have long functions with local variables that persist throughout the entirety of the function, it is hard to refactor the function without causing changes somewhere else because of how tightly coupled the function is.

Commented Out Code

Commented out code should be treated like roadkill. Don’t look at it, don’t smell it, don’t ask where it came from, just get rid of it. The longer you keep it there, the longer it makes the rest of the code smell.

If you try to uncomment the code, who knows if it will even compile? Will that section of code invalidate something else in the code? Just delete it. If you need it later, you can always check your Version Control System, because you are using a VCS, right?

TODO Comments

Don’t write TODO comments, instead maybe just… do it? Most of the time these comments get forgotten about and later on might become irrelevant or wrong. When another coder sees the TODO comment later on, how do they know if this still needs to be done? Delete these comments too.

The only time a TODO comment is ok is if you are waiting for a merge from another teammate. This is not meant to last a long time, just until you can make the fix and submit it.

“When you feel the need to write a comment, first try to refactor the code so that any comment becomes superfluous.” — Martin Fowler

Comments Lie

Another problem with comments is that they lie to us.

When Jimmy makes a comment above the new function he wrote, he thinks he is helping out any future developer that sees his code. What he is really doing is setting a trap. His comment may lie (no pun intended) dormant for months or years not being touched, just waiting to become a nasty lie. Then one day during one of the hundreds of refactors and requirement changes, his comment becomes invalidated from some far away module.

When you change a line of code, how do you know you didn’t just invalidate a comment somewhere else in the code? There is no way to know. Comments must rot.

Then later on a developer wants to split name into firstName and lastName.

Bam! The comment is now wrong. You could update the comment to reflect the changes, but do you really want to manually maintain all of your comments after each change? You are a developer, not a documentor.

But this comment is easy to notice and is no problem to change.

Let’s see another example:

Later on someone changes the function findEmployees so that it finds employees by a list of names instead of a list of statuses.

First, the comment above findEmployees has been invalidated, so that needs to be changed. No problem, right? Wrong.

The comment above processEmployees has also been invalidated, so that needs to be changed too. How many other comments were just invalidated by this small refactoring? How many lies were created in the source code by this one change?

Alternative Solution:

If you name your functions accurately and precisely, comments will not be needed, and you won’t spread lies in your code.

“Code never lies, comments sometimes do.” — Ron Jeffries

When Comments Are Ok

I know many developers are diehard supporters of code comments, and to them I must admit that sometimes comments are ok. You should still feel squeamish every time you write one, but sometimes they are a necessary evil.

Complex Expressions

If you have a complicated SQL or regex statement, then go ahead and write a comment. It can be difficult to express statements such as these cleanly and expressively in your code. Writing a comment above these expressions will help your fellow developers out greatly in their understanding of your code.

Warnings

If you need to warn other developers of side effects or bad things that will happen, it is ok to leave a comment near this code. These comments can act as harbingers of mysterious behavior in your code and add value to your code.

Clarification of Intent

If you can’t write expressive code, own up to your failure and write a comment. You are responsible for the code you write, so never leave poorly written code unexplained.

If you must write a comment, make sure it is local. A non-local comment placed far away from what it is referencing is destined to rot and become a lie. A comment referencing a function or variable should be directly above it. A warning comment can be above or beside the code it is referencing. If your IDE supports comment highlighting, make your warning comments stand out from the rest of the code.

I have established my feelings about code comments. I despise them, but I recognize that sometimes they are needed.

So please, stop writing so many comments.
___
References:
https://medium.com/mindorks/how-to-write-clean-code-lessons-learnt-from-the-clean-code-robert-c-martin-9ffc7aef870c
https://medium.com/@bpnorlander/stop-writing-code-comments-28fef5272752
Created: 2022-09-08 19:21
## Structuring your code
- use **Abstraction**: Hide details of internal workings