# Subroutines, Scoping, and Sourcing

No procedural programming language would be complete without some notion of subroutines, functions, or other such constructs. The Bourne shell is no exception.

In the Bourne shell, there are two basic ways to approach subroutines. The first is through executing outside tools (which may include a script executing itself recursively). This was described briefly in [Basic Control Statements](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/FlowControl,Expansion,andParsing/FlowControl,Expansion,andParsing.html#//apple_ref/doc/uid/TP40004268-CH4-SW2). However, there are other techniques for obtaining result code information from external scripts. These are described in [Working with Result Codes](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/ResultCodes,Chaining,andArgumentParsing/ResultCodes,Chaining,andArgumentParsing.html#//apple_ref/doc/uid/TP40004268-CH5-SW3). You can also make execution of one command be conditional upon the result code returned by another command as described in [Chaining Execution](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/ResultCodes,Chaining,andArgumentParsing/ResultCodes,Chaining,andArgumentParsing.html#//apple_ref/doc/uid/TP40004268-CH5-SW7).

The second way to approach subroutines (and one which generally results in better performance) is through the use of actual subroutines. These are described in [Subroutine Basics](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/SubroutinesandScoping/SubroutinesandScoping.html#//apple_ref/doc/uid/TP40004268-TP40003513-SW4). You can also write short, simple subroutines inline as described in [Anonymous Subroutines](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/SubroutinesandScoping/SubroutinesandScoping.html#//apple_ref/doc/uid/TP40004268-TP40003513-SW6).

The scoping rules for shell subroutines differ from the scoping rules for most other programming languages. Shell script variable scoping is explained in [Variable Scoping](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/SubroutinesandScoping/SubroutinesandScoping.html#//apple_ref/doc/uid/TP40004268-TP40003513-SW2).

You may find it useful to include one entire shell script inside another. This subject is covered in [Including One Shell Script Inside Another (Sourcing)](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/SubroutinesandScoping/SubroutinesandScoping.html#//apple_ref/doc/uid/TP40004268-TP40003513-SW5).

Finally, you may find it useful to execute outside scripts in the background and check their status at a later time. You can learn about this in [Background Jobs and Job Control](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/AdvancedTechniques/AdvancedTechniques.html#//apple_ref/doc/uid/TP40004268-TP40003521-SW61).

## Subroutine Basics

Subroutines in the Bourne shell look very much like C functions without the argument list. You call these subroutines just like you run a program, and subroutines can be used anywhere that you can use an executable.

Here is a simple example that prints "Arg 1: This is an arg" using a shell subroutine:

|   |
|---|
|#!/bin/sh|
||
|mysub()|
|{|
|echo "Arg 1: $1"|
|}|
||
|mysub "This is an arg"|

Just as shell script arguments are stored in shell variables named $1, $2, and so on, so too are the arguments to shell subroutines. In fact, in most ways, shell subroutines behave exactly like executing an external script. One place where they behave differently is in variable scoping. See [Variable Scoping](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/SubroutinesandScoping/SubroutinesandScoping.html#//apple_ref/doc/uid/TP40004268-TP40003513-SW2) for more information.

In general, a subroutine can do anything that a shell script can do. It can even return an exit status to the calling part of the shell script. For example:

|   |
|---|
|#!/bin/sh|
||
|mysub()|
|{|
|return 3|
|}|
||
|mysub "This is an arg"|
|echo "Subroutine returned $?"|

**Note:** Be careful _not_ to use `exit` in the subroutine. If you do, the entire script will exit, not just the subroutine. This is one way in which subroutines behave differently than separate scripts behave.

**C Shell Note:** The C shell does not support subroutines. You can, however, use additional external scripts to simulate them. For very simple subroutines, you can also approximate the functionality with aliases as described in [The alias Builtin](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/BeforeYouBegin/BeforeYouBegin.html#//apple_ref/doc/uid/TP40004268-CH1-SW9).

## Anonymous Subroutines

The Bourne shell allows you to group more than one command together and treat them both as a separate command. In effect, you are creating an anonymous subroutine inline.

For example, if you want to copy a large number of files from one place to another, you could use `cp`, but this may not be semantically ideal for any number of reasons. Another option is to use `tar` to create an archive on standard output, then pipe that to a second instance of `tar` that extracts the archive.

The basic commands needed are show below. The first command in this example archives the listed files and prints the archive contents to standard output. The second command takes an archive form standard output and extracts the files.

|   |
|---|
|tar -cf - file1 file2 file3 ...|
|tar -xf -|

Thus, to copy files from one place to another, you could pipe the first `tar` command to the second one. However, there’s a problem with that: because the second `tar` is running in the same directory, you are extracting the files on top of themselves. If you’re lucky, nothing happens at all. In the worst case scenario, you could lose files this way.

Thus, you need run two commands on the right side of the pipe: a `cd` command to change directories before extracting the archive and the `tar` command itself. You can do this with an anonymous subroutine.

Here is a simple example:

|   |
|---|
|tar -cf - file1 file2 file3 \| \|
|{ cd "/destination" ; tar -xf - ; }|

Notice the semicolon before the close curly brace. This semicolon is required. Also notice the space after the opening curly brace. This space is also required. Forgetting either of these results in a syntax error.

Of course, as written, there is still some risk involved in using this code. If the destination directory does not exist, the `cd` command fails, and the `tar` command executes in the wrong directory. To solve this problem, you should check the exit status of the first command before running the second one.

For example:

|   |
|---|
|tar -cf - file1 file2 file3 \| \|
|{ if cd "/destination" ; then tar -xf - ; fi; }|

This version will execute the `cd` command, then execute the second `tar` command _only_ if the `cd` command was successful.

**C Shell Note:** The C shell does not support anonymous subroutines. You can, however, use additional external scripts to simulate them. You can also roughly approximate this functionality through careful use of chaining as described in [Chaining Execution](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/ResultCodes,Chaining,andArgumentParsing/ResultCodes,Chaining,andArgumentParsing.html#//apple_ref/doc/uid/TP40004268-CH5-SW7). For example:

|   |
|---|
|( cd / && ls ) \| more|

Unfortunately, if you need the second command to execute even if the first command fails, you can quickly end up with very unreadable code.

|   |
|---|
|((ls /boguslocation \| true) && (ls \| true)) \| more|

## Variable Scoping

Subroutines execute within the same shell instance as the main shell script. As a result, all shell variables are, by default, shared between the subroutines and the main program body. This creates a bit of a problem when writing recursive code.

Fortunately, variables do not have to remain global.

### Declaring a Local Variable

To declare a variable local to a given subroutine, use the `local` statement.

|   |
|---|
|#!/bin/sh|
||
|mysub()|
|{|
|local MYVAR|
|MYVAR=3|
|echo "SUBROUTINE: MYVAR IS $MYVAR";|
|}|
||
|MYVAR=4|
|echo "MYVAR INITIALLY $MYVAR"|
|mysub "This is an arg"|
|echo "MYVAR STILL $MYVAR"|

This script will tell you that the initial value is 4, the value was changed to 3 in the subroutine, and remains 4 when the subroutine returns. Were it not for a `local` declaration of `MYVAR` in the subroutine, the subsequent change to `MYVAR` would have propagated back to the main body of the script.

Much like the `export` statement, the `local` statement can be used at the beginning of an assignment statement as well. For example, the previous subroutine could have contained the following line instead:

|   |
|---|
|local MYVAR=3|

In either case, any subsequent changes to the variable `MYVAR` remain local to this subroutine.

If this subroutine calls itself recursively, a new copy of `MYVAR` is created for each call to this subroutine, resulting in a call stack much like local variables in C or other languages.

Unlike most other languages, however, if this subroutine calls other subroutines, the local copy of `MYVAR` is also used by those other subroutines (unless they also declare a local copy of `MYVAR`). In effect, it is as though the global variable `MYVAR` were replaced with a new global variable that gets destroyed and replaced with the original when the subroutine returns.

**Important:** Changes to this variable in subroutines that do not have a `local` declaration of `MYVAR` will still result in modifications to the global copy of `MYVAR` except when those subroutines are called from this one.

### Using Global Variables in Subroutines

In general, you can freely read and modify global variables within any subroutine. However, there are two situations in which this is not the case:

- Changes to variables previously declared as `local` in the current call stack. This is described further in [Declaring a Local Variable](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/SubroutinesandScoping/SubroutinesandScoping.html#//apple_ref/doc/uid/TP40004268-TP40003513-SW23).
    
- Changes made in subroutines called through inline execution.
    

If you call a subroutine using inline execution, that subroutine gets a local copy of all shell variables. Changes made to those variables are not propagated back into the main script context because the subroutine gets executed in a separate shell.

The following script demonstrates these concepts:

|   |
|---|
|#!/bin/sh|
||
|# Demonstrates scoping rules.|
||
|changevalue()|
|{|
|NAME="$1"|
||
|eval "$NAME=\"\$(expr \"\$$NAME\" \"+\" \"1\")\""|
|eval echo "\$$NAME"|
|}|
||
|localchange()|
|{|
|local X=17|
||
|printf "Local variable X: $X + 1 is: "|
|changevalue X|
|echo "which is also $X"|
|}|
||
|A=3|
|printf "$A + 1 is "|
|changevalue A|
|echo "which is also $A"|
||
|B=3|
|printf "$B + 1 is "|
|RESULT="$(changevalue B)"|
|echo $RESULT|
|echo "which is NOT $B"|
||
|localchange|
|echo "X in a global context is \"$X\""|

**Note:** The use of `eval` is explained in [Using the eval Builtin for Data Structures, Arrays, and Indirection](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/AdvancedTechniques/AdvancedTechniques.html#//apple_ref/doc/uid/TP40004268-TP40003521-SW1).

Notice that when `changevalue` is called directly, the changes it makes to global variables are propagated back to the main script body. When it is called using inline execution, the changes are lost.

This can cause problems for any subroutine that returns a string and also has side effects. There are two straightforward design patterns that can be used to solve this:

- The subroutine could store its output string in a variable instead of printing it. The caller would then use that variable instead of using inline execution to capture the subroutine’s output in a variable.
    
    If desired, one argument to the subroutine could be the name of the variable to use. By designing it in this way, the caller can specify a variable that is local to the calling subroutine, thus avoiding global namespace pollution.
    
- The caller can redirect the subroutine’s output to a file and subsequently use inline execution with the `cat` command to copy the subroutine’s output into a variable.
    

Both methods are functionally equivalent.

## Including One Shell Script Inside Another (Sourcing)

As with any programming language that includes subroutines, it is often useful to build up a library of common subroutines that your scripts can use. To avoid duplicating this content, the Bourne shell scripting language provides a mechanism to include one shell script inside another by reference. This process is commonly referred to as sourcing.

To source one script from another, you use the `.` builtin.

For example, create a file containing the subroutine `mysub` from [Variable Scoping](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/SubroutinesandScoping/SubroutinesandScoping.html#//apple_ref/doc/uid/TP40004268-TP40003513-SW2). Call it `mysub.sh`. To use this subroutine in another script, you can do the following:

|   |
|---|
|#!/bin/sh|
|MYVAR=4|
||
|# The next line sources the external script.|
|. /path/to/mysub.sh|
||
|echo "MYVAR INITIALLY $MYVAR"|
|mysub "This is an arg"|
|echo "MYVAR STILL $MYVAR"|

This script does exactly the same thing as the script in the previous section. The only difference is that the subroutine used is in a different file.

In addition to using the period (`.`) character, many shells provide a `source` builtin that does the same thing. For example:

|   |
|---|
|# This form is less compatible.|
|source /path/to/mysub.sh|

The `source` builtin is more popular among former C shell programmers, while the period (`.`) version is more popular among Bourne shell purists. The period version is considered portable.

**Compatibility Note:** The `source` builtin is a BASH extension that is also supported by ZSH. Other Bourne shell variants do not support this builtin. For maximum portability, you should always use the period (`.`) builtin instead.

These examples are not as straightforward as they seem, however. While this works very well for including subroutines, you cannot always use this in place of executing an outside script, as execution and sourcing behave very differently with respect to variables. The following example demonstrates this:

|   |
|---|
|#!/bin/sh|
|# Save as sourcetest1.sh|
|MYVAR=3|
|. sourcetest2.sh|
|echo "MYVAR IS $MYVAR"|

|   |
|---|
|#!/bin/sh|
|# Save as sourcetest2.sh|
|MYVAR=4|

You will notice that the second script changed the value of a variable that was local to the first script. Unlike executing a script as a normal shell command, executing a script with the `source` builtin results in the second script executing within the same overall context as the first script. Any variables that are modified by the second script will be seen by the calling script. While this can be very powerful, it is easy to clobber variables if you aren't careful.

**C Shell Note:** The C shell supports the `source` builtin, but does not support the period form (`.`).

### Finding the Absolute Path of the Current Script

Occasionally, you may write a script that needs to execute itself or needs to source a subroutine library in the same directory. When you do, it can be useful to obtain the absolute path of the script itself.

The shell variable `$0` contains the name passed in on the command line. If the script was executed with an absolute path, this is all you need. However, if the script is in a directory contained in the `PATH` environment variable, this may contain nothing more than the name of the script.

To obtain the actual path of the script, you must take advantage of the shell’s ability to search through the locations in the `PATH` variable. The following snippet returns the path of the executing script. This path may be relative to the current working directory.

|   |
|---|
|SCRIPT="$(which $0)"|

Your script can then execute itself like this:

|   |
|---|
|"$SCRIPT" arguments go here|

You can get a complete absolute path by adding a few more lines:

|   |
|---|
|SCRIPT="$(which $0)"|
|if [ "x$(echo $SCRIPT \| grep '^\/')" = "x" ] ; then|
|SCRIPT="$PWD/$SCRIPT"|
|fi|

If the path starts with a leading slash (`/`), it is already an absolute path, so you don’t need to do anything to it. If it does not, prepending the current working directory turns it into one.

**Note:** This result is not a minimized absolute path; it may contain references to the current (`.`) or enclosing (`..`) directories. It is, however, an absolute path that is will not break even if your script changes directories or modifies its `PATH` environment variable.

[Next](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/PaintbyNumbers/PaintbyNumbers.html)[Previous](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/ResultCodes,Chaining,andArgumentParsing/ResultCodes,Chaining,andArgumentParsing.html)
