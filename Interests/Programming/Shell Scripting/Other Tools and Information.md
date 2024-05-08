[Documentation Archive](https://developer.apple.com/library/archive/navigation/) [Developer](https://developer.apple.com/)

Search

# Shell Scripting Primer

[Companion File](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/scripts.zip "Download Companion File")

[Next](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/StartingPoints/StartingPoints.html)[Previous](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/SpecialShellVariables/SpecialShellVariables.html)

# Other Tools and Information

The final piece to understanding shell scripting (and to understanding other people’s shell scripts) is comprehending the use (and abuse) of command-line tools. The scripts listed in this section are commonly used in shell scripts.

Each of these tools has its own syntax and its own quirks. It is impractical to explain them all in detail. However, this chapter briefly highlights some common tools and includes links to their manual pages for finding additional information about them.

## General Tools

The tools in this section are general tools that don’t fit into any broad categories.

|   |   |
|---|---|
**Table C-1**  Commonly used general scripting tools
|Tool|Description|
|`bc`|Short for “basic calculator”, performs floating point math and various other useful calculations that are not practical with basic shell math support.|
|`expect`|Used to work with hard-to-handle command-line tools that require more complex interaction than is possible with a single pipe. For example, you could use an `expect` script to interact with `getty` over a `tty` or other bidirectional connection to log into a remote computer. In general, scripting that requires two-way interaction between the script and a program is most easily done with an `expect` script.|
|`expr`|Evaluates a numerical expression. This command supports basic integer math, and is frequently used for incrementing a loop iterator.|
|`false`|Returns a failure exit status (nonzero).|
|`sleep`|Pauses execution for a period of time (measured in seconds).|
|`true`|Returns a successful exit status (`0`).|

## Text Processing Tools

The tools listed in this section are commonly used for text processing. Unless otherwise noted, these commands take input from standard input (if applicable) and print the result to standard output.

Many of these commands use regular expressions. The syntax of regular expressions is described in [Regular Expressions Unfettered](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/RegularExpressionsUnfettered/RegularExpressionsUnfettered.html#//apple_ref/doc/uid/TP40004268-CH238-SW7). For additional usage notes specific to individual applications, see the manual page for the command itself.

|   |   |
|---|---|
**Table C-2**  Commonly used text processing tools
|Tool|Description|
|`awk`|Short for Aho, Weinberger, and Kernighan; a programming language in itself, used for text processing using regular expressions. This tool is described further in [How AWK-ward](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/Howawk-ward/Howawk-ward.html#//apple_ref/doc/uid/TP40004268-TP40003518-SW10).|
|`grep`|Short for Global [search for] Regular Expressions and Print; prints lines matching an input pattern (optionally with a specified number of lines of leading and/or trailing context). The `grep` command can take input from standard input or from files.<br><br>Common variants include `` `[agrep](ftp://ftp.cs.arizona.edu/agrep/)` `` (“approximate grep” from the Univ. of AZ), `fgrep`, and `egrep`.|
|`head`|Prints the first few lines from a file (or standard input). The number of lines can be specified with the `-n` flag.|
|`perl`|A programming language whose scripts can be easily embedded in shell scripts using the `-e` flag. Perl's regular expression language is somewhat richer than basic regular expressions (and easier to read than character classes in extended regular expressions), making it popular for text processing use.|
|`sed`|Short for stream editor; performs more complex text substitutions using regular expressions.|
|`sort`|Sorts a series of lines. By default, `sort` reads these lines from its standard input. After its standard input is closed, it sorts them and prints the results to its standard output.|
|`tail`|Prints the last few lines from of a file (or standard input). The number of lines can be specified with the `-n` flag. Alternatively, you can specify the starting position as a byte or line offset from either the start or end of the file.|
|`tee`|Copies standard input to standard output, saving a copy into a file (or multiple files).|
|`tr`|Replaces one character with another.|
|`uniq`|Filters out adjacent lines that match.|

## File Commands

These commands are used to manipulate files, including renaming, moving, and deleting files, changing permissions, creating directories, listing files, and so on.

|   |   |
|---|---|
**Table C-3**  Commonly used file manipulation tools
|Tool|Description|
|`cd`|Changes the current working directory. The command `cd ..` moves up a directory, for example.|
|`chflags`|Changes flags on a file or directory. Most of these flags are relatively obscure. For changing permissions flags, use `chmod` instead.|
|`chgrp`|Changes the group ID associated with a file or directory.|
|`chmod`|Changes modes (permission bits) or access control lists (ACLs) on a file or directory.|
|`chown`|Changes the ownership of files or directories. This command can also change the group if desired.|
|`find`|Lists or searches for files in a directory and its subdirectories.|
|`ln`|Creates symbolic links and hard links to files or directories.|
|`ls`|Lists the files in the current directory.|
|`mkdir`|Creates new directories.|
|`mkfifo`|Creates named pipes for communication. This tool is useful in situations where pipes cannot be established while executing the commands, such as connecting two tools in a circular fashion.|
|`mv`|Moves or renames files and directories.|
|`rm` and `rmdir`|Removes files and directories|
|`stat`|Prints detailed file status information, such as the type of file, last modification date, and so on.|
|`GetFileInfo` and `SetFile`|These tools, installed as part of the Developer Tools installation, are useful for getting and manipulating things like extended attributes.<br><br>Be aware that if you write a script that depends on these, it will require the Developer Tools to be installed.|

## Disk Commands

The tools listed in this section perform operations on disks, file systems, partition tables, and disk images.

|   |   |
|---|---|
**Table C-4**  Commonly used disk-related and partition-related tools
|Tool|Description|
|`diskutil`|Mounts and unmounts volumes and disks, checks disks for consistency, erases optical disks, wipes disks with a security wipe, partitions disks, manipulates RAID sets, and so on.<br><br>This utility is the command-line counterpart to the Disk Utility application.|
|`fsck`, `fsck_msdos`, `fsck_hfs`|Checks a file system for consistency.|
|`hdiutil`|Creates and manipulates disk images, including attaching disk images for mounting.|
|`mount` and `umount`<br><br>(Also `mount_afp`, `mount_cd9660`, `mount_cddafs`, `mount_fdesc`, `mount_ftp`, `mount_hfs`, `mount_msdos`, `mount_nfs`, `mount_ntfs`, `mount_smbfs`, `mount_udf`, `mount_url`, and `mount_webdav`)|Mounts and unmounts volumes.<br><br>If you unmount automounted volumes behind the back of the disk arbitration system, you can cause strange behavior in the GUI. Use these commands with care, and if you are trying to unmount an automounted volume, use `hdiutil` or `diskutil` instead.|

## Archiving and Compression Commands

The tools in this section allow you to create archive files that contain copies of multiple files for ease of distribution, to extract the contents of archive files, and compress and decompress files to reduce disk space or network utilization.

The compression tools can also generally be used with pipes to compress data without storing it in a file. The archive tools can generally use standard input or output for reading or writing the archive itself, but not the contents thereof. The `funzip` variant of the zip archiving tool can be used with two pipes, but can only extract the first file from an archive.

|   |   |
|---|---|
**Table C-5**  Commonly used archiving and compression tools
|Tool|Description|
|`bzip2`, `bunzip2`, and `bzip2recover`|Compresses and decompresses files using the Burrows-Wheeler block sorting text compression algorithm and Huffman coding. This compression tool takes somewhat longer than other tools such as `gzip`, but tends to result in smaller files, and is thus growing in popularity for distributing large files.<br><br>Files created with this tool end with the `.bz2` extension.|
|`compress` and `uncompress`|Compresses and decompresses files using the Lempel-Ziv-Welsh (LZW) compression algorithm. This compression format has largely fallen out of popularity.<br><br>Files created by this tool end with the `.Z` extension.|
|`gzip`, `gunzip`, `zcat`, and `gzcat`|Compresses, uncompresses, and prints the contents of files in the GNU Zip (LZ77-based) format. This compression scheme is popular with UNIX and Linux users.<br><br>While based on the same underlying compression scheme, the GNU Zip and ZIP file formats are not the same. The ZIP file format can contain multiple files, while the Gzip file format can only contain a single file (though this single file may be a `tar` archive).<br><br>Files created by this tool end with the `.gz` extension.|
|`zip`, `unzip`, and `funzip`|Compresses and uncompresses files and directories using the ZIP file format (deflate, based on LZ77 and Huffman coding). This file format is commonly used for exchanging compressed files with Windows users.<br><br>Files created by this tool end with the `.zip` extension.|
|`tar`|Creates, appends to, and extracts multifile archives in the `tar` (short for “Tape ARchive”) format. This format is the standard format for storing multiple files in a single archive among UNIX and Linux users. The tar file format is usually seen in a compressed form, using either `gzip` or `bzip2`.<br><br>Files created by this tool end with the `.tar` extension (or the `.tgz` or `.tbz` extensions for tar archives compressed with `gzip` or `bzip2`).|

## For More Information

There are a nearly unlimited number of tools that you might find useful when writing shell scripts. These are just a few of the more common ones. You can find out about the command-line tools that ship as part of OS X by looking in the man pages, either online (_OS X Man Pages_) or by using the `man` command on the command line.

For help finding a command to perform a particular task, you can either search the online version of the man pages or use the `apropos` command on the command line.

Happy scripting!

[Next](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/StartingPoints/StartingPoints.html)[Previous](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/SpecialShellVariables/SpecialShellVariables.html)

  

  

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