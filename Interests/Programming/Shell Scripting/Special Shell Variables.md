[[Command Line Primer|<- Previous]] [[Other Tools and Information|Next ->]] 
# Special Shell Variables

The Bourne shell has a number of special “automatic” variables that it maintains for informational purposes. These variables provide information such as the process ID of the shell, the exit status of the last command, and so on. This section provides a list of these special variables. For additional variables supported by specific Bourne shell variants such as BASH and ZSH, see the `bash` and `zshparam` manual pages, respectively.

|Variable|Description|
|---|---|
|**Process information**|   |
|`$$`|Process ID of shell|
|`$PPID`|Process ID of shell’s parent process.<br><br>**Quirk Warning:**For subshells, the value of PPID is inherited from the parent shell. Thus, PPID is only the parent of the outermost shell process.|
|`$?`|Exit status of last command.|
|`$_`|Name of last command.|
|`$!`|Process ID of last process run in the background using ampersand (&) operator. This is commonly used in conjunction with the `wait` builtin.|
|`$PATH`|A colon-delimited list of locations where trusted executables are installed. Any executable in one of these locations can be executed without specifying a complete path.|
|**Field and record parsing**|   |
|`$IFS`|Input Field Separators (uses are explained in [Variable Expansion and Field Separators](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/FlowControl,Expansion,andParsing/FlowControl,Expansion,andParsing.html#//apple_ref/doc/uid/TP40004268-CH4-SW22))|
|**User information**|   |
|`$HOME`|The user’s home directory.|
|`$UID`|The user’s ID.<br><br>**Security Warning:**This value can be modified by the calling script, so it should not be used for authentication purposes.|
|`$USER`|The user’s (short) login name.<br><br>**Security Warning:**This value can be modified by the calling script, so it should not be used for authentication purposes.|
|**Miscellaneous Variables**|   |
|`$#`|Number of arguments passed to the shell. This variable is described further in [Handling Flags and Arguments](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/ResultCodes,Chaining,andArgumentParsing/ResultCodes,Chaining,andArgumentParsing.html#//apple_ref/doc/uid/TP40004268-CH5-SW63).|
|`$@`|Complete list of arguments passed to the shell, separated by spaces.. This variable is described further in [Handling Flags and Arguments](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/ResultCodes,Chaining,andArgumentParsing/ResultCodes,Chaining,andArgumentParsing.html#//apple_ref/doc/uid/TP40004268-CH5-SW63).|
|`$*`|Complete list of arguments passed to the shell, separated by the first character of the `IFS` (input field separators) variable. This variable is described further in [Handling Flags and Arguments](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/ResultCodes,Chaining,andArgumentParsing/ResultCodes,Chaining,andArgumentParsing.html#//apple_ref/doc/uid/TP40004268-CH5-SW63).|
|`$-`|A list of all shell flags currently enabled.|
|`$PWD`|The current working directory. Equivalent to executing the `pwd` command.|

[Next](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/ForMoreInformation/ForMoreInformation.html)[Previous](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/CommandLInePrimer/CommandLine.html)

  

  

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