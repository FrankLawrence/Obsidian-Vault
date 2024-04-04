# Designing Scripts for Cross-Platform Deployment

For the most part, scripts that run on other UNIX-based or UNIX-like platforms (Linux, for example) also run correctly on OS X and vice versa. There are differences, however.

In addition to finding subtle variations in the file system hierarchy and the behavior of common command-line tools, you will also find different tools and technologies for device I/O and for adding and removing users and groups.

## Bourne Shell Version

OS X provides BASH as its Bourne shell implementation. When executed as `/bin/sh`, it should be fully compatible with other implementations. However, occasionally differences may arise. The same is true of other operating systems that use BASH or ZSH as their Bourne shell implementation.

For maximum compatibility, you should carefully avoid using any BASH-specific extensions in shell scripts. If you cannot avoid BASH extensions, you should explicitly make the script execute in BASH by changing the first line to the following:

|   |
|---|
|#!/bin/bash|

You should use a similar first line for scripts written using ZSH extensions.

**Compatibility Note:** For detailed lists of places where BASH and ZSH differ from pure Bourne shell variants, see [http://www.gnu.org/software/bash/manual/bashref.html#Major-Differences-From-The-Bourne-Shell](http://www.gnu.org/software/bash/manual/bashref.html#Major-Differences-From-The-Bourne-Shell) and [http://zsh.dotsrc.org/FAQ/zshfaq02.html](http://zsh.dotsrc.org/FAQ/zshfaq02.html).

For more information about BASH and ZSH, see the manual pages for `bash` and `zsh`.

For maximum cross-platform compatibility, you should test your code using several shells, including `dash` and/or `ash`. For more information about DASH, see [http://gondor.apana.org.au/~herbert/dash/](http://gondor.apana.org.au/~herbert/dash/).

## Cross-Platform Line Endings

Different operating systems use different characters to indicate the end of each line in text files. This can cause strange and unusual behavior if you aren’t expecting it:

- Command-line tools in OS X (and other UNIX or Linux variants) use UNIX-style line endings. This means that each line in a text file ends with a newline character (character `10`/`0xA`, often abbreviated LF).
    
- Many older Mac applications use "Mac-style” line endings. This means that each line in a text file ends with a carriage return character (character `13`/`0xD`, often abbreviated CR).
    
    When processed with command-line utilities in UNIX or Linux variants, files with legacy Mac-style line endings show up as a single line on the screen; as each line printed to the screen, it overwrites the previous line. This is because UNIX and Linux move the cursor to the left edge of the screen when they encounter a carriage return, but do not move the cursor down a line.
    
- Windows applications and many network services use Windows-style line endings. This means that each line in a text file ends with both a carriage return and a line feed (character `13`/`0xD` followed by character `10`/`0xA`, often abbreviated CR/LF or CRLF).
    
    When processed with command-line utilities in UNIX or Linux variants, content with Windows-style line endings looks right, but may behave in unexpected ways due to the extra carriage return at the end of each line. For example, the extra carriage return can perturb the splitting behavior in awk, can cause patterns that use the end-of-line anchor in regular expressions to fail, and so on.
    
- Occasionally, you may also encounter a file that ends with a newline followed by a carriage return (the reverse of Windows line endings, abbreviated LF/CR or LFCR).
    
    When processed with command-line utilities in UNIX or Linux variants, as with Windows-style line endings, everything will appear right, but you will get strange behavior, including field splitting problems, misbehavior of patterns containing the start-of-line anchor in regular expressions, and so on.
    

It is generally straightforward to detect the line ending type of a text file and read it correctly. The following code snippet demonstrates one way:

**Listing 10-1**  Converting line endings to UNIX-style newlines

|   |
|---|
|TYPE="$(file "$1" \| sed 's/.*with //' \| sed 's/ .*//')"|
||
|if [ "$TYPE" = "CR" ] ; then|
|DATA="$(tr '\r' '\n' < "$1")"|
|else|
|# Most versions of the "file" command can't detect|
|# LFCR line endings, so do this even if the file|
|# appears to have UNIX line endings.|
|DATA="$(tr -d '\r' < "$1")"|
|fi|

Converting between these formats is also relatively easy once you have determined that you need to do so.

**Listing 10-2**  Converting between line ending formats

|   |
|---|
|# Convert from legacy Mac-style CR line endings|
|# to UNIX-style LF line endings for use with|
|# command-line tools|
|tr '\r' '\n' < mac_text_file > unix_text_file|
||
|# Convert from UNIX-style LF to legacy Mac-style CR|
|# line endings|
|tr '\n' '\r' < unix_text_file > mac_text_file|
||
|# Convert from Windows-style CR/LF line endings (or|
|# LF/CR line endings) to UNIX line endings|
|tr -d '\r' < windows_text_file > unix_text_file|
||
|# Convert from UNIX-style LF line endings to|
|# Windows-style CR/LF line endings|
|CR="$(printf "\r")"|
|sed "s/$/$CR/" < unix_text_file > windows_text_file|

## Working with Device I/O

OS X uses the I/O Kit for device drivers. Unlike most UNIX-based and UNIX-like operating systems, most devices are not exposed through device files in `/dev`. (Disks and serial ports are notable exceptions.)

In general, device I/O must be written in a C-derived language using the functionality in the I/O Kit framework. However, if you are writing your own device driver, you can expose a device file in `/dev` if desired.

**Note:** Devices _cannot_ be accessed through `/dev/mem` in OS X.

See _[IOKit Fundamentals](https://developer.apple.com/library/archive/documentation/DeviceDrivers/Conceptual/IOKitFundamentals/Introduction/Introduction.html#//apple_ref/doc/uid/TP0000011)_ for general information, _[Accessing Hardware From Applications](https://developer.apple.com/library/archive/documentation/DeviceDrivers/Conceptual/AccessingHardware/AH_Intro/AH_Intro.html#//apple_ref/doc/uid/TP30000376)_ to learn how to write an application to access device drivers from user space, or _[Kernel Programming Guide](https://developer.apple.com/library/archive/documentation/Darwin/Conceptual/KernelProgramming/About/About.html#//apple_ref/doc/uid/TP30000905)_ to learn how to support device files and the `[ioctl](https://developer.apple.com/library/archive/documentation/System/Conceptual/ManPages_iPhoneOS/man2/ioctl.2.html#//apple_ref/doc/man/2/ioctl)` system call in the kernel.

## File System Hierarchy

A number of files are in different places in OS X than in other operating systems. For more information about the OS X layout, read _[File System Overview](https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPFileSystem/BPFileSystem.html#//apple_ref/doc/uid/10000185i)_. For more information about other operating systems, read the following:

- `hier`—The OS X manual page hier(7) describes the OS X file-system hierarchy.
    
- [http://www.FreeBSD.org/cgi/man.cgi?query=hier&sektion=7](http://www.FreeBSD.org/cgi/man.cgi?query=hier&sektion=7)—The FreeBSD manual page `hier(7)` describes the FreeBSD file-system hierarchy. It is similar to the hierarchy used by most BSD-based operating systems. (No, the spelling of section is not a typo.)
    
- [http://www.pathname.com/fhs/](http://www.pathname.com/fhs/)—The Filesystem Hierarchy Standard describes the file system hierarchy used by Linux-based operating systems, and is derived from the hierarchy used by AT&T UNIX-based operating systems.
    
- [http://h20000.www2.hp.com/bc/docs/support/SupportManual/c02255645/c02255645.pdf](http://h20000.www2.hp.com/bc/docs/support/SupportManual/c02255645/c02255645.pdf)—This appendix from the HP-UX documentation describes the hierarchy of AT&T UNIX-based operating systems.
    
- [http://publib.boulder.ibm.com/infocenter/pseries/v5r3/index.jsp?topic=/com.ibm.aix.baseadmn/‍doc/baseadmndita/fs_tree_org.htm](http://publib.boulder.ibm.com/infocenter/pseries/v5r3/index.jsp?topic=/com.ibm.aix.baseadmn/doc/baseadmndita/fs_tree_org.htm)—This page in the IBM pSeries and AIX Information Center describes the hierarchy of AIX.
    

## System Administration Tasks

This section provides an overview of a few common system administration tasks. Complete coverage of system administration tasks is beyond the scope of this document. For a more thorough treatment, read _Introduction to Command-Line Administration_ at [http://manuals.info.apple.com/en_US/IntroCommandLine_v10.6.pdf](http://manuals.info.apple.com/en_US/IntroCommandLine_v10.6.pdf).

### Managing Users and Groups

In the default configuration of OS X, users and groups are not stored in a password file on disk. Thus, you cannot modify the password file directly.

OS X supports a number of data stores for user and group information, including LDAP and flat files. Depending on the configuration, users could potentially be stored locally or remotely and accessed through any of these methods. Thus, to add users and groups through shell scripts in a general way, you _must_ use the Directory Service command-line utility, `dscl` (or the Directory Service API upon which that utility is based).

Because the `dscl` tool is specific to OS X, if you are writing scripts for cross-platform deployment, you should test for its existence and fall back to traditional password file modification if it is not there. To learn how to do this, read [The if Statement](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/FlowControl,Expansion,andParsing/FlowControl,Expansion,andParsing.html#//apple_ref/doc/uid/TP40004268-CH4-SW3).

To learn more about managing users and groups from the command line, read _Introduction to Command-Line Administration_ at [http://manuals.info.apple.com/en_US/IntroCommandLine_v10.6.pdf](http://manuals.info.apple.com/en_US/IntroCommandLine_v10.6.pdf).

To learn more about Directory Service records at a high level, read _[Open Directory Programming Guide](https://developer.apple.com/library/archive/documentation/Networking/Conceptual/Open_Directory/Introduction/Introduction.html#//apple_ref/doc/uid/TP40000917)_. To learn how to use the Directory Service command line utility to alter those records, read the manual page for `dscl`.

To see how to manually add a new user from the command line, read the [Additional Features](https://developer.apple.com/library/archive/documentation/Porting/Conceptual/PortingUnix/additionalfeatures/additionalfeatures.html#//apple_ref/doc/uid/TP40002856) chapter of _[Porting UNIX/Linux Applications to OS X](https://developer.apple.com/library/archive/documentation/Porting/Conceptual/PortingUnix/intro/intro.html#//apple_ref/doc/uid/TP30001003)_. For scripts to help you add new users and groups programmatically, see [User and Group Management](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/StartingPoints/StartingPoints.html#//apple_ref/doc/uid/TP40004268-CH2-SW24) in the [Starting Points](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/StartingPoints/StartingPoints.html#//apple_ref/doc/uid/TP40004268-CH2-SW1) chapter of this document.

### Access Control List (ACL) Management

Some UNIX-based and UNIX-like operating systems provide `setfacl`, `chacl`, or `acledit`/`aclget`/`aclput` for setting file and directory ACLs. OS X does not. Instead, OS X provides file ACL modification through the `chmod` command.

Regrettably, there is no standardized syntax for getting and setting ACLs on the command line (nor even a standard set of supported rights across operating systems). Currently, the only way to portably handle ACLs is to avoid them entirely or to require your users to write an OS-specific plug-in.

If you must use ACLs in a cross-platform script, you must special-case the code on a per-OS basis. The easiest way to do this is to use the output of the `uname` command. (See the `uname` manual page for more information.)

### Disk Management and Partitioning

Disk management and partitioning tools vary widely from one UNIX-based or UNIX-like OS to the next. It is impractical for this document to cover the subject in depth.

For information on other UNIX-based and UNIX-like operating systems, a good place to start is the _UNIX System Administration Handbook_ by Nemeth and others.

For information about OS X command-line tools for disk management and partitioning, read _Introduction to Command-Line Administration_ at [http://manuals.info.apple.com/en_US/IntroCommandLine_v10.6.pdf](http://manuals.info.apple.com/en_US/IntroCommandLine_v10.6.pdf), and see section 8 of _OS X Man Pages_. In particular, you should look at the man pages for `hdiutil`, `pdisk`, `fdisk`, `gpt`, and `diskutil`.

## General Command-Line Tool Differences

A number of command-line tools behave differently across various UNIX-based and UNIX-like operating systems. This chapter explains some of the key differences in those tools.

UNIX-based and UNIX-like operating systems generally fall into one of three camps:

- **AT&T UNIX:** Also known as UNIX System V (in its latest incarnation), AT&T UNIX was the original UNIX operating system. Its descendants include most operating systems that are commonly referred to as UNIX.
    
- **BSD:** Short for Berkeley Software Distribution, BSD is the name given to a family of operating systems descended from a derivative of UNIX that was originally distributed by the University of California, Berkeley, in the 1970s.
    
    Over the years, the Berkeley distribution and the AT&T distribution continued to diverge. The result is that there are a number of subtle syntax differences between shell scripts written for systems that follow AT&T semantics versus those that follow BSD semantics.
    
    In the 1990s, BSDi (a commercial company formed as a result of the UC Berkeley research) released the BSD operating system as open source. Most modern BSD operating systems are derived from this source base, known as 4.4BSD-Lite release 2.
    
    Because of licensing restrictions on the original UNIX source code, the portions that were originally written by AT&T had to be rewritten under a more permissive license in order to release it as open source. This contributed further to the differences in syntax between BSD-based and AT&T UNIX-based operating systems.
    
- **Linux and GNU:** During the 1990s, a new operating system, Linux, was born. Combining a kernel written by Linus Torvalds and a number of utilities written by the Free Software Foundation (FSF) for their own operating system project (GNU Hurd), this operating system quickly grew into a very important third UNIX-like operating system.
    
    Adding to the importance of Linux and the GNU tools was the advent of MacBSD, FreeBSD, NetBSD, OpenBSD, and other BSD variants. Although BSD-based operating systems had many common utilities, they had no replacements for a few of the missing AT&T pieces. For this reason, many of these tools have also made their way into these BSD-based operating systems. In a similar way, BSD-derived tools frequently appear as part of Linux distributions.
    

Over the years, a number of standards have emerged to mitigate the differences in syntax between these operating systems, including POSIX and the Single Unix Specification (SUS). As operating systems work towards compliance with these specifications, many of the differences in syntax are gradually fading into irrelevance. However, for true cross-platform compatibility, you should still be aware of these differences.

OS X prior to version 10.5 provided tools that generally follow BSD semantics (or, in some cases, Linux or GNU semantics). Beginning in OS X v10.5, many of these tools instead obey AT&T semantics (most of the time; see note below for exceptions). Thus, some tools behave differently depending on the version of OS X. These differences are described in the manual pages for the individual tools.

**Note:** While tools in OS X v10.5 and later generally obey AT&T semantics, this is not always true. In particular, when executed from installer scripts or startup items, they obey BSD semantics for backwards compatibility with existing scripts.

As a convenience to script developers, you can also obtain legacy behavior from most command-line tools by setting certain environment variables as described in the `compat` manual page.

For more information on legacy-mode command support, see _[Unix 03 Conformance Release Notes](https://developer.apple.com/library/archive/releasenotes/Darwin/RN-Unix03Conformance/index.html#//apple_ref/doc/uid/TP40004772)_, the `compat` manual page, and the manual pages for individual commands.

### awk

In operating systems that follow AT&T semantics, the `awk` command supports certain forms of extended regular expressions (such as `{n,m}`, `[[==]]`, and `[[..]]`) without explicitly setting flags to enable extended regular expression support. Because this behavior is not portable, you should not depend on it.

Because of this difference, if you find a regular expression that a particular `awk` interpreter cannot handle, you should first try enabling extended regular expression support and then see if the problem goes away. This will usually break other parts of the expression, however. If so, you must rewrite the regular expression to fully use the extended regular expression syntax.

To learn about basic and extended regular expressions, read [Regular Expressions Unfettered](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/RegularExpressionsUnfettered/RegularExpressionsUnfettered.html#//apple_ref/doc/uid/TP40004268-CH238-SW7). To learn more about the `awk` interpreter, read the manual page for `awk`. To learn more about the AWK scripting language, read [How AWK-ward](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/Howawk-ward/Howawk-ward.html#//apple_ref/doc/uid/TP40004268-TP40003518-SW10).

### chown

If you pass the `-P` flag to `chown`, it does not follow symbolic links. Thus, the file that a symbolic link points to is _never_ modified if you specify the `-P` flag.

However, in operating systems that follow AT&T semantics, when you issue the command `chown -RP directory_name`, the user ID of the symbolic link itself _is_ modified. In operating systems that follow BSD semantics, the symbolic link itself is _not_ modified.

### cp

If you pass both the `-i` and `-f` flags to `cp`, the flag that takes precedence varies among operating systems. These flags specify opposite behavior, so you should never use them together.

Also, the `-f` option has different behavior depending on the operating system:

|Flags|BSD semantics|AT&T semantics|
|---|---|---|
|`-f` without `-p`|Destination file permissions unchanged.|Destination file permissions set to default permissions.|
|`-f` with `-p`|Destination file permissions set to permissions of source file.|Destination file permissions set to permissions of source file.|

Finally, in operating systems that follow AT&T semantics, when copying recursively, the copy operation stops as soon as any error occurs. In operating systems that follow BSD semantics, copy operation completes to the maximum extent possible. In either case, the command exits with a nonzero result code.

If you need to ensure that a copy operation does not stop on first failure, you can use `tar` instead. For an example of how to use `tar` to copy files, see [Anonymous Subroutines](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/SubroutinesandScoping/SubroutinesandScoping.html#//apple_ref/doc/uid/TP40004268-TP40003513-SW6).

### crontab

In AT&T-based UNIX systems, the `crontab` command reads from standard input by default, but on BSD-based systems, it does not. For cross-platform compatibility, you should specify a hyphen (`-`) for the filename instead. This works on with versions of `crontab` that obey both AT&T and BSD semantics.

### date

The result codes returned by `date` vary depending on the operating system. For cross-platform compatibility, you can only assume that a result code of zero (`0`) indicates success and any other value indicates some sort of failure.

### df

The `df` command has two different meanings for the `-t` flag beginning in OS X v10.5. They are as follows:

- If you include a value afterwards (for example, `-t hfs`), it behaves like the `-T` flag. This usage is deprecated.
    
- Without an argument, it tells `df` to print the total allocated space. Because this option is the default, this use of the `-t` flag is unnecessary.
    

The default block size varies on different operating systems. Linux and most BSD-based operating systems default to a 1k block size, while AT&T UNIX-based operating systems default to a 512-byte block size.

For consistent behavior across multiple operating systems, you should always specify a block size explicitly. For example, the -k flag specifies that the block size should be reported in kilobytes.

Finally, the capacity percentage reported by `df` may be rounded differently in different operating systems.

### dos2unix and unix2dos

Linux provides these two utilities for converting between UNIX-style and DOS-style line endings. Using these tools is not portable, and OS X does not provide these utilities.

Instead of using `dos2unix` or `unix2dos`, you should instead use `tr` or `sed` as described in [Cross-Platform Line Endings](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/PortingScriptstoMacOSX/PortingScriptstoMacOSX.html#//apple_ref/doc/uid/TP40004268-TP40003517-SW37).

### du

Operating systems that follow AT&T semantics allow you to pass a combination of the `-L`, `-H`, and `-P` options to `du`. The last flag encountered determines the command's behavior. In operating systems that follow BSD semantics, specifying more than one of these options results in an error. To fix this problem, delete all but the last of these options.

Also, many BSD-based operating systems cannot detect symbolic link loops. For cross-platform compatibility, you should generally not tell `du` to follow symbolic links unless you are certain that no cycles can occur.

### echo

Of particular interest is the difference in behavior of the `echo` builtin and the corresponding standalone command. If you want to issue a prompt, in BSD-derived operating systems you can leave off the trailing newline by typing the following:

|   |
|---|
|echo -n "Prompt: "|

In AT&T UNIX-derived operating systems, the equivalent is:

|   |
|---|
|echo "Prompt: \c"|

Unfortunately, this difference makes it very difficult to write scripts that depend on this behavior in a cross-platform way. For portability, you should avoid either of these constructions. As an alternative, you can either use the `printf` command instead of `echo` or use the `tr` command to remove the newline.

For example, the following lines both print "Prompt: “ followed by the word “newline” immediately afterward on the same line:

|   |
|---|
|echo "Prompt: " \| tr -d '\n'; echo "newline"|
|printf "Prompt: "; printf "newline\n";|

The `echo` command also varies in the way it handles control-character escape sequences such as `\r`. Because these are handled differently in different operating systems, you should avoid using them with `echo`. As an alternative, use the `printf` command to print these sequences, or store the desired control character in a shell variable using `printf` or `tr`.

For example, the following code sends an XON (Control-Q) byte to standard output:

|   |
|---|
|XON="$(echo 'x' \| tr 'x' "\\021")"|
|echo "Here is an XON: $XON"|

**Note:** The behavior of `-n`, `\c`, and other escape sequences may also vary between shell builtin versions of `echo` and the `/bin/echo` executable, depending on the operating system and the shell you are using.

### file

The `file` command has two switches that behave differently in different operating systems: `-i` and `-r` (or `--raw`). For consistent behavior, you should avoid these switches.

In AT&T UNIX-based operating systems, the `-i` option tells the `file` command to not classify the contents of regular files using the external `mime.types` file. This results in faster performance but provides less detailed analysis.

In BSD-derived operating systems, the `-i` flag tells the `file` command to output raw mime type strings rather than the more traditional human readable ones. For this behavior, you should use the `--mime` flag instead, though that option is also not supported universally.

The `-r` and `--raw` options are supported only in BSD-derived operating systems. These flags tell the file command not to translate unprintable characters to their octal representations. AT&T-derived operating systems never do this.

### grep

In some operating systems, `grep` fails silently if you try to match a caret in the middle of a line, while other versions of `grep` warn about the mistake. Such an expression is not a legal regular expression, of course, but if your script depends on getting an error in this case (or not getting an error), the script is not fully portable.

### head

The `head` command exists across most operating systems. However, different versions provide several flags that are nonstandard.

The only flag that can be used portably is the `-n` flag, which takes a line count.

Most operating systems (including OS X) also support the `-c` flag, which specifies a byte count, but this support is not guaranteed to be portable. It is possible to emulate this functionality portably with the help of an AWK script, however, as follows:

**Listing 10-3**  Emulating head -c using AWK: 01_head_c.sh

|   |
|---|
|#!/bin/sh|
||
|# Usage: ./head_c filename bytecount|
|FILENAME=$1|
|COUNT=$2|
||
|SCRIPT="$(mktemp '/tmp/head_c.XXXXXXXXXX')"|
||
|cat << EOF > "$SCRIPT"|
|BEGIN {|
|FS="";|
|my_string = ""|
|}|
|{|
|my_string = my_string "\n" \$0;|
|}|
|END {|
|# Start from character 2 to skip the bogus leading newline.|
|print substr(my_string, 2, $COUNT);|
|}|
|EOF|
||
|awk -f "$SCRIPT" "$FILENAME"|
||
|rm "$SCRIPT"|

You may also run into a minor compatibility problem when porting scripts from Linux to OS X. When you pass multiple filenames to the `head` command, it prints a heading line for each file name in the form

|   |
|---|
|==> filename <==|

The Linux version of `head` provides a `-q` flag that disables printing the header marker even if you specify multiple files. It also provides a `-v` flag that forces header printing even when only one file is specified.

As an alternative to the `-v` flag, you can output the filename marker in your script with a simple `echo` statement like this one:

|   |
|---|
|echo "==> $FILENAME <=="|

As an alternative to the `-q` flag, provided that there is no possibility of your files’ contents actually matching the pattern, you can strip out the markers with `grep` like this:

|   |
|---|
|head -n 1 file1 file2 ... \| grep -v '^==>.*<==$'|

In addition to these flag differences, POSIX specifies that the input files for `head` must be valid text files, which means that all byte sequences must be valid for the current locale. Although not all versions of `head` enforce this restriction, versions that do may fail when used with binary files in some operating systems unless you change the local settings.

If your scripts must process binary files, be sure to specify the “C” locale before executing commands that work with these binary files. To change the locale, issue the following statement:

|   |
|---|
|export LANG="C"|

### join

The `-e` option tells the `join` command to insert the specified string into empty fields. In operating systems that follow BSD semantics, substitution occurs _only_ if there are no nonempty fields after the empty field. In operating systems that follow AT&T UNIX semantics, substitution always occurs.

Not all `join` flags are supported on all operating systems. For portability, you should limit yourself to `-a`, `-e`, `-o`, `-t`, `-v`, `-1`, and `-2`.

### less

See [more or less](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/PortingScriptstoMacOSX/PortingScriptstoMacOSX.html#//apple_ref/doc/uid/TP40004268-TP40003517-SW36).

### ls

When `-H` is specified (and is not overridden by `-L` or `-P`) and a file argument is a symbolic link that resolves to a non-directory file, the output reflects the nature of the link, rather than that of the file. In operating systems that follow BSD semantics, the output describes the file.

The `-f` option turns on the `-a` option (show files whose names have a period (`.`) as the first character). In operating systems that follow BSD semantics, it does not.

The `-o` option causes the listing to be in long format, but to omit the group id. In operating systems that follow BSD semantics, the `-o` option modifies the `-l` option, causing file flags to be listed.

The `-g`, `-n`, and `-o` options turn on the `-l` option (causing the listing to be in long format). In operating systems that follow BSD semantics, they do not.

### mkfifo

In operating systems that follow BSD semantics, the `mkfifo` command applies a mask of `0666` to the mode passed in for the `-m` option. In operating systems that follow AT&T semantics, no mask is applied.

### more or less

Different operating systems handle the `-n` and `-p` flags to the `more` command differently.

In operating systems that follow the BSD and AT&T semantics, the `-n` option specifies the number of lines per screen, and the `-p` option allows you to specify commands (such as `:p`) to execute each time a new screenful of text is displayed.

In operating systems that follow Linux semantics (and for the `less` command on all operating systems), the `-n` flag tells the `more` command to to suppress line numbering, and the `-p` flag specifies a search pattern.

### mv

If you tell the `mv` command to move a subdirectory into its current parent directory (by typing `mv foo/bar foo`, for example), the behavior varies in a subtle way. No action occurs in any operating system because you are effectively moving a directory on top of itself. However, operating systems that follow BSD semantics exit with a zero (success) result code, whereas operating systems that follow AT&T semantics display an error message and exit with a nonzero (failure) result code.

### pr

In AT&T UNIX semantics, the last space before the tab stop is replaced with a tab character. This replacement does not occur in most open source (BSD or Linux) implementations. For cross-platform consistency, you can globally replace the tab with a space by piping the output to `tr` with appropriate arguments. For example:

|   |
|---|
|pr [arguments...] \| tr '\t' ' '|

### ps

While not frequently used in shell scripts, the `ps` command behaves very differently between operating systems that follow BSD and AT&T semantics. The differences are summarized in the following table:

|Flag|AT&T|BSD|
|---|---|---|
|`-e`|Display information about other users’ processes, including those without controlling terminals; same as `-A`.|Display the environment variable settings for each process; same as `-E`.|
|`-g`|Display information about processes with the specified session leaders.|Unused option.|
|`-l`|"Long” display format; includes the `paddr` field.|"Long” display format; does not include the `paddr` field.|
|`-u`|Display processes belonging to a particular user. For example, `ps -u root` displays all processes belonging to the root user.|Display the fields `user`, `pid`, `%cpu`, `%mem`, `vsz`, `rss`, `tt`, `state`, `start`, `time` and `command`. Also implies the `-r` option (sort by CPU usage).|

**Note:** For the most part, the information available from `ps` is similar in all variants (with the exception of the `-u` flag). The headings themselves, however, differ somewhat among BSD, AT&T, and Linux variants of the `ps` command. Similarly, column order is not guaranteed to be consistent across platforms. For this reason, programmatic use of ps is generally discouraged.

Most BSD and Linux variants have deprecated the use of BSD variants of flags when they are preceded by a dash. Passing these flags without a dash in these operating systems will generate the BSD behavior more consistently (at least on BSD and Linux-based operating systems). However, because this behavior is not portable, you should generally not depend on the specific quirks of a particular `ps` implementation.

### rename

The `rename` command is a command that exists on some Linux distributions. To add further confusion, there are two separate commands that have this name, depending on the distribution, and the syntax for the two commands is completely different:

- In some Linux distributions, `rename` is a command from the util-linux-ng package, found at [http://userweb.kernel.org/~kzak/util-linux-ng/](http://userweb.kernel.org/~kzak/util-linux-ng/).
    
- In other Linux distributions, `rename` is a Perl script, also known in various incarnations as `prename` or `perl-rename` that ships as part of the Perl distribution. This script is available from [CPAN](http://www.cpan.org/).
    

Because the use of the `rename` tool is not portable even across Linux distributions, you should generally use the `find` command, if possible.

If `find` is insufficient, you can easily install the Perl `rename` command using the `cpan` tool. To do this, first log in in as an admin user, then run Terminal, then type:

|   |
|---|
|sudo cpan File::Rename|

The `sudo` command then asks you to enter your admin password.

Once the `File::Rename` CPAN package is installed, the rename command is in `/usr/local/bin`.

Be sure to document this nonstandard dependency appropriately in your script, along with an explanation of how to install the module.

### sed

Different versions of `sed` use different flags for enabling extended regular expressions. GNU `sed` (commonly used in Linux) uses the `-r` flag. BSD versions of `sed` (including the OS X version) use the `-E` flag. If your script must run on both platforms, you must test for compatibility first. For example:

|   |
|---|
|STRING="$(echo 'xy' \| sed -E 's/(x)y/\1/' 2> /dev/null)"|
|if [ "$STRING" = "x" ] ; then|
|SEDERE="-E"|
|else|
|SEDERE="-r"|
|fi|
|...|
|sed $SEDERE ...|

In addition, most GNU versions of `sed` generate warnings for unused labels. Most other implementations do not.

Also, when the `y` function is specified (for example, `sed y/string1/string2/`), most GNU versions convert double backslashes to single backslashes. This behavior is not portable, so you should not depend on it.

Because of this incompatibility, if you need to construct an expression containing user-entered strings that could potentially include a backslash, you should avoid the problem entirely by using the `s` function (for example, `sed s/string1/string2/`) instead of the `y` function.

### sort

The form `sort +POS1 -POS2 ...` is a syntax specific to the GNU version of `sort` and is considered obsolete. This syntax is not portable and is not supported in OS X beginning in version 10.5.

For example:

|   |
|---|
|$ cat data|
|b  a|
|a  b|
||
|$ sort data|
|a  b|
|b  a|
||
|$ sort +1 -2 data|
|sort: invalid option -- 2|
|Try `sort --help' for more information.|

Instead, you should use the `-k` flag to do the same thing. For example:

|   |
|---|
|$ sort -k 2,3 data|
|b  a|
|a  b|

**Note:** The field and character positions are numbered differently with this syntax. Numbering for the `-k` syntax starts at one (`1`), while the obsolete plus and minus syntax starts at zero (`0`).

**Compatibility Note:** OS X v10.5 and later does not support this legacy GNU sort syntax. However, as a temporary workaround while you rewrite the offending scripts, you can set the `_POSIX2_VERSION` environment variable as show in the following snippet:

|   |
|---|
|export _POSIX2_VERSION=200111|
|# or in CSH|
|setenv _POSIX2_VERSION 200111|

Do not rely on this workaround for production code; its continued support is not guaranteed.

For more information on compatibility issues with the `sort` command, see the manual page for `sort`.

### stty

Prior to OS X v10.5, the `stty` command did not support the following control modes:

- `bs0` and `bs1`
    
- `cr0`, `cr1`, `cr2`, and `cr3`
    
- `ff0` and `ff1`
    
- `nl0` and `nl1`
    
- `tab0`, `tab1`, `tab2`, and `tab3`
    
- `vt0` and `vt1`
    

In addition, prior to OS X v10.5, `stty` did not support the following options:

- `ocrnl` and `-ocrnl`
    
- `ofdel` and `-ofdel`
    
- `ofill` and `-ofill`
    
- `onlret` and `-onlret`
    
- `onocr` and `-onocr`
    

In legacy mode, these modes and options are still not accepted. For more information, see the manual page for `stty`.

### tail

The `tail` command differs significantly between Linux and OS X. The GNU variant of `tail` provides options that the OS X version does not and vice versa. Both provide features that are not part of the POSIX specification, and thus may not be portable.

According to the POSIX specification, the following flags are portable: `-f` (continue to wait for the file to grow or for the FIFO to provide additional data), `-c` (byte count), and `-n` (line count).

Further, POSIX only explicitly requires the `tail` command to accept a single filename as an argument. Any use with multiple files is inherently not portable.

`-b` (OS X)

OS X provides a `-b` flag that allows you to specify a location in 512-byte block increments. For maximum portability, multiply the number by 512 yourself and use the `-c` flag instead.

`-F` (OS X and Linux)

Both Linux and OS X provide a `-F` flag that is equivalent to `-f --retry`. This is easily avoided with the workarounds described as part of the entries for the individual `--follow` and `--retry` flags.

`--follow` (Linux)

Linux also provides a `--follow` flag, which is equivalent to `-f` except when used with file descriptors.

When working with files, use `-f` instead.

The file descriptor syntax is not portable and is not supported except in Linux. Use a named pipe (FIFO) instead.

`--max-unchanged-stat` (Linux)

Linux provides a `--max-unchanged-stat` that tries reopening a file if you are using the `-f` flag and the file hasn’t changed in a while. This allows it to handle the case here the file is renamed and a new file with the same name is created as often happens with log files. There is no easy portable replacement for this feature.

`--pid` (Linux)

Linux provides a `--pid` flag that terminates the `tail` command after the specified process ID dies.

There is no easy portable replacement for this feature, though it could be replaced in a not-so-portable fashion by a script running as a background job that uses the `ps` command to verify the existence of the process.

Assuming the process being watched was originally started by the shell script in the background, it could also be replaced by running the `tail` command in the background and using the `wait` shell builtin to wait for the process ID to exit, then killing the `tail` command. For more information, see [Background Jobs and Job Control](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/AdvancedTechniques/AdvancedTechniques.html#//apple_ref/doc/uid/TP40004268-TP40003521-SW61).

`-q` (Linux)

As with the `head` command, Linux provides `-v` and `-q` flags. See [head](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/PortingScriptstoMacOSX/PortingScriptstoMacOSX.html#//apple_ref/doc/uid/TP40004268-TP40003517-SW35) earlier in this section for explanation of these flags and suggested alternatives.

`-r` (OS X)

OS X provides a `-r` flag that reverses the order of the lines printed. It also changes the behavior of the leading plus (`+`) and minus (`-`) symbols when passed as part of arguments to the `-b`, `-c`, and `-n` flags.

It is possible to write an AWK script to emulate this behavior by pushing each line in the input file into an array, then printing the lines in reverse order and either skipping a given number of entries in the array to skip lines or using `substr` call to skip a given number of bytes. The [head](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/PortingScriptstoMacOSX/PortingScriptstoMacOSX.html#//apple_ref/doc/uid/TP40004268-TP40003517-SW35) section of this chapter provides an example of how to emulate `head -c` using an AWK script; this example provides a good starting point for writing a script that emulates this `tail` feature.

`--retry` (Linux)

Linux provides a `--retry` flag to keep trying to open a file if it does not exists.

This is commonly used, with the `-f` flag, and in that usage, is equivalent to the `-F` flag, which OS X supports.

By itself, however, OS X has no equivalent flag, though you can trivially approximate it in a more portable fashion by writing a while loop in a shell script that repeatedly checks for the file until it finds it, then runs the `tail` command.

`-s` and `--sleep-interval` (Linux)

Linux provides `-s` and `--sleep-interval` flags to lower CPU use by adding a delay between checks to see if a file you are watching with `-f` has grown.

`-v` (Linux)

As with the `head` command, Linux provides `-v` and `-q` flags. See [head](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/PortingScriptstoMacOSX/PortingScriptstoMacOSX.html#//apple_ref/doc/uid/TP40004268-TP40003517-SW35) earlier in this section for explanation of these flags and suggested alternatives.

In addition to these flag differences, POSIX specifies that the input files for `tail` must be valid text files, which means that all byte sequences must be valid for the current locale. Although not all versions of `tail` enforce this restriction, versions that do may fail when used with binary files in some operating systems unless you change the local settings.

If your scripts must process binary files, be sure to specify the “C” locale before executing commands that work with these binary files. To change the locale, issue the following statement:

|   |
|---|
|export LANG="C"|

Finally, unlike the `head` command, POSIX does not require that the `tail` command be able to store and print a text block of arbitrary length. It requires only that the buffer size be at least 10 times the value of `LINE_MAX`. The value of `LINE_MAX` is implementation dependent, but must be at least 2048 bytes.

While this theoretical 20,480 byte limit in the output of the `tail` command is not commonly enforced in modern operating systems, the only _guaranteed_ portable way to generate larger results from `tail` is to use another tool such as AWK.

### uudecode, uuencode

In most Linux and BSD-derived operating systems, `uudecode` applies a mask of `0666` to file modes, thus preventing the creation of executable files (or files with other special modes). In operating systems that follow AT&T semantics, no mask is applied.

For consistency, if you require the results of `uudecode` to be executable or have nonstandard modes, your script should set the execute flag explicitly with `chmod`.

In operating systems that follow AT&T semantics, if `uudecode` overwrites an existing file, it cannot necessarily change its mode unless the file is owned by the current user or `uudecode` is running as the root user.

### which

In OS X, the `which` command can take the -s flag for “silent” behavior. In this mode, it does not output any text and returns an exit status of `0` if the command exists in any of the paths listed in the `PATH` environment variable or `1` if it does not (or `2` if you pass an invalid flag).

This flag does not exist in many operating systems that obey AT&T semantics. The GNU version of `which` used in Linux also does not support this flag. As an alternative, you can redirect the output of `which` to `/dev/null` as described in [Pipes and Redirection](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/ShellInputandOutput/ShellInputandOutput.html#//apple_ref/doc/uid/TP40004268-CH3-SW9).

Also, some (not all) Linux distributions come with the GNU `which` command. This command differs significantly in its behavior from other UNIX-like operating systems. In order to support searching for multiple commands in a single `which` statement, its exit status contains the number of commands that were not found, or -1 if you pass it unknown flags. (It also supports a number of formatting flags that are not broadly available.)

For reliable cross-platform use, you should specify exactly one command argument at a time, pass no flags (except the ubiquitous `-a` flag, if desired), and assume that an exit status of either `-1` or `2` indicates a usage error.

### who

In operating systems that follow AT&T semantics, if you use the `-u` flag, the `who` command displays the process ID of the corresponding `login` process. In operating systems that follow BSD semantics, it does not display the process ID.

**Compatibility Note:** You can get the BSD semantics in OS X v10.5 by enabling legacy mode as described in the `compat` manual page.

### xargs

If you pass the `-L` flag to the `xargs` command, `xargs` calls the specified utility every time a certain number of lines are read. However, some details differ slightly:

- **Counting:** In operating systems that follow BSD semantics, the number of lines is based on the number of newlines encountered. Every line (including blank lines) is counted. In operating systems that follow AT&T UNIX semantics, blank lines are ignored for counting purposes.
    
- **Concatenation:** In operating systems that follow AT&T UNIX semantics, any line ending with a space is combined with the lines that follow it, up to and including the first nonblank line. This concatenation does not occur in operating systems that follow BSD semantics.
    
- **Combining Options:** In operating systems that follow BSD semantics, the `-L` and `-n` options can be used together. In operating systems that follow AT&T UNIX semantics, the `-L` and `-n` options are mutually exclusive, and the last one given on the command line will be used.
    

[Next](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/AdvancedTechniques/AdvancedTechniques.html)[Previous](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/Howawk-ward/Howawk-ward.html)

  

  

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