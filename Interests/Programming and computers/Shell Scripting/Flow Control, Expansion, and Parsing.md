# Flow Control, Expansion, and Parsing

The topics of flow control, expansion, and parsing may seem somewhat disparate, but they are closely related in the context of Bourne shell scripts.

In particular, because of the token splitting rules, parsing and expansion are most likely to make a behavioral difference in the context of control statements (`if`, `while`, and so on).

Similarly, to fully understand variable expansion, you must understand how it interacts with parsing, including when the contents of variables undergo further token splitting.

Because of the complex relationship between these topics, they are described together in a single chapter.

## Basic Control Statements

The examples in previous chapters have been very basic, linear programs. This section shows how to add flow control statements that allow for more complex programs.

### The if Statement

The first control statement you should be aware of in shell scripting is the `if` statement. This statement behaves very much like the `if` statement in other programming languages, with a few subtle distinctions.

The first distinction is that the test performed by the `if` statement is actually the execution of a command. When the shell encounters an `if` statement, it executes the statement that immediately follows it. Depending on the return value, it will execute whatever follows the `then` statement. Otherwise, it will execute whatever follows the else statement.

The second distinction is that in shell scripts, many things that look like language keywords are actually programs. For example, the following code executes `/bin/true` and `/bin/false`.

|   |
|---|
|# always execute|
|if true; then|
|ls|
|else|
|echo "true is false."|
|fi|
|# never execute|
|if false; then|
|ls|
|fi|

In both of these cases, an executable is being run—specifically, `/bin/true` and `/bin/false`. Any executable could be used here.

A return of zero (0) is considered to be true (success), and any other value is considered to be false (failure). Thus, if the executable returns zero (0), the commands following the `then` statement will be executed. Otherwise, the statements following the `else` clause (if one exists) will be executed.

The reason for this seemingly backwards definition of `true` and `false` is that most UNIX tools exit with an exit status of zero upon success and a nonzero exit status on failure, with positive numbers usually indicating a user mistake and negative numbers usually indicating a more serious failure of some sort. Thus, you can easily test to see if a program completed successfully by seeing if the exit status is the same as that of `true`.

One related statement that you should be familiar with is `elif`. This statement is similar to saying `else if` except that it does not require an additional `fi` at the end of the conditional, and thus results in more readable code.

For example:

|   |
|---|
|`#/bin/sh`|
||
|read A|
|if [ "$A" = "foo" ] ; then|
|echo "Foo"|
|elif [ "$A" = "bar" ] ; then|
|echo "Bar"|
|else|
|echo "Other"|
|fi|

