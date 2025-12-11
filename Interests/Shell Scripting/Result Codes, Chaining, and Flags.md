# Result Codes, Chaining, and Flags

This chapter covers concepts related to the arguments that scripts take and the results that they return to their caller. It consists of three parts:

- [Working with Result Codes](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/ResultCodes,Chaining,andArgumentParsing/ResultCodes,Chaining,andArgumentParsing.html#//apple_ref/doc/uid/TP40004268-CH5-SW3) explains the numeric result codes that scripts and tools return to the calling scripts or tools. It further explains how scripts can use those values to find out whether a tool succeeded or failed.
    
    For example, the `if` statement and the `test` command work together to control program flow (as described in [Flow Control, Expansion, and Parsing](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/FlowControl,Expansion,andParsing/FlowControl,Expansion,andParsing.html#//apple_ref/doc/uid/TP40004268-CH4-SW1)). This section explains how this interaction works under the hood.
    
- [Chaining Execution](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/ResultCodes,Chaining,andArgumentParsing/ResultCodes,Chaining,andArgumentParsing.html#//apple_ref/doc/uid/TP40004268-CH5-SW7) takes the concept of result codes one step further, demonstrating how you can make a series of commands execute conditionally depending on whether the previous commands succeeded or failed.
    
- [Handling Flags and Arguments](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/ResultCodes,Chaining,andArgumentParsing/ResultCodes,Chaining,andArgumentParsing.html#//apple_ref/doc/uid/TP40004268-CH5-SW63) tells how to write scripts that take complex flags and arguments.
    

## Working with Result Codes

Result codes, also known as return values, exit statuses, and probably several other names, are one of the more critical features of shell scripting, as they play a role in almost every aspect of script execution.

Whenever a command executes (including the open bracket shell builtin used as part of the `if` and `while` statements), a result code is generated. If the command exits successfully, the result is usually zero (`0`). If the command exits with an error, the result code will vary according to the tool. (See the documentation for the tool in question for a list of result codes.) The possible range of result codes is 0-255.

There are three ways of testing to see if a script executes correctly. The first is with an immediate test using the if statement. For example:

|   |
|---|
|if ls mysillyfilename ; then|
|echo "File exists."|
|fi|

**Note:** This example is not the best way of testing whether a file exists. It is only intended as an example of a tool that returns a different exit status depending on whether it was successful at performing a task.

For more information about how to test for file existence using the if statement, see [The test Command and Bracket Notation](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/FlowControl,Expansion,andParsing/FlowControl,Expansion,andParsing.html#//apple_ref/doc/uid/TP40004268-CH4-SW5).

**C Shell Note:** The C shell also supports this technique (with a different syntax) as described in [The if Statement](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/FlowControl,Expansion,andParsing/FlowControl,Expansion,andParsing.html#//apple_ref/doc/uid/TP40004268-CH4-SW3).

The second way is by testing the last exit status returned. The exit status is stored in the shell variable `$?`. For example:

|   |
|---|
|ls mysillyfilename|
|if [ $? = 0 ] ; then|
|echo "File exists."|
|fi|

**C Shell Note:** The C shell exit status variable is called `$status`.

The third way is by taking advantage of the “and” operator:

|   |
|---|
|ls mysillyfilename && echo "File exists."|

These three code examples should generate the same output. The third technique is explained further in [Chaining Execution](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/ResultCodes,Chaining,andArgumentParsing/ResultCodes,Chaining,andArgumentParsing.html#//apple_ref/doc/uid/TP40004268-CH5-SW7).

## Chaining Execution

The shell provides three operators for chaining execution:and (`&&`), or (`||`) and not (`!`).

And (`&&`)

If the command to the left succeeds (has a zero exit status), the command to the right executes. Otherwise, it does not. The result code returned by this operation is success (zero) only if both commands return zero. Otherwise, its result code is whatever was returned by whichever command failed.

Or (`||`)

If the command to the left succeeds (has a zero exit status), the command to the right does not execute. If the command to the left fails, the command to the right does execute. If the leftmost command succeeds, the exit status returned by this operator is zero. Otherwise, the exit status returned is the exit status of the command to the right of the operator.

Not (`!`)

Executes the command to the right of the operator. If the command returns a zero exit status, the operator returns a nonzero exit status. If the command returns a nonzero exit status, the operator returns a zero exit status.

The three operators are shown in the following snippet:

|   |
|---|
|ls / \| ! ls mysillyfilename && echo "Whatever."|

The operator precedence rules in Bourne shell scripts are very different from those in C. Parentheses are evaluated first, as they can be used to override grouping of operators. After that, however, evaluation of operators occurs in order from left to right.

For example, the following line lists all of the files in the root directory, then echoes “It’s a boy”:

|   |
|---|
|ls / \| ls /xy && echo "It's a boy"|

The `||` operator takes precedence over the `&&` operator because of left-to-right evaluation rules. The shell shortcuts evaluation of the `||` operator. Thus, because `ls /` always succeeds, the `||` operator causes the second `ls` to be skipped entirely, and the statement up to the `&&` operator evaluates to `true` (`0`). This value is then combined with the echo statement after it by the `&&` operator. Thus, the echo statement executes afterwards.

**Note:** These rules are very different from the rules in C or most other programming languages. If you substitute function calls in C with the same return values (`true`, `false`, and `true`), the resulting statement behaves very differently. Consider the following statement:

|   |
|---|
|if (a() \| b() && c()) { ... }|

If functions `a` and `c` return `true` and function `b` returns `false`, the `&&` operator takes precedence over the `||` operator. Thus, when the first function call (`a`) executes and returns true, the `||` operator shortcuts the rest of the statement. However, the expression as a whole still evaluates to `true` in this case. The reason for this is easier to see if you rewrite the statement with parentheses to show the operator precedence like this:

|   |
|---|
|if  (a() \||
|(b() && c())|
|) { ... }|

You can modify the order of operations (or clarify it to avoid confusing people who are not used to languages without operator precedence) by adding parentheses, as shown in the next snippet:

|   |
|---|
|ls / \| ( ls /nonexistentfile && echo "file exists" )|

In this case, because the first `ls` statement is successful, the remainder of the statement is skipped. If you replace the `ls /` with `false`, the failed listing of `nonexistentfile` generates an error message and a nonzero exit status, which in turn causes the `echo` statement to still be skipped.

Of course, the existence of these operators also means that you could write an `if` statement without actually using the `if` keyword, as shown in the following snippet:

|   |
|---|
|FOO=3|
|[ $FOO -eq 3 ] && echo "three"|

Because this decreases readability, however, this syntax is not recommended. This form is presented here only to help with comprehension of existing scripts.

**C Shell Note:** The C shell syntax for chaining is identical to the Bourne shell syntax. However, you should be aware that some versions of the C shell have subtle bugs in their logic behavior. If you run into these bugs, adding parentheses around single statements can sometimes help.

## Handling Flags and Arguments

Throughout this chapter and previous chapters, examples have shown basic argument handling with variables such as `$1`, `$2`, and so on. This is fine for simple scripts, but some scripts call for more advanced argument processing. This section describes several techniques for processing arguments.

### Special Multi-argument Variables

The shell provides a number of special variables associated with argument lists:

`$#`.

Contains the number of arguments.

`$*`.

Expands to the list of arguments, starting from `$1`.

If this variable appears outside double quotes, each argument is treated as a single indivisible field for field splitting purposes. For example, if used in the argument list to a command, each original argument is passed to that command as a separate argument.

If this variable appears within double quotes, each argument is separated by the value of the `IFS` variable, and no field splitting occurs within the resulting block. Thus, if this variable is used as part of the argument list to a command, this entire `IFS`-delimited string is passed in as a single argument. See [Variable Expansion and Field Separators](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/FlowControl,Expansion,andParsing/FlowControl,Expansion,andParsing.html#//apple_ref/doc/uid/TP40004268-CH4-SW22) for more information about the `IFS` variable.

**Compatibility Note:** In AIX, if you surround this variable with quotes, the shell wraps each individual argument with quotes when it expands the variable.

`$@`.

Expands to the list of arguments, starting from `$1`.

If this variable appears outside double quotes, argument splitting behavior is not defined by the specification. However, in most shells, text is split as though the entire contents of each argument were inserted as-is, separated by spaces, and without any quotes.

If this variable appears within double quotes, each argument is treated as a single indivisible field for field splitting purposes. Thus, if this variable is used within double quotes as part of the argument list to a command, each original argument is passed as a separate argument to that command.

In addition, if this variable appears within double quote marks along with other text (`"BLAH$@BLAH"`, for example), the portion of the string prior to the `$@` is prepended to the first argument, and the portion of the string after the `$@` is appended to the last argument.

**C Shell Note:** This variable does not exist in C shell. Use `$*` instead.

The following code listings demonstrate the use of these arguments and the subtle differences between them.

**Listing 5-1**  00_listargs.sh

|   |
|---|
|#!/bin/sh|
||
|for i in "$@" ; do|
|echo ARG $i|
|done|

**Listing 5-2**  01_testargs.sh

|   |
|---|
|#!/bin/sh|
||
|IFS="|
|"|
||
|echo "COUNT: $#"|
|echo|
|echo '\$*'|
|./00_listargs.sh $*|
|echo|
|echo '"\$*"'|
|./00_listargs.sh "$*"|
|echo|
|echo '$@'|
|./00_listargs.sh $@|
|echo|
|echo '"$@"'|
|./00_listargs.sh "$@"|
|echo|
|echo '"foo bar$*bar foo"'|
|./00_listargs.sh "foo bar$*bar foo"|
|echo|
|echo '"foo bar$@bar foo"'|
|./00_listargs.sh "foo bar$@bar foo"|

Save these scripts with the filenames shown, then run them by typing `./01_testargs.sh This is a "silly test"` and note the differences in the way these variables behave.

### The shift Builtin

The `shift` builtin provides a way to remove arguments from the argument list. Each time you call the `shift` builtin, the first argument is deleted and the remaining arguments are shifted down by one. You can also specify an optional numeric argument to indicate how many times you want to shift the argument list.

The following script demonstrates the shift builtin:

**Listing 5-3**  02_shift.sh

|   |
|---|
|#!/bin/sh|
||
|echo "\$1: $1 \$2: $2 \$3: $3 \$4: $4 \$5: $5 \$6: $6"|
||
|shift|
||
|echo "\$1: $1 \$2: $2 \$3: $3 \$4: $4 \$5: $5 \$6: $6"|
||
|shift 2|
||
|echo "\$1: $1 \$2: $2 \$3: $3 \$4: $4 \$5: $5 \$6: $6"|

Run this script by typing `./02_shift.sh The quick brown fox jumped over the lazy dog.` and notice how the arguments change. Initially, the first six arguments are `"The quick brown fox jumped over"`. After the first `shift` statement, the first six arguments are `"quick brown fox jumped over the"`. After the second shift statement, the first six arguments are `"fox jumped over the lazy dog"`.

**C Shell Note:** The C shell implementation of the `shift` builtin is somewhat different, though the most basic form is the same. The C shell version does not take a numeric parameter to indicate the number of times to shift, however. Instead, if you pass it an array variable as an argument; the contents of the array are shifted similarly.

### The getopts builtin and the getopt command

The `getopts` builtin and the `getopt` command both process a list of arguments in a manner that is similar to the `[getopt](https://developer.apple.com/library/archive/documentation/System/Conceptual/ManPages_iPhoneOS/man3/getopt.3.html#//apple_ref/doc/man/3/getopt)` function in C. If you are writing a Bourne shell script, the `getopts` builtin is strongly recommended because it is faster, safer, and more flexible. (If you are writing a C shell script, the `getopts` builtin is not available.)

Both `getopt` and `getopts` take an option string as an argument. This option string is constructed as follows:

Simple flag

Just use the letter of the flag. For example, to add the `"-f"` flag, add the letter `"f"` to the option string.

Flag with argument

Use the letter of the flag followed by a colon. For example, if you want to accept something like `"-o filename"`, you would add `"o:"` to the option string.

As a special option, the `getopts` built-in supports detection of unknown flags and missing arguments. To enable this option, add a colon (`:`) as the first character of the option string.

#### The getopts Builtin

The `getopts` builtin puts your script in control of the argument parsing process. Each call to `getopts` returns a single flag and, where applicable, the argument to that flag. The syntax is as follows:

|   |
|---|
|getopts _opt_string_ _user_specified_variable_ [_args_]|

The option string is described above in [The getopts builtin and the getopt command](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/ResultCodes,Chaining,andArgumentParsing/ResultCodes,Chaining,andArgumentParsing.html#//apple_ref/doc/uid/TP40004268-CH5-SW62). The user-specified variable is described below. The `getopts` builtin can also optionally take a list of arguments to process. You should generally omit this.

The `getopts` builtin modifies the values of the following variables:

_user_specified_variable_

The first option you pass to `getopts` is the name of a variable. The `getopts` variable puts the flag itself into the specified variable (without the leading hyphen).

`OPTARG`

The argument value associated with the current flag (if applicable).

`OPTERR`

In some shells, if this variable is set to `1`, error reporting by the underlying `[getopt](https://developer.apple.com/library/archive/documentation/System/Conceptual/ManPages_iPhoneOS/man3/getopt.3.html#//apple_ref/doc/man/3/getopt)` function is enabled. If set to `0`, error reporting is disabled. This is not portable, but it is relatively harmless to set this variable “just in case”. This variable is ignored if the first character of the option string is a colon (`:`), which tells `getopts` that the script knows how to handle and report errors.

`OPTIND`

The index of the current argument being processed. You should set this to 1 before calling the `getopts` builtin for the first time (or to start over, processing the arguments again using a different set of options).

For example, the following script is a crude variant of the `ls` command. It takes an optional `-l` flag that enables long listings and an optional `-o` flag that contains the name of a file into which it writes its output. If no output file is specified, it writes its output to standard output. It also takes an optional path or list of paths that are passed to `ls` as-is.

**Listing 5-4**  03_getopts.sh

|   |
|---|
|#!/bin/sh|
||
|DO_LONG=""|
||
|# Start processing options at index 1.|
|OPTIND=1|
|# OPTERR=1|
|OUTPUT_FILE=""|
|while getopts ":hlo:" VALUE "$@" ; do|
||
|echo "GOT FLAG $VALUE"|
||
|if [ "$VALUE" = "h" ] ; then|
|echo "Usage: $0 [-l] [-o outputfile] [path ...]"|
|exit 1|
|fi|
|if [ "$VALUE" = "l" ] ; then|
|DO_LONG="-l"|
|fi|
|if [ "$VALUE" = "o" ] ; then|
|echo "Set output file to \"$OPTARG\""|
|OUTPUT_FILE="$OPTARG"|
|fi|
|# The getopt routine returns a colon when it encounters|
|# a flag that should have an argument but doesn't.  It|
|# returns the errant flag in the OPTARG variable.|
|if [ "$VALUE" = ":" ] ; then|
|echo "Flag -$OPTARG requires an argument."|
|echo "Usage: $0 [-l] [-o outputfile] [path ...]"|
|exit 1|
|fi|
|# The getopt routine returns a question mark when it|
|# encounters an unknown flag.  It returns the unknown|
|# flag in the OPTARG variable.|
|if [ "$VALUE" = "?" ] ; then|
|echo "Unknown flag -$OPTARG detected."|
|echo "Usage: $0 [-l] [-o outputfile] [path ...]"|
|exit 1|
|fi|
|done|
||
|# The first non-flag argument is at index $OPTIND, so shift one fewer|
|# to move it into $1|
|shift `expr $OPTIND - 1`|
||
|if [ "$OUTPUT_FILE" = "" ] ; then|
|ls $DO_LONG "$@"|
|else|
|ls $DO_LONG "$@" > $OUTPUT_FILE|
|fi|
||
|exit $?|

You should notice two things about this script. First, it takes advantage of the leading colon in the option string. This tells `getopts` that the script knows how to handle errors. Second, it provides two additional options—one for the colon (`:`) flag and one for the question mark (`?`) flag. The colon flags is returned when `getopts` encounters a flag with a missing argument. The question mark flag is returned when `getopts` encounters an unknown flag. These two additional cases are enabled by the leading colon in the option string.

**Note:** The `$?` variable is explained further in [Working with Result Codes](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/ResultCodes,Chaining,andArgumentParsing/ResultCodes,Chaining,andArgumentParsing.html#//apple_ref/doc/uid/TP40004268-CH5-SW3).

#### The getopt Command

The `getopt` command takes a different approach than the `getopts` builtin. It processes the entire argument list at once and lets you know whether the argument list matches the list of valid flags or not. If the argument list matches, `getopt` canonicalizes the argument list, putting the flags and their optional arguments first (prior to any non-flag arguments), followed by a single `"--"` argument to indicate that there are no more flags to process.

**Warning:** The `getopt` command does not support arguments that contain spaces because of the way it reconstructs the argument list. If at all possible, use the `getopts` builtin instead.

Because of this limitation, using `getopt` in Bourne shell scripts is strongly discouraged. To avoid encouraging bad behavior, the code snippet in this section is presented exclusively in the C shell dialect.

The syntax of the `getopt` command is as follows:

|   |
|---|
|getopt _opt_string_ _args_|

The following snippet behaves much like the one in [Listing 5-4](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/ResultCodes,Chaining,andArgumentParsing/ResultCodes,Chaining,andArgumentParsing.html#//apple_ref/doc/uid/TP40004268-CH5-SW61). Unlike in that example, it is not possible to programmatically detect the nature of errors (missing arguments or invalid flags).

Also, as noted previously, filenames containing spaces are not handled correctly by `getopt`. This is not a problem with the script. It is a fundamental limitation of the `getopt` tool and the way its output is parsed.

**Cross-Platform Compatibility Note:** The GNU (Linux) version of `getopt` provides additional flags that cause it to output a string quoted for a particular shell to work around this limitation. That usage is not portable, however, and is not compatible with the OS X `getopt` implementation.

**Listing 5-5**  01_getopt.csh

|   |
|---|
|#!/bin/csh|
||
|set OUTPUT_FILE=""|
|set DO_LONG=""|
||
|set argv=`getopt "hlo:" $*`|
||
|if ( $status != 0 ) then|
|echo "Usage: $0 [-l] [-o outputfile] [path ...]"|
|exit 1|
|endif|
||
|while ( "$1" != "--" )|
|echo "GOT FLAG $1"|
|switch($1)|
|case "-h":|
|echo "Usage: $0 [-l] [-o outputfile] [path ...]"|
|exit 1|
|case "-o":|
|set OUTPUT_FILE="$2"|
|shift|
|breaksw|
|case "-l":|
|set DO_LONG="-l"|
|breaksw|
|endsw|
|shift|
|end|
||
|shift # remove trailing --|
||
|# echo "ARGS: $*"|
||
|if ( "$OUTPUT_FILE" == "" ) then|
|ls $DO_LONG $*|
|else|
|ls $DO_LONG $* > $OUTPUT_FILE|
|endif|
||
|exit $status|

**Note:** The `$status` variable is explained further in [Working with Result Codes](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/ResultCodes,Chaining,andArgumentParsing/ResultCodes,Chaining,andArgumentParsing.html#//apple_ref/doc/uid/TP40004268-CH5-SW3).

[Next](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/SubroutinesandScoping/SubroutinesandScoping.html)[Previous](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/FlowControl,Expansion,andParsing/FlowControl,Expansion,andParsing.html)

  

  

---

Copyright © 2003, 2014 Apple Inc. All Rights Reserved. [Terms of Use](http://www.apple.com/legal/internet-services/terms/site.html) | [Privacy Policy](http://www.apple.com/privacy/) | Updated: 2014-03-10

## How helpful is this document?

*

 Very helpful Somewhat helpful  Not helpful

## How can we improve this document?

 Fix typos or links Fix incorrect information  Add or update code samples  Add or update illustrations  Add information about...

*

_* Required information_

To submit a product bug or enhancement request, please visit the [Bug Reporter](https://developer.apple.com/bugreporter/) page.

Please read [Apple's Unsolicited Idea Submission Policy](http://www.apple.com/legal/policies/ideas.html) before you send us your feedback.