# Shell Input and Output

The Bourne shell provides a number of ways to read and write files, display text, and get information from the user, including `echo` (described previously in [Shell Script Basics](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/shell_scripts/shell_scripts.html#//apple_ref/doc/uid/TP40004268-CH237-SW3)), `printf`, `read`, `cat`, pipes, and redirection. This chapter describes these mechanisms.

## Shell Script Input and Output Using printf and read

The Bourne shell syntax provides basic input with very little effort.

```bash
#!/bin/sh
printf "What is your name?  -> "
read NAME
echo "Hello, $NAME.  Nice to meet you."
```

You will notice two things about this script. The first is that it introduces the `printf` command. This command is used because, unlike `echo`, the `printf` command does not automatically add a newline to the end of the line of output. This behavior is useful when you need to use multiple lines of code to output a single line of text. It also just happens to be handy for prompts.

**Note:** In most operating systems, you can tell `echo` to suppress the newline. However, the syntax for doing so varies. Thus, `printf` is recommended for printing prompts. See [Designing Scripts for Cross-Platform Deployment](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/PortingScriptstoMacOSX/PortingScriptstoMacOSX.html#//apple_ref/doc/uid/TP40004268-TP40003517-SW2) for more information and other alternatives.

The second thing you'll notice is the `read` command. This command takes a line of input and separates it into a series of arguments. Each of these arguments is assigned to the variables in the `read` statement in the order of appearance. Any additional input fields are appended to the last entry.

You can modify the behavior of the `read` command by modifying the shell variable `IFS` (short for internal field separators). The default behavior is to split inputs everywhere there is a space, tab, or newline. By changing this variable, you can make the shell split the input fields by tabs, newlines, semicolons, or even the letter 'q'. This change in behavior is demonstrated in the following example:

```bash
#!/bin/sh
printf "Type three numbers separated by 'q'. -> "
IFS="q"
read NUMBER1 NUMBER2 NUMBER3
echo "You said: $NUMBER1, $NUMBER2, $NUMBER3"
```


If, for example, you run this script and enter `1q3q57q65`, the script replies with `You said: 1, 3, 57q65`. The third value contains `57q65` because only three values are requested in the read statement.

**Note:** The `read` statement _always_ stops reading at the first newline encountered. Thus, if you set `IFS` to a newline, you cannot read multiple entries with a single `read` statement.

**Warning:** Changing `IFS` may cause unexpected consequences for variable expansion. For more information, see [Variable Expansion and Field Separators](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/FlowControl,Expansion,andParsing/FlowControl,Expansion,andParsing.html#//apple_ref/doc/uid/TP40004268-CH4-SW22).

But what if you don’t know how many parameters the user will specify? Obviously, a single read statement cannot split the input up into an arbitrary number of variables, and the Bourne shell does not contain true arrays. Fortunately, the `eval` builtin can be used to simulate an array using multiple shell variables. This technique is described in [Using the eval Builtin for Data Structures, Arrays, and Indirection](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/AdvancedTechniques/AdvancedTechniques.html#//apple_ref/doc/uid/TP40004268-TP40003521-SW1).

Alternatively, you can use the `for` statement, which splits a single variable into multiple pieces based on the internal field separators. This statement is described in [The for Statement](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/FlowControl,Expansion,andParsing/FlowControl,Expansion,andParsing.html#//apple_ref/doc/uid/TP40004268-CH4-SW10).

**C Shell Note:** In the C shell, the syntax for reading is completely different. The following script is the C shell equivalent of the script earlier in this section:

   
```bash
printf "What is your name?  -> "
set NAME = "$<"
echo "Hello, $NAME.  Nice to meet you."
```

The C shell does not provide a way to read multiple values in a single command, though you can approximate this with careful use of `sed` as described in [Regular Expressions Unfettered](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/RegularExpressionsUnfettered/RegularExpressionsUnfettered.html#//apple_ref/doc/uid/TP40004268-CH238-SW7) or `cut`. For example:

   
```bash
#!/bin/csh
printf "Type three numbers separated by 'q'. -> "

set LINE = "$<"

set NUMBER1 = `echo "$LINE" \| cut -f 1 -d 'q'`
set NUMBER2 = `echo "$LINE" \| cut -f 2 -d 'q'`
set NUMBER3 = `echo "$LINE" \| cut -f 3 -d 'q'`

echo "You said: $NUMBER1, $NUMBER2, $NUMBER3"
```

## Bulk I/O Using the cat Command

For small I/O, the `echo` command is well suited. However, when you need to create large amounts of data, it may be convenient to send multiple lines to a file simultaneously. For these purposes, the `cat` command can be particularly useful.

By itself, the `cat` command really doesn’t do anything that can’t be done using redirect operators (except for printing the contents of a file to the user’s screen). However, by combining it with the special operator `<<`, you can use it to send a large quantity of text to a file (or to the screen) without having to use the `echo` command on every line.

For example:

   
```bash
cat > mycprogram.c << EOF
#include <stdio.h>
int main(int argc, char *argv[])
{
	char array[] = { 0x25, 115, 0 };
	char array2[] = { 68, 0x61, 118, 0x69, 0144, 040,
		0107, 97, 0x74, 119, 0157, 0x6f,
		100, 0x20, 0x72, 117, 'l', 0x65,
		115, 041, 012, 0 };
	printf(array, array2);
}
EOF
```

This example script takes the text after the line containing the `cat` command up to (but not including) the line that _begins_ with `EOF` and stores it into the file `mycprogram.c`. Note that the token `EOF` can be replaced with any token, so long as the following conditions are met:

- The token must not contain spaces unless you surround it with quotation marks. (These outer quotation marks are not considered part of the token unless you quote them.)
    
- Shell variables in the name of the token are _not_ expanded, so the `$` character is just like any other ordinary character.
    
- The token after the `<<` in the starting line must match the token at the beginning of the last line.
    
- The end-of-block token must be the only thing that appears on the line. If it shares the line with any other characters (_including_ whitespace), it will be treated as part of the text to be output.
    
- The end-of-block token you choose must never appear as a line in the intended output string.
    

This technique is also frequently used for printing instructions to the user from an interactive shell script. This avoids the clutter of dozens of lines of `echo` commands and makes the text much easier to read and edit in an external text editor (if desired).

**Note:** Although shell variables cannot be used to define the token itself, by default, shell variables `are` expanded within the string to be printed. To disable this expansion, surround the token with single or double quote marks. For example:

   
```bash
cat << 'EOF'
The variable in this line will not be expanded: $PATH
EOF
```

Notice that `EOF` does not appear in quotes in the actual text. This is a key difference between the Bourne shell and C shell behavior. If you want to explicitly look for EOF within single quotes, you would write it like this:

   
```bash
cat << "'EOF'"
...
'EOF'
```

or

   
```bash
cat << \''EOF'\'
...
'EOF'
```

Another classic example of this use of `cat` in action is the `.shar` file format, created by the tool `shar` (short for SHell ARchive). This tool takes a list of files as input and uses them to create a giant shell script which, when executed, recreates those original files. To avoid the risk of the end-of-block token appearing in the input file, it prepends each line with a special character, then strips that character off on output.

**C Shell Note:** The multiline `cat` syntax in the C shell is the same as in the Bourne shell, with one key difference: the entire token is treated as literal text for matching purposes, including backslashes and quotation marks. For example:

   
```bash
cat << 'EOF'
The variable in this line will not be expanded: $PATH
'EOF'
```

For another example:

   
```bash
cat << \''EOF'\'
The variable in this line will not be expanded: $PATH
\''EOF'\'
```

In both cases, the quotation marks still behave as a switch to control whether or not to expand variables within the output.

## Pipes and Redirection

As you may already be aware, the true power of shell scripting lies not in the scripts themselves, but in the ability to read and write files and chain multiple programs together in interesting ways.

Each program in a UNIX-based or UNIX-like system has three basic file descriptors (normally a reference to a file or socket) reserved for basic input and output: standard input (often abbreviated stdin), standard output (stdout), and standard error (stderr).

The first, standard input, normally takes input from the user's keyboard (when the shell window is in the foreground, of course). The second, standard output, normally contains the output text from the program. The third, standard error, is generally reserved for warning or error messages that are not part of the normal output of the program. This distinction between standard output and standard error is a very important one, as explained in [Pipes and File Descriptor Redirection (Bourne Shell)](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/ShellInputandOutput/ShellInputandOutput.html#//apple_ref/doc/uid/TP40004268-CH3-SW13).

### Basic File Redirection

One of the most common types of I/O in shell scripts is reading and writing files. Fortunately, it is also relatively simple to do. Reading and writing files in shell scripts works exactly like getting input from or sending output to the user, but with the standard input redirected to come from a file or with the standard output redirected to a file.

For example, the following command creates a file called `MyFile` and fills it with a single line of text:

   
```bash
echo "a single line of text" > MyFile
```

Appending data is just as easy. The following command appends another line of text to the file `MyFile`.

   
```bash
echo "another line of text" >> MyFile
```

You should notice that the redirect operator (`>`) creates a file, while the append operator (`>>`) appends to the file.

Many (but not all) Bourne-compatible shells support a third operator in this family, the merging redirect operator (`>&`) that redirects standard error and standard output simultaneously to a file. For example:

   
```bash
ls . THISISNOTAFILE >& filelistwitherrors
```

This creates a file called `filelistwitherrors`, containing both a listing of the current directory and an error message about the nonexistence of the file `THISISNOTAFILE`. The standard output and standard error streams are merged and written out to the resulting file.

**Compatibility Note:** Not all Bourne shell variants support the `>&` operator when used in this way. This simplified behavior is not specified by POSIX, and a few shells (most notably `ash` and its Debian derivative, `dash`) generate an error if you try to use this operator without specifying a file descriptor number after the `>&`. For maximum portability, you should redirect standard output to a file, then separately combine standard error into standard output like this:

   
```bash
ls . THISISNOTAFILE > filelistwitherrors 2>&1
```

See [Pipes and File Descriptor Redirection (Bourne Shell)](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/ShellInputandOutput/ShellInputandOutput.html#//apple_ref/doc/uid/TP40004268-CH3-SW13) for more information about using file descriptor redirection to combine file descriptors.

**Note:** The `>&` operator is also very powerful when used for file descriptor redirection. Additional uses beyond basic use are described in more detail in [Pipes and File Descriptor Redirection (Bourne Shell)](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/ShellInputandOutput/ShellInputandOutput.html#//apple_ref/doc/uid/TP40004268-CH3-SW13) and [Scripting Interactive Tools Using File Descriptors](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/AdvancedTechniques/AdvancedTechniques.html#//apple_ref/doc/uid/TP40004268-TP40003521-SW50).

### Pipes and File Descriptor Redirection (Bourne Shell)

The simplest example of the use of pipes is to pipe the standard output of one program to the standard input of another program. Type the following on the command line:

   
```bash
ls -l \| grep 'rwx'
```

You will see all of the files whose permissions (or name) contain the letters `rwx` in order. The `ls` command lists files to its standard output, and the `grep` command takes its input and sends any lines that match a particular pattern to its standard output. Between those two commands is the pipe operator (`|`). This tells the shell to connect the standard output of `ls` to the standard input of `grep`.

Where the distinction between standard output becomes significant is when the `ls` command gives an error.

   
```bash
ls -l THISFILEDOESNOTEXIST \| grep 'rwx'
```

You should notice that the `ls` command issued an error message (unless you have a file called `THISFILEDOESNOTEXIST` in your home directory, of course). If the `ls` command had sent this error message to its standard output, it would have been gobbled up by the `grep` command, since it does not match the pattern `rwx`. Instead, the `ls` command sent the message to its standard error descriptor, which resulted in the message going directly to your screen.

In some cases, however, it can be useful to redirect the error messages along with the output. You can do this by using a special form of the combining redirection operator (`>&`).

Before you can begin, though, you need to know the file descriptor numbers. Descriptor 0 is standard input, descriptor 1 is standard output, and descriptor 2 is standard error. Thus, the following command combines standard error into standard output, then pipes the result to grep:

   
```bash
ls -l THISFILEDOESNOTEXIST 2>&1 \| grep 'rwx'
```

This operator is also often useful if your script needs to send a message to standard error. The following command sends “an error message” to standard error:

   
```bash
echo "an error message" 1>&2
```

This works by taking the standard output (descriptor 1) of the echo command and redirects it to standard error (descriptor 2).

You should notice that the ampersand (`&`) appears to behave somewhat differently than it did in [Basic File Redirection](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/ShellInputandOutput/ShellInputandOutput.html#//apple_ref/doc/uid/TP40004268-CH3-SW10). Because the ampersand is followed immediately by a number, this causes the output of one data stream to be merged into another stream. In actuality, however, the effect is the same (assuming your shell supports the use of `>&` by itself).

The redirect (`>`) operator implicitly redirects standard output. When combined with an ampersand and followed by a filename, in some shells, it merges standard output and standard error and writes the result to a file, though this behavior is not portable. By specifying numbers, your script is effectively overriding which file descriptor to use as its source and specifying a file descriptor to receive the result instead of a file.

**Note:** Be careful when mixing normal redirection with file descriptor merging. The following command combines standard output and standard error into a single output file.

   
```bash
ls . BOGUSFILENAME > filelistwitherrors 2>&1
```

If you reverse the order of the redirects, however, only standard output is written into the file.

   
```bash
ls . BOGUSFILENAME 2>&1 > just the file
```

Further, if you pipe the result of the second version above into another utility, it will receive the standard error output from the `ls` command.

### Pipes and File Descriptor Redirection (C Shell)

The C shell does not support the full set of file descriptor redirection that the Bourne shell supports. In some cases, alternatives are provided. For example, you can pipe standard output and standard error to the same process using the `|&` operator as shown in the following snippet:

   
```bash
ls -l THISFILEDOESNOTEXIST \|& grep 'rwx'
```

Some other operations, however, are not possible. You cannot, for example, redirect standard error without redirecting standard output. At best, if you can determine that your standard output will always be `/dev/tty`, you can work around this by redirecting standard output to `/dev/tty` first, then redirecting both the now-empty standard output and standard error using the `>&` operator. For example, to redirect only standard error to `/dev/null`, you could do this:

   
```bash
(ls > /dev/tty) >& /dev/null
```

This technique is _not_ recommended for general use, however, as it will send output to your screen if anyone runs your script with standard output set to a file or pipe.

You can also work around this using a file, but not in an interactive way. For example:

   
```bash
(ls > /tmp/mytemporarylslisting) >& /dev/null
cat /tmp/mytemporarylslisting
```

It is, however, possible to discard standard output and capture standard input. For example:

   
```bash
(ls / /bogusfile > /dev/null) \|& more
```

It is not possible to redirect messages to standard error using the C shell unless you write a Bourne shell script or C program to do the redirection for you.

[Next](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/FlowControl,Expansion,andParsing/FlowControl,Expansion,andParsing.html)[Previous](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/shell_scripts/shell_scripts.html)

  

  

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