This example reads a string from standard input and prints one of three things, depending on whether you typed “foo”, “bar’, or anything else. (The bracket syntax used in this example is explained in the next section, [The test Command and Bracket Notation](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/FlowControl,Expansion,andParsing/FlowControl,Expansion,andParsing.html#//apple_ref/doc/uid/TP40004268-CH4-SW5).)

**C Shell Note:** The C shell syntax is similar to C. There are two forms:

|   |
|---|
|#!/bin/csh|
||
|set A = "$<"|
||
|if ( "x${A}" == "xfoo" ) echo "Foo (single line)"|
||
|if ( "x${A}" == "xfoo" ) then|
|echo "Foo"|
|else if ( "x${A}" == "xbar" ) then|
|echo "Bar"|
|else|
|echo "Other"|
|endif|

Note that the `echo` or `then` statement _must_ appear on the same line as the `if` statement. If it does not, you get an “empty if” error and the script terminates.

### The test Command and Bracket Notation

While the `if` statement can be used to run any executable, the most common use of the `if` statement is to test whether some condition is true or false, much like you would in a C program or other programming language. For example, the `if` statement is commonly used to see if two strings are equal.

Because the `if` statement runs a command, in order to use the `if` statement in this fashion, you will need a program to run that performs the comparison desired. Fortunately, one is built into the OS: `test`. (For more information about using other commands with the `if` statement, see [Working with Result Codes](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/ResultCodes,Chaining,andArgumentParsing/ResultCodes,Chaining,andArgumentParsing.html#//apple_ref/doc/uid/TP40004268-CH5-SW3).)

The `test` executable is rarely run directly, however. Generally, it is invoked by running `[`, which is just a symbolic link or hard link to `/bin/test`.

**Note:** Although the open bracket is a command, and there is a man page, you will have a hard time getting to it on the command line. Use:

|   |
|---|
|man \\[|

to see it (or just look at the man page for `test`).

In this form, the syntax of an `if` statement more closely resembles other languages. Consider the following example:

|   |
|---|
|#!/bin/sh|
||
|FIRST_ARGUMENT="$1"|
|if [ "$FIRST_ARGUMENT" = "Silly" ] ; then|
|echo "Silly human, scripts are for kiddies."|
|else|
|echo "Hello, world $FIRST_ARGUMENT!"|
|fi|

There are three things you should notice. First, the space before the equals sign is critical. This space is the difference between assignment (no space) and comparison (space). The spaces around the brackets are also critical; failure to include these spaces results in a syntax error. (The open bracket is really just a command, and it expects its last argument to be a close bracket by itself.)

Second, you should notice the use of double quote marks. This serves two purposes. First, it ensures that even if the variable or string is empty, there is a placeholder. This also ensures that the code will function correctly if the variable’s value contains spaces.

If you are looking at older code, you may also see the empty variable problem solved in another way:

|   |
|---|
|if [ x$VARIABLE = x ] ; then|
|echo "Empty variable \$VARIABLE"|
|fi|

In this older style, the two arguments to the comparison are preceded by an ‘x’ (and in this example, on the right side, the ‘x’ precedes nothing, thus comparing the value to an empty string).

The reason this is needed is because variable substitution occurs before this statement is executed. If you omit the ‘x’ on the left side and the value in `$VARIABLE` is empty, then this statement evaluates to “`if [ = x ]`”, which is a blatant syntax error.

This style is not recommended for new code. It does not handle spaces inside variables, and provides a significant attack vector for arbitrary code injection. See [Shell Script Security](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/ShellScriptSecurity/ShellScriptSecurity.html#//apple_ref/doc/uid/TP40004268-CH8-SW1) for more information.

**Note:** This example introduces another special character, the backslash. It is also known as a quote character because the character immediately after it is treated as though it were within quotes. Thus, in this case, the snippet prints the name of the variable (`$VARIABLE`) rather than its contents. The use of backslash (and other similar characters) is described further in [Quoting Special Characters](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/FlowControl,Expansion,andParsing/FlowControl,Expansion,andParsing.html#//apple_ref/doc/uid/TP40004268-CH4-SW28).

The `test` command can also be used for various other tests, including the testing for the existence of a file, basic numerical comparisons, checking whether a path points to a directory, an executable, or a symbolic link, and so on. For example, the `-d` flag checks whether its argument is a directory, as shown in this snippet:

|   |
|---|
|if [ -d "/System/Library/Frameworks" ] ; then|
|echo "/System/Library/Frameworks is a directory."|
|fi|

A complete list of flags and operators supported by the test command can be found in the man page `test`.

**C Shell Note:** While the test command can be used in the C shell, it is somewhat unusual to do so; the `if` and `while` statements in the C shell do not use it as part of their normal syntax.

### The while Statement

In addition to the `if` statement, the Bourne shell also supports a `while` statement. Its syntax is similar.

|   |
|---|
|while true; do|
|ls|
|done|

Like the `if` statement’s `then` and `fi`, the while statement is bracketed by `do` and `done`. Much like the `if` statement, the while statement takes a single argument that contains a command to execute. The loop terminates when this command’s exit status is false (nonzero).

As with the `if` statement, the most common command used to control looping is the bracket command (as described in [The test Command and Bracket Notation](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/FlowControl,Expansion,andParsing/FlowControl,Expansion,andParsing.html#//apple_ref/doc/uid/TP40004268-CH4-SW5)).

For example:

|   |
|---|
|while [ "x$FOO" != "x" ] ; do|
|FOO="$(cat)";|
|done|

Of course, this is a rather silly example. However, it does demonstrate one of the more powerful features in the Bourne shell scripting language: the `$()` operator, which inserts the output of one command into the middle of a statement. In the case above, the `cat` command is executed, and its standard output is stored in the variable `FOO`. This technique is described more in [Inline Execution](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/FlowControl,Expansion,andParsing/FlowControl,Expansion,andParsing.html#//apple_ref/doc/uid/TP40004268-CH4-SW30).

At any time during a loop, you can terminate the loop early with the `break` statement or skip ahead to the next iteration of the loop with the `continue` statement. When working with nested loops, these statements may be followed by an optional numerical argument to alter execution of the enclosing loops.

For example, consider the following statements:

|   |
|---|
|break 2|
|continue 2|

The first statement above (`break 2`) breaks out of not only the top level `while` loop, but also the `while` or `for` loop that contains it. The second statement above (`continue 2`) not only causes the remainder of the current loop to be skipped, but also causes the remainder of the loop that encloses it to be skipped.

**C Shell Note:** The C shell syntax is similar:

|   |
|---|
|set FOO = "x"|
|while (${FOO} != "")|
|set FOO = `cat`|
|end|

Just as in C, the `break` and `continue` statements are also supported for further loop control. However, the C shell does not support breaking or continuing at any nesting level other than the topmost level.

### The for Statement

The most unusual control structure in this chapter is the `for` statement. It can take two very different forms depending on what you want to do.

In a standard Bourne shell, the `for` statement in shell scripts is completely unlike its C equivalent (which requires numerical computation, as described in [Paint by Numbers](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/PaintbyNumbers/PaintbyNumbers.html#//apple_ref/doc/uid/TP40004268-TP40003512-SW1)), and actually behaves much like the `foreach` statement in various languages.

In some modern Bourne shell variants, you can also do a numerical version of a for loop. The syntax is nearly identical to the C syntax for for loops.

The two syntaxes are covered in the following sections.

#### Standard for Loops

The `for` statement in Bourne shell scripts iterates through the items in a list. For each item, it sets the loop variable to the item, then executes a series of statements.

In the next example, the list is `*.JPG`. When the shell performs globbing on this (see [Special Characters Explained](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/FlowControl,Expansion,andParsing/FlowControl,Expansion,andParsing.html#//apple_ref/doc/uid/TP40004268-CH4-SW25) for more information), it replaces the `*.JPG` with a list of files in the current directory that end in `.JPG`.

Without going into details about the regular expression syntax used by the `sed` command (this syntax is described in more detail in [Regular Expressions Unfettered](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/RegularExpressionsUnfettered/RegularExpressionsUnfettered.html#//apple_ref/doc/uid/TP40004268-CH238-SW7)), the following script renames every file in the current directory that ends with `.JPG` to end in `.jpg`.

|   |
|---|
|#!/bin/sh|
|for i in *.JPG ; do|
|mv "$i" "$(echo $i \| sed 's/\.JPG$/.x/')"|
|mv "$(echo $i \| sed 's/\.JPG$/.x/')" "$(echo $i \| sed 's/\.JPG$/.jpg/')"|
|done|

The `for` statement (by default) splits the file list on unquoted spaces. For example, the following script will print the letters “a” and “b” on separate lines, then print “c d” on a third line:

|   |
|---|
|#!/bin/sh|
|for i in a b c\ d ; do|
|echo $i|
|done|

Under certain circumstances, you can change the way that the `for` statement splits lists by changing the contents of the variable `IFS`. The details of when this does and does not work are described in [Variable Expansion and Field Separators](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/FlowControl,Expansion,andParsing/FlowControl,Expansion,andParsing.html#//apple_ref/doc/uid/TP40004268-CH4-SW22).

At any time during a loop, you can terminate the loop early with the `break` statement or skip ahead to the next iteration of the loop with the `continue` statement. When working with nested loops, these statements may be followed by an optional numerical argument to alter execution of the enclosing loops.

For example, consider the following statements:

|   |
|---|
|break 2|
|continue 2|

The first statement above (`break 2`) breaks out of not only the top level `for` loop, but also the `while` or `for` loop that contains it. The second statement above (`continue 2`) not only causes the remainder of the current loop to be skipped, but also causes the remainder of the loop that encloses it to be skipped.

**C Shell Note:** The C shell `foreach` statement is similar.

|   |
|---|
|#!/bin/csh|
||
|foreach i ( *.JPG )|
|mv "${i}" `echo ${i} \| sed 's/\.JPG$/.x/'`|
|mv `echo ${i} \| sed 's/\.JPG$/.x/'` `echo ${i} \| sed 's/\.JPG$/.jpg/'`|
|end|

While the C shell supports the `break` and `continue` statements in a `foreach` loop, it does not support breaking or continuing at any nesting level other than the topmost level.

#### Extended for Loops

Most modern Bourne shells (including BASH) provide an extension for numerical for loops using a variant of the built-in math operator (double parentheses). You can see this style of `for` loop in the following script. It takes a single argument and counts from 1 up to the number specified in that argument. To demonstrate the concept as succinctly as possible, it makes no attempt to validate its input. You, however, should always do so in your scripts.

|   |
|---|
|#!/bin/bash|
||
|# This is an extension that is supported in|
|# bash, zsh, and many other recent sh variants,|
|# but is not always valid.|
|#|
|# Usage: for5.sh <number>|
||
|for (( i = 1 ; i <= $1 ; i++ )) ; do|
|echo "I is $i"|
|done|

For maximum portability, however, you should use a `while` loop, as shown below:

|   |
|---|
|i=1|
|while [ $i -le $1 ] ; do|
|echo "I is $i"|
|i=`expr $i '+' 1`|
|done|

### The case statement

The final control statement in this chapter is the `case` statement. The `case` statement in shell scripts is similar to the C `switch` statement. It allows you to execute multiple commands depending on the value of a variable. The syntax is as follows:

|   |
|---|
|case expression in|
|[(] value \| value \| value \| ... ) command; command; ... ;;|
|[(] value \| value \| value \| ... ) command; command; ... ;;|
|...|
|esac|

You should notice three things about this syntax. First, each case is terminated by a double semicolon. Second, the opening parenthesis is optional and is frequently dropped by script authors. Third, a single set of commands can be applied to any number of values separated by the pipe (vertical bar) character (`|`).

For example, the following code sample prints the English names for the numbers 0–9, then prints them again.

|   |
|---|
|#!/bin/sh|
||
|LOOP=0|
||
|while [ $LOOP -lt 20 ] ; do|
|# The next line is explained in the|
|# math chapter.|
|VAL=`expr $LOOP % 10`|
||
|case "$VAL" in|
|( 0 ) echo "ZERO" ;;|
|( 1 ) echo "ONE" ;;|
|( 2 ) echo "TWO" ;;|
|( 3 ) echo "THREE" ;;|
|( 4 ) echo "FOUR" ;;|
|( 5 ) echo "FIVE" ;;|
|( 6 ) echo "SIX" ;;|
|( 7 ) echo "SEVEN" ;;|
|( 8 ) echo "EIGHT" ;;|
|( 9 ) echo "NINE" ;;|
|( * ) echo "This shouldn't happen." ;;|
|esac|
||
|# The next line is explained in the|
|# math chapter.|
|LOOP=$((LOOP + 1))|
|done|

You should notice the `( * )` case at the end. It is equivalent to the `default` case in C. While that case will never be reached in this example, if you change the value of the modulo from 10 to any larger value, you will see that this case executes when no previous case matches the value of the expression.

**C Shell Note:** The C shell `switch` statement is functionally equivalent, but behaves somewhat differently.

Like in C, each `case` statement falls through into the following case statement until the shell encounters a `breaksw` statement, which causes execution to immediately jump out of the entire `switch` statement.

|   |
|---|
|#!/bin/csh|
||
|set LOOP = 0|
||
|while ( ${LOOP} <= 20 )|
|set VAL = `expr ${LOOP} % 10`|
|switch (${VAL})|
|case 0:|
|echo "ZERO" ; breaksw|
|case 1:|
|echo "ONE" ; breaksw|
|case 2:|
|echo "TWO" ; breaksw|
|case 3:|
|echo "THREE" ; breaksw|
|case 4:|
|echo "FOUR" ; breaksw|
|case 5:|
|echo "FIVE" ; breaksw|
|case 6:|
|echo "SIX" ; breaksw|
|case 7:|
|echo "SEVEN" ; breaksw|
|case 8:|
|echo "EIGHT" ; breaksw|
|case 9:|
|echo "NINE" ; breaksw|
|default:|
|echo "This shouldn't happen."|
|endsw|
||
|set LOOP = `expr ${LOOP} + 1`|
||
|end|

### The expr Command

No discussion of tests and comparisons would be complete without mentioning the `expr` command. This command can perform various string comparisons and basic integer math. The math portions of the `expr` command are described in [The expr Command Also Does Math](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/PaintbyNumbers/PaintbyNumbers.html#//apple_ref/doc/uid/TP40004268-TP40003512-SW2).

The `expr` command is fairly straightforward. Each expression or token passed to the command must be surrounded by quotes if it may contain multiple words or characters that the shell considers special. For example, to compare two strings alphabetically, you could use the following command:

|   |
|---|
|expr "This is a test" '<' "I am a person"|

The following version fails miserably because the shell interprets the less-than sign as a redirect and tries to read from a file called “I am a person”:

|   |
|---|
|expr "This is a test" < "I am a person"|

The details of quoting are described further in [Parsing, Variable Expansion, and Quoting](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/FlowControl,Expansion,andParsing/FlowControl,Expansion,andParsing.html#//apple_ref/doc/uid/TP40004268-CH4-SW20).

**Note:** Be careful when using the `expr` command. Any expression that generates a numerical value (including the string comparison in the previous example) effectively generates two seemingly contradictory results. It returns one value through its exit status and a different numerical value by way of its standard output.

The exit status is zero if a logical expression evaluates to true and one if the expression evaluates to false. The output printed to standard output is one if a logical expression evaluates to true and zero if the expression evaluates to false. Notice that these values are reversed. Be sure to use the exit status when comparing the result to the output of commands like `true`, not the value printed to standard output.

This disparity is only really confusing for computations that return a logical true or false value, of course. The behavior can be explained fairly simply: the `expr` command returns a “success” exit status, zero, if the command prints a value other than zero or an empty string. If it prints a zero or an empty string, its exit status is one (failure).

The `expr` command supports the usual complement of string comparisons (equality, inequality, less-than, greater-than, less-than-or-equal, and greater-than-or-equal).

In addition to these comparisons, the `expr` command can do several other tests: a logical “or” operator, a logical “and” operator, and a (fairly limited) basic regular expression matching operator.

While normally used for logic purposes, you can use the “or” operator to substitute a default string using the or operator like this:

|   |
|---|
|#!/bin/sh|
||
|NAME=`expr "$1" '\|' "Untitled"`|
|echo "The chosen name was $NAME"|

The “or” operator (|) prints the value of the first expression (`"$1"` in this example) if it is nonempty and contains something other than the number zero (0). Otherwise, if the second string is nonempty and contains something other than the number zero, it prints the second expression (`"Untitled"` in this example). If both strings are empty or zero, it prints the number zero. The exit status of the command is zero on success, one if both strings are empty or zero.

**Note:** Because the `expr` command does not distinguish between the number zero (0) and an empty string, you should not use `expr` to test for an empty string if there is a possibility that the string might be `“0”`.

The “and” operator (`&`) is similar, returning either the first string (if both strings are nonempty) or zero (if either string is empty).

Finally, the `expr` command can work with basic regular expressions (_not_ extended regular expressions) to a limited degree.

To count the number of characters from the beginning of the string (all expressions are implicitly anchored to the start of the string) up to and including the last letter ‘i’, you could write an expression like this:

|   |
|---|
|STRING="This is a test"|
|expr "$STRING" : ".*i"|

The string to the right side of the colon is a relatively simple regular expression. The period character matches a single character. The asterisk modifies the behavior of the period so that it matches zero or more characters. (Read [Regular Expressions Unfettered](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/RegularExpressionsUnfettered/RegularExpressionsUnfettered.html#//apple_ref/doc/uid/TP40004268-CH238-SW7) for further explanation.) If the string does not match the expression, the `expr` command returns zero (0), which corresponds with the number of characters matched.

The most common use for this syntax is obtaining the length of a string, as shown in this snippet:

|   |
|---|
|STRING="This is a test"|
|expr "$STRING" : ".*"|

This same syntax can be used to return the text captured by the first set of parentheses in a basic regular expression. For example, to print the four characters immediately prior to the last occurrence of “est”, you could write an expression like this one:

|   |
|---|
|STRING="This is a test" expr "$STRING" : '.*\(....\)est'|

Because this expression contains capturing parentheses, if the first string does not match the expression, the `expr` command prints an empty string.

For more information about writing basic regular expressions, read [Regular Expressions Unfettered](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/RegularExpressionsUnfettered/RegularExpressionsUnfettered.html#//apple_ref/doc/uid/TP40004268-CH238-SW7).

**C Shell Note:** This behaves the same in C shell as it does in the Bourne shell (apart from the usual syntax differences). For example:

|   |
|---|
|#!/bin/csh|
||
|set NAME = `expr "${1}" '\|' "Untitled"`|
||
|echo "The chosen name was ${NAME}"|

## Parsing, Variable Expansion, and Quoting

In both the Bourne shell and the C shell, lines of code are processed in multiple passes. The first pass is a parsing pass in which the basic structure of the line of code is extracted. In this pass, quotation marks serve as delimiters between individual pieces of information. For example, you can print a letter immediately after the contents of a variable without a space by closing (and reopening if necessary) the enclosing double quotes immediately after the variable name.

The second pass is an expansion pass. In this pass, any variable is expanded and any inline execution is performed. If a variable contains special characters, the resulting text is further expanded unless that variable is surrounded by double quotes. This may cause unexpected behavior if, for example, a variable contains a wildcard character.

**Note:** While the expansion of a variable or command inline will not cause a syntax error by itself, it can change the behavior of the `eval` builtin. See [Using the eval Builtin for Data Structures, Arrays, and Indirection](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/AdvancedTechniques/AdvancedTechniques.html#//apple_ref/doc/uid/TP40004268-TP40003521-SW1) for more information.

Finally, the third pass is an execution pass. In this pass, the code is actually executed.

In some cases, you may need to change the way variable expansion takes place. You might want to use a nonstandard character to split a variable containing a list, change the way the shell handles special characters, or execute a command and substitute its output in the middle of another command. These techniques are described in the sections that follow.

### Variable Expansion and Field Separators

In Bourne shell scripts, two operations are affected by the value of the `IFS` (internal field separators) shell variable: the `read` statement and variable expansion. The effect on the `read` statement is described separately in [Shell Script Input and Output Using printf and read](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/ShellInputandOutput/ShellInputandOutput.html#//apple_ref/doc/uid/TP40004268-CH3-SW2).

Whenever the shell expands a variable, the value of `IFS` comes into play. For example, the following script will print “a” and “b” on separate lines, then “c d” on a third line:

|   |
|---|
|#!/bin/sh|
||
|IFS=":"|
|LIST="a:b:c d"|
|for i in $LIST ; do|
|echo $i|
|done|

This occurs _only_ because the value on the right side of the `for` statement contains a variable (`LIST`) that is expanded by the shell. When the shell expands the variable, it replaces the colon with a space and quotes any spaces in the original string. In effect, by the time the `for` statement sees the values, the right side of the for statement contains `a b c\ d`, just as in the example shown in [The for Statement](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/FlowControl,Expansion,andParsing/FlowControl,Expansion,andParsing.html#//apple_ref/doc/uid/TP40004268-CH4-SW10).

If you insert the exact contents of `LIST` on the right side of the variable, this script will instead print “a:b:c” on one line and “d” on the other. This demonstrates why it is very important to choose record separators correctly.

**Cross-Platform Compatibility Note:** This treatment of record separators is consistent in all modern Bourne shell variants (ASH, BASH, DASH, KSH, ZSH, newer versions of the `sh` interpreter, and so on). Some earlier Bourne shell variants use `IFS` when the shell splits a list even if no expansion is involved.

To avoid unexpected behavior, you should avoid setting nonstandard values for `IFS` except when you are expanding a shell variable that depends on this.

As an exception, it is safe to modify `IFS` during a `read` statement. Be sure to save the original value in another variable and restore it afterwards, however, to avoid unexpected behavior elsewhere in the script.

**C Shell Note:** Most versions of csh do not allow you to alter the field separator. If you need more precise control over field separators, you can use the `cut` command in a `while` loop, incrementing a counter.

|   |
|---|
|#!/bin/csh|
||
|set IFS = ":"|
|set LIST = "a:b:c d"|
|set POS = 1|
|set i = `echo "${LIST}" \| cut -f ${POS} -d ':'`|
||
|# Repeat until you get an empty field.  This only works if|
|# you know you should never encounter an empty field.  Otherwise,|
|# you must know the number of fields.|
||
|while ( "x${i}" != "x" )|
|echo $i|
|set POS = `expr ${POS} '+' 1`|
|set i = `echo "${LIST}" \| cut -f ${POS} -d ':'`|
|end|

If you cannot guarantee that there are no empty fields in the list, you must first count the fields and use a counter in your loop test. To learn how to count the fields, see [The expr Command](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/FlowControl,Expansion,andParsing/FlowControl,Expansion,andParsing.html#//apple_ref/doc/uid/TP40004268-CH4-SW16). To learn how to use counters, read [The expr Command Also Does Math](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/PaintbyNumbers/PaintbyNumbers.html#//apple_ref/doc/uid/TP40004268-TP40003512-SW2), substituting the C shell syntax as described in [Shell Variables and Printing](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/shell_scripts/shell_scripts.html#//apple_ref/doc/uid/TP40004268-CH237-SW8) and [Inline Execution](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/FlowControl,Expansion,andParsing/FlowControl,Expansion,andParsing.html#//apple_ref/doc/uid/TP40004268-CH4-SW30) as appropriate.

### Special Characters Explained

There are several special characters in shell scripts: a dollar sign (`$`), an asterisk (`*`), a question mark (`?`), curly braces (`{` and `}`), square brackets (`[` and `]`), parentheses (`(` and `)`), single and double quote marks (`'` and `"`), the backtick mark (`` ` ``, sometimes called the left single quote mark), and the backslash (`\`). These characters are treated differently by the shell.

Most of these special characters are used in filename expansion, also known as _globbing_. Globbing characters obey different expansion rules than other characters.

The characters behave as follows:

- Dollar sign ($)—the first character in variable expansion, shell builtin math, and inline execution. Variable names beginning with a dollar sign are expanded regardless of whether they appear inside double quotes. If used outside of double quotes, any globbing characters within the contents of the variable are also expanded. Variable names within the contents are not expanded, however.
    
- Asterisk (*)—a wildcard character that matches any number of characters in a filename. For example, `ls *.jpg` matches all files that end with the extension `.jpg`. The asterisk is used in globbing.
    
- Question mark (?)—a wildcard character that matches a single character in a filename. For example, `ls a?t.jpg` matches both `ant.jpg` and `art.jpg`. The question mark is used in globbing.
    
- Curly braces—matches any of a series of options in a filename. For example, `ls *.{jpg,gif}` matches every file ending with either `.jpg` or `.gif`. Curly braces are used in globbing.
    
- Square brackets—matches any of a series of characters in a filename. For example, `ls a[rn]t.jpg` matches `art.jpg` and `ant.jpg`, but does not match `aft.jpg`. If the first character is a caret (`^`), it matches every character except for the characters listed.
    
    The syntax of these character classes is similar to character classes in regular expressions, but there are a number of subtle differences. For more information, see the Open Group’s page on pattern matching notation at [http://www.opengroup.org/onlinepubs/009695399/utilities/xcu_chap02.html#tag_02_13](http://www.opengroup.org/onlinepubs/009695399/utilities/xcu_chap02.html#tag_02_13).
    
    Square brackets are used in globbing.
    
- Parentheses—these characters serve multiple purposes, depending on context:
    
    - Used to mark the beginning of a new subroutine. This is described in [Subroutines, Scoping, and Sourcing](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/SubroutinesandScoping/SubroutinesandScoping.html#//apple_ref/doc/uid/TP40004268-TP40003513-SW1).
        
    - Used to group a chain of operations. This is described in [Chaining Execution](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/ResultCodes,Chaining,andArgumentParsing/ResultCodes,Chaining,andArgumentParsing.html#//apple_ref/doc/uid/TP40004268-CH5-SW7).
        
    - Used for math in some Bourne shell variants. This is described in [The Easy Way: Parentheses](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/PaintbyNumbers/PaintbyNumbers.html#//apple_ref/doc/uid/TP40004268-TP40003512-SW5).
        
    - Used in `for` loop iterators supported by some Bourne shell variants. This is described in [Extended for Loops](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/FlowControl,Expansion,andParsing/FlowControl,Expansion,andParsing.html#//apple_ref/doc/uid/TP40004268-CH4-SW13).
        
- Double-quote marks—disables argument splitting on word boundaries (spaces) and shell expansion of most special characters within the quote marks, with a few exceptions:
    
    - Variables _are_ expanded within double quote marks. The contents of variables, however, are _not_ expanded in any way even if they contain globbing characters.
        
    - Inline execution is also expanded within double quote marks.
        
    - The backslash character still functions within double quote marks in the Bourne shell and variants thereof, but not in C shell variants.
        
    
    **Note:** Although globbing-related characters are not generally expanded within double quotes, expansion of globbing characters within strings enclosed in double quotes may still occur if the double quotes are on the right side of a variable assignment and the variable is later used without double quotes. For example:
    
    |   |
    |---|
    |FOO="*.c"   # *.c does not get expanded here|
    |ls $FOO     # *.c DOES get expanded here|
    
- Single-quote marks—disables argument splitting on word boundaries (spaces) and disables _all_ shell expansion (including variables). The backslash is treated just like any other literal character when it appears within single quotes. For example, `'\"'` is a string that contains a backslash and a double quote mark.
    
- Backtick marks—roughly equivalent to `$()`, these are used to delimit code for inline execution. This technique is described in [Inline Execution](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/FlowControl,Expansion,andParsing/FlowControl,Expansion,andParsing.html#//apple_ref/doc/uid/TP40004268-CH4-SW30).
    
- Backslash—causes the next character to be treated as a literal character, overriding the special behaviors explained in this section. This technique is described further in [Quoting Special Characters](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/FlowControl,Expansion,andParsing/FlowControl,Expansion,andParsing.html#//apple_ref/doc/uid/TP40004268-CH4-SW28).
    

If your script accepts user input, these characters can produce unexpected results if you do not quote them properly. Consider the following example:

|   |
|---|
|#!/bin/sh|
|echo "Filename?"|
|read NAME|
|ls $NAME|
|ls "$NAME"|

If a user types `*.jpg` at the prompt, the first command lists all files ending in `.jpg` because the variable is expanded first, and then the expression within it is expanded. The second command lists a single file (or prints an error if you don’t have a file named `*.jpg`).

**C Shell Note:** In Bourne shell variants, globbing occurs anywhere a variable is expanded or a globbing character appears as literal text outside of quotation marks. In the C shell, it is slightly more limited.

Within expressions such as the right half of an `if` statement, the C shell provides two additional operators: the `=~` and `!~` operators. These are similar to string comparison operators, except that the right side is treated using filename globbing rules (for example, foo* matches files named foo, foot, fool, and so on). Although this operator visually resembles the regular expression operator in Perl, this C shell operator does _not_ perform a regular expression comparison.

### Quoting Special Characters

Sometimes, when writing shell scripts, you may need to explicitly include quotation marks, dollar signs, or other special characters in your output. The way that you do this depends on the context.

If the string you wish to quote is not within quote marks, it probably should be. Otherwise, you have to deal with _all_ of the shell special characters (described in [Special Characters Explained](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/FlowControl,Expansion,andParsing/FlowControl,Expansion,andParsing.html#//apple_ref/doc/uid/TP40004268-CH4-SW25)) plus any new special characters that might be added in the future. Protecting against special characters is particularly important if your script takes arbitrary user input and passes it as an argument to a command.

However, if your script is not handling user input, you can quote a single character by simply preceding it with a backslash (`\`). This tells the shell to treat it as a literal character instead of interpreting it normally. For example, the following code sample prints the word “Hello” enclosed in double-quotation marks.

|   |
|---|
|echo \"Hello\"|

If the character you wish to quote is within double quotes, the same rules apply. The only difference is that with the exception of dollar signs and the double-quote marks themselves, you don’t need to quote special characters in this context. For example, to print the name of a variable followed by its value, you could write a statement like the following, which prints “The value of $VAR is 3” (with no quotes):

|   |
|---|
|VAR=3|
|echo "The value of \$VAR is $VAR"|

Similarly, you can quote a backslash with another backslash if you need to print it. For example, the following statement prints “This \ is a backslash.“ (again, without quotes):

|   |
|---|
|echo "This \\ is a backslash."|

If the character you wish to quote is within single quotes, shell expansion of special characters is disabled entirely. Thus, the only characters that are special are the single-quote marks themselves, because they terminate the single-quote context.

Because special character handling is disabled, a backslash does _not_ quote anything between single-quote marks. Instead, a backslash is interpreted as literal text. Thus, to include a literal single quote within a single-quote context, you must terminate the single-quote context, then include the single quote (either by quoting it with a backslash or by surrounding it with double quotes), then start a new single-quote context.

For example, the following lines of code both print a popular phrase from an American children’s television show:

|   |
|---|
|echo 'It'\''s a beautiful day in the neighborhood.'|
|echo 'Won'"'"'t you be my neighbor?'|

**C Shell Note:** The C shell does not support using a backslash to quote a character within a double-quoted string. Thus, in the C shell, you print a backslash like this:

|   |
|---|
|echo "This \ is a backslash."|

To print a literal dollar sign for a variable name, you must either put the dollar sign in single quotes or quote it with a backslash _outside_ of any quote marks. For example:

|   |
|---|
|echo "This is "'$'"FOO"|
|echo "This is "\$"FOO"|

Both statements print the words “This is $FOO”.

Similarly, to print a quotation mark, you must either surround it with the opposite type of quotation mark or quote it outside of quotation marks. For example, the following statement will not work:

|   |
|---|
|echo "This is \"wrong\" and will cause csh to exit with an error"|

This fails because the first backslash is treated as part of the string, which is terminated with the quotation mark immediately after it. Because the third quotation mark is not within a string, however, the backslash quotes it, turning it into a literal character. Thus, it does not start a new string. The fourth quotation mark (at the end of the line) then begins a string. As a result, there is no matching double quote mark to end the string and CSH exits with an unmatched quotation mark error.

Instead, you can use either of the following syntaxes:

|   |
|---|
|echo "You probably meant "\""this"\"" or "'"'" this"'"'"."|

In the first part, the string is terminated with a double quote mark followed by a quoted double quote mark (displayed literally), followed by opening a new string with a double quote mark. In the second part, the string is terminated with a double quote mark, followed by a double quote mark within single quotes, followed by opening a new string with a double quote mark.

The construction of code that takes advantage of this parsing difference to execute different code depending on whether it is executing in a Bourne shell or a C shell is left as an exercise for the reader.

### Inline Execution

The Bourne shell provides two operators for executing a command and placing its output in the middle of another command or string. These operators are the `$()` operator and the backtick (`` ` ``) operator (not to be confused with a normal single quote).

These operators are often used with commands that generate a list of filenames to pass them as the argument list to another command. For example, the `grep` command, when passed the `-l` flag, returns a list of files that match. This technique is often combined with the `-r` flag, which makes grep search recursively for files within any directories that it encounters in its file list. Thus, if you want to edit any files whose contents contain the word "`myname`" with `vi`, for example, you could do it like this:

|   |
|---|
|vi $(grep -rl myname directory_of_files)|

You can, however, use this to execute any command. There is one small caveat you should be aware of, however. The backtick operator cannot be nested. For example, the following command produces an error:

|   |
|---|
|FOO=1; BAR=3|
|echo "Try this command: `echo $FOO + "`expr $BAR + 1`"`"|

This fails because the echo command ends at the second backtick. Thus, the command executed is `echo $FOO + "`. If you need to nest inline execution, you can use the `$()` operator for the nested command. For example, the previous example can be written correctly as follows:

|   |
|---|
|FOO=1; BAR=3|
|echo "Try this command: `echo $FOO + "$(expr $BAR + 1)"`"|

You should notice that double-quotation marks can be safely nested within a command enclosed by either backticks or the `$()` operator.

**Note:** Evaluation of inline commands, much like expansion of variables, occurs _after_ the statement itself is fully parsed. Thus, it is safe to use either the backtick (`` ` ``) or `$()` operator even if the command may produce double-quote marks in its output. You do not need to quote the resulting content in any way.

**C Shell Note:** The C shell only partially supports inline execution:

- The C shell does not support the `$()` syntax.
    
- The C shell support for the backtick syntax is somewhat limited in that newline characters in the result are always stripped and replaced with spaces. If you need to preserve newlines, you should store the results in a temporary file instead of in a shell variable, then operate on the resulting file.
    

[Next](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/ResultCodes,Chaining,andArgumentParsing/ResultCodes,Chaining,andArgumentParsing.html)[Previous](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/ShellInputandOutput/ShellInputandOutput.html)
