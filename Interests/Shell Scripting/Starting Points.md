[Documentation Archive](https://developer.apple.com/library/archive/navigation/) [Developer](https://developer.apple.com/)

Search

# Shell Scripting Primer

[Companion File](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/scripts.zip "Download Companion File")

[Next](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/AnExtremeExample/AnExtremeExample.html)[Previous](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/ForMoreInformation/ForMoreInformation.html)

# Starting Points

This appendix provides a number of short script snippets that simplify common tasks and provides links to a few other scripts in other chapters.

## Files and Directories

### Copying Files and Directories

The first script demonstrates how to copy a folder full of files and folders to a different location using `cp`.

**Warning:** Warning: Do not put a slash at the end of the name of `folder_to_copy`. In some operating systems, this causes the contents of `folder_to_copy` to be copied into `destination_directory` instead of the whole folder.

**Listing D-1**  Copying a folder recursively

|   |
|---|
|cp -R -p folder_to_copy destination_directory|

The next script shows how to copy a tree of files and folders, preserving the source directory structure using `tar`. For example, this results in `destination/file1`, `destination/dir2/file2`, and so on.

**Listing D-2**  Copying multiple files and directories to another location, preserving the directory structure

|   |
|---|
|tar -czf - file1 dir2/file2 dir3/file3 \| \|
|{ cd /destination ; tar -xzf - ; }|

The next two scripts show how to copy entire trees of files from one server to another securely using `tar` and `ssh`.

**Listing D-3**  Copying a tree of files and folders from the current directory to a remote computer

|   |
|---|
|# Copies _directory_or_file_name_ on the local machine|
|# to _/path/to/destination/directory_or_file_name_ on|
|# a remote machine.|
||
|tar -czf - directory_or_file_name \| ssh username@hostname \|
|"cd /path/to/destination; tar -xzf -"|

**Listing D-4**  Copying a tree of files and folders from a remote computer to the current directory

|   |
|---|
|# Copies the directory called _directory_name_ from|
|# _/path/to/source/directory_name_ on a remote server|
|# to the current directory on the local machine.|
||
|ssh username@hostname "cd /path/to/source; \|
|tar -czf - directory_name" \| tar -xzf -|

The following script recovers from a failed `tar` copy. Normally, you would just use `rsync`, but occasionally you may have to copy lots of files to or from an ISP that disallows `rsync` and sets an unreasonably low maximum CPU time for executables, causing `tar` to die repeatedly.

**Note:** This script uses the `stat` command-line tool, which uses completely nonstandard flags across different operating systems. The variables `LOCALFORMATFLAG`, `LOCALFORMAT`, `REMOTEFORMATFLAG`, and `REMOTEFORMAT` must be adjusted for the operating system on the local and remote systems, respectively. The examples given cover OS X and Linux. See the manual page for `stat` on each machine to determine the correct flags. The format string should contain the path of the file, followed by a space, followed by the length of the file (in bytes).

**Listing D-5**  Code to recover from a truncated tar copy

|   |
|---|
|#!/bin/sh|
||
|USERNAME="remoteuser"|
|REMOTEHOST="remotehost.example.org"|
|SRCDIR="/path/to/testdir"|
|OUTDIR="/remote/path/here"|
||
|# Format is "path bytecount"|
|LOCALFORMATFLAG="-f"  # OS X|
|LOCALFORMAT="%N %z"   # OS X|
|REMOTEFORMATFLAG="-c" # Linux|
|REMOTEFORMAT="%n %s"  # Linux|
||
|OUTDIRQUOTED="$(echo "$OUTDIR" \| sed 's/"/\\"/g')"|
||
|IFS="|
|"|
||
|BACKUPLIST=""|
||
|cd "$SRCDIR"|
||
|# Generate a list of files and their length in bytes on the local|
|# and local machines.|
|LOCALFILELIST="$(cd "$SRCDIR" ; find . -exec stat "$LOCALFORMATFLAG" \|
|"$LOCALFORMAT" {}  \; \| sort)"|
|REMOTEFILELIST="$(ssh $USERNAME@$REMOTEHOST "cd \"$OUTDIRQUOTED\" ; \|
|find . -exec stat "$REMOTEFORMATFLAG" '$REMOTEFORMAT' {}  \; \| sort")"|
||
|# echo "RFL: $REMOTEFILELIST"|
||
|# Loop until there are no more local files to check.|
|while true ; do|
|LNFILES="$(echo "$LOCALFILELIST" \| grep -c .)"|
|LNFM1="$(expr "$LNFILES" '-' '1')"|
|RNFILES="$(echo "$REMOTEFILELIST" \| grep -c .)"|
|RNFM1="$(expr "$RNFILES" '-' '1')"|
||
|# echo "@TOP LNFM1: $LNFM1 RNFM1 $RNFM1"|
||
|# If there are no more local files, break out of the outer loop.|
|# Otherwise, pop the first filename from the list.|
|if [ $LNFM1 -lt 0 ] ; then|
|break;|
|else|
|LOCALLINE="$(echo "$LOCALFILELIST" \| head -n 1)"|
|LOCALFILE="$(echo "$LOCALLINE" \| sed 's/ [0-9][0-9]*$//')"|
|LOCALQUOTED="$(echo "$LOCALFILE" \| sed 's/"/\\"/g')"|
|LOCALLENGTH="$(echo "$LOCALLINE" \| \sed 's/.* \([0-9][0-9]*\)$/\1/')"|
|LOCALFILELIST="$(echo "$LOCALFILELIST" \| tail -n $LNFM1)"|
|fi|
||
|# If there are no more remote files, every local file must|
|# be added to the list of files to copy.|
|# Otherwise, pop the first filename from the list.|
|if [ $RNFM1 -lt 0 ] ; then|
|REMOTELINE=""|
|REMOTEFILE=""|
|REMOTELENGTH=0|
|REMOTEFILELIST=""|
|else|
|REMOTELINE="$(echo "$REMOTEFILELIST" \| head -n 1)"|
|REMOTEFILE="$(echo "$REMOTELINE" \| sed 's/ [0-9][0-9]*$//')"|
|REMOTELENGTH="$(echo "$REMOTELINE" \| sed 's/.* \([0-9][0-9]*\)$/\1/')"|
|REMOTEFILELIST="$(echo "$REMOTEFILELIST" \| tail -n $RNFM1)"|
|fi|
||
|# echo "OLOOP LOCALFILE: $LOCALFILE REMOTEFILE: $REMOTEFILE"|
|# echo "LOCALFILELIST: $LOCALFILELIST"|
|# echo "REMOTEFILELIST: $REMOTEFILELIST"|
||
|# If the filenames do not match, then the local file does|
|# not exist on the remote server (because the lists are sorted).|
|if [ "$LOCALFILE" != "$REMOTEFILE" ] ; then|
||
|# Until they do match, keep adding files to the list of stuff to copy.|
|while [ "$LOCALFILE" != "$REMOTEFILE" -a "$LOCALFILE" != "" ] ; do|
|# echo "NOMATCHLOOP LOCALFILE: $LOCALFILE REMOTEFILE: $REMOTEFILE"|
||
|# echo "ADDED \"$LOCALQUOTED\" TO BACKUP LIST"|
||
|BACKUPLIST="$BACKUPLIST \"$LOCALQUOTED\""|
||
|# If it is a directory, adding the directory to the archive|
|# adds everything in it, so skip everything in it.|
|if [ -d "$LOCALFILE" ] ; then|
|# echo "ISDIR"|
||
|DIRLOOP=1|
|LList2="$LOCALFILELIST"|
||
|# Loop until we run out of files or the names do not match.|
|while [ $DIRLOOP = 1 ] ; do|
|LOCALFILE="$(echo "$LOCALFILE" \| sed 's/\/$//')"|
|LOCALQUOTED="$(echo "$LOCALFILE" \| sed 's/"/\\"/g')"|
||
|LNFILES2="$(echo "$LList2" \| grep -c .)"|
|LNFM1_2="$(expr "$LNFILES2" '-' '1')"|
||
|# echo "LList2: $LList2"|
|if [ $LNFM1_2 -lt 0 ] ; then|
|# We ran out of files, so stop looking for files in|
|# the directory.|
||
|LLine2=""|
|LF2=""|
|LLen2=0|
|LList2=""|
|DIRLOOP=0|
|else|
|# Grab the next file in the list.|
|LLine2="$(echo "$LList2" \| head -n 1)"|
|LF2="$(echo "$LLine2" \| sed 's/ [0-9][0-9]*$//')"|
|LLen2="$(echo "$LLine2" \| \|
|sed 's/.* \([0-9][0-9]*\)$/\1/')"|
|LList2="$(echo "$LList2" \| tail -n $LNFM1_2)"|
||
|# echo "INDIRLOOP: FILE IS $LF2"|
||
|# Repeatedly strip off the last part of the path|
|# until it matches or the path is empty.|
|INDIR="NO"|
|while [ "$LF2" != "" -a "$LF2" != "." ] ; do|
|# echo "LF2: \"$LF2\""|
|LF2="$(dirname "$LF2" \| sed 's/\/$//')";|
|if [ "$LF2" = "$LOCALFILE" ] ; then|
|# It matches.  The file is in the directory.|
|INDIR="YES"; LF2="";|
|fi|
|done|
|if [ $INDIR = "YES" ] ; then|
|# Because this file is in the directory, commit|
|# the changes to the local file list (thus|
|# removing this file from the list).|
||
|# echo "INDIR"|
|LOCALFILELIST="$LList2"|
|else|
|# This file is not in the directory.  Don't take it|
|# off the list, and stop looking for files in the|
|# directory.|
||
|# echo "NOTINDIR"|
|DIRLOOP=0|
|fi|
|fi|
|done|
||
|# Recount the number of files in the local list because it may|
|# have changed significantly.|
|LNFILES="$(echo "$LOCALFILELIST" \| grep -c .)"|
|LNFM1="$(expr "$LNFILES" '-' '1')"|
||
|else|
|# It is not a directory.  Pop the file from the list.|
||
|# echo "@BOTTOM LOCALFILELIST: $LOCALFILELIST"|
||
|# Recount the number of files in the local list.|
|LNFILES="$(echo "$LOCALFILELIST" \| grep -c .)"|
|LNFM1="$(expr "$LNFILES" '-' '1')"|
||
|# echo "@BOTTOM LNFM1: $LNFM1 RNFM1 $RNFM1"|
||
|# Grab the next file.  This is the middle loop iterator|
|# testing to see if the filename matches.|
|if [ $LNFM1 -lt 0 ] ; then|
|LOCALLINE=""|
|LOCALFILE=""|
|LOCALQUOTED=""|
|LOCALLENGTH=0|
|LOCALFILELIST=""|
|else|
|LOCALLINE="$(echo "$LOCALFILELIST" \| head -n 1)"|
|LOCALFILE="$(echo "$LOCALLINE" \| sed 's/ [0-9][0-9]*$//')"|
|LOCALQUOTED="$(echo "$LOCALFILE" \| sed 's/"/\\"/g')"|
|LOCALLENGTH="$(echo "$LOCALLINE" \| \|
|sed 's/.* \([0-9][0-9]*\)$/\1/')"|
|LOCALFILELIST="$(echo "$LOCALFILELIST" \| tail -n $LNFM1)"|
|fi|
||
|fi|
|done|
|fi|
||
|# When the script reaches this point,|
|if [ "$LOCALFILE" = "$REMOTEFILE" -a "$LOCALFILE" != "" \|
|-a $LOCALLENGTH != $REMOTELENGTH ] ; then|
|if [ ! -d "$LOCALFILE" ] ; then|
|# echo "ADDED \"$LOCALQUOTED\" TO BACKUP LIST"|
|BACKUPLIST="$BACKUPLIST \"$LOCALQUOTED\""|
|fi|
|fi|
|done|
||
|echo "BACKUPLIST $BACKUPLIST"|
||
|if [ "$BACKUPLIST" != "" ] ; then|
|eval tar -czf - $BACKUPLIST  \| ssh $USERNAME@$REMOTEHOST \|
|"cd \"$OUTDIRQUOTED\" ; tar -xzf -"|
|fi|

### Renaming Files

The following example shows how to standardize the case of the file extension on image files.

|   |
|---|
|find photo_directory -iname '*.jpg' -exec \|
|mv {} `echo {} \| sed 's/\.[jJ][pP][gG]$/.jpg/'` \;|

### Converting File Line Endings

[Listing 10-1](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/PortingScriptstoMacOSX/PortingScriptstoMacOSX.html#//apple_ref/doc/uid/TP40004268-TP40003517-SW47) and [Listing 10-2](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/PortingScriptstoMacOSX/PortingScriptstoMacOSX.html#//apple_ref/doc/uid/TP40004268-TP40003517-SW48) show how to convert between the line ending formats used for text files on various platforms.

## Image Manipulation

In [Advanced Techniques](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/AdvancedTechniques/AdvancedTechniques.html#//apple_ref/doc/uid/TP40004268-TP40003521-SW35), [Listing 11-13](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/AdvancedTechniques/AdvancedTechniques.html#//apple_ref/doc/uid/TP40004268-TP40003521-SW48) shows how to resize an image using `osascript`.

In addition to the `osascript` interface, OS X includes the `sips` command, which provides a direct shell interface to some of the image processing features in OS X.

The following snippet shows how to use `sips` to scale an image to a maximum of 250 pixels horizontally or vertically and convert the image to JPEG format.

|   |
|---|
|sips -s format jpeg --resampleHeightWidthMax 250 myphoto.tif --out mythumb.jpg|

You can also combine `sips` with `exiftool` (available from [http://www.sno.phy.queensu.ca/~phil/exiftool/](http://www.sno.phy.queensu.ca/~phil/exiftool/)) for even greater power and control. The following script uses `sips` and `exiftool` to automatically rotate a photograph based on the encoded orientation information, and allows you to specify an offset (in 90 degree increments) to adjust the rotation further.

**Listing D-6**  Rotating an image using sips

|   |
|---|
|#!/bin/sh|
||
|# Adjust paths as needed|
|EXIFTOOL=/usr/local/bin/exiftool|
|SIPS=/usr/bin/sips|
||
|INPUTFILE="$1"|
|OUTPUTFILE="$2"|
|OFFSET="$3"|
||
|# If the user doesn't specify an offset, assume zero.|
|if [ "$OFFSET" = "" ] ; then|
|OFFSET=0|
|fi|
||
|# Use exiftool to read the EXIF orientation tag as a raw numeric value.|
|ORIENTATION="$($EXIFTOOL -b -Orientation $INPUTFILE)"|
||
|# If no orientation tag is found, assume no rotation is needed.|
|if [ "$ORIENTATION" = "" ] ; then|
|ORIENTATION=1|
|fi|
||
|# This table determines the rotation (in 90 degree increments)|
|# based on the EXIF orientation tag and determines whether a|
|# coordinate transformation is needed.|
|case $ORIENTATION in|
|(1)    ROT=0; FLIP=0;; # No rotation or flip needed.|
|(2)    ROT=0; FLIP=1;; # Flip horizontal.|
|(3)    ROT=2; FLIP=0;; # Rotate 180, no flip.|
|(4)    ROT=2; FLIP=1;; # Rotate 180, flip.|
|(5)    ROT=3; FLIP=1;; # Rotate 270, flip.|
|(6)    ROT=1; FLIP=0;; # Rotate 90, no flip.|
|(7)    ROT=1; FLIP=1;; # Rotate 90, flip.|
|(8)    ROT=3; FLIP=0;; # Rotate 270, no flip.|
|(*)    echo "BAD ORIENTATION $ORIENTATION" ; exit -1;;|
|esac|
||
|# Calculate the number of degrees to rotate the image|
|# based on the above table and the user-entered adjustment.|
|DEGREES="$(expr 90 '*' '(' $OFFSET '+' $ROT ')')"|
||
|# Generate the additional flags for sips if flipping is required.|
|FLIPSTR=""|
|if [ $FLIP = 1 ] ; then|
|FLIPSTR="--flip horizontal"|
|else|
|FLIPSTR=""|
|fi|
||
|# Perform the transformation.|
|$SIPS $FLIPSTR --rotate $DEGREES $INPUTFILE --out $OUTPUTFILE|
||
|# Delete the orientation keys so that sips and other tools|
|# won't get confused when doing auto-rotation.|
|$EXIFTOOL -Orientation= $OUTPUTFILE|

## Networking

### Using SIGSTOP And SIGCONT To Manage Long-Lived Daemons

This trick prevents FTP servers on DSL connections from hopelessly clogging up the upstream link by using the `killall` command. It also traps Control-C and other likely signals so that if you break out of the script, the FTP processes are restarted correctly.

**Listing D-7**  Slowing down an FTP server

|   |
|---|
|#!/bin/sh|
||
|SECONDS_TO_RUN=5|
|SECONDS_TO_PAUSE=20|
||
|handler() {|
|killall -CONT ftpd|
|exit 0|
|}|
||
|trap handler SIGHUP SIGTERM SIGQUIT SIGINT|
|# This must be run as root or the ftp user.|
|while true ; do|
|killall -STOP ftpd|
|sleep $SECONDS_TO_PAUSE|
|killall -CONT ftpd|
|sleep $SECONDS_TO_RUN|
|done|

### A Shell-Based Web Server

The [Networking With Shell Scripts](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/AdvancedTechniques/AdvancedTechniques.html#//apple_ref/doc/uid/TP40004268-TP40003521-SW58) section in [Advanced Techniques](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/AdvancedTechniques/AdvancedTechniques.html#//apple_ref/doc/uid/TP40004268-TP40003521-SW35) described how to write a simple daemon using netcat. It is possible to write remarkably complex daemons using this technique.

The first step in an HTTP daemon is parsing the initial request. For simple GET requests without query strings, this is fairy trivial. The following snippet takes the request line as an argument and sets global variables containing the request type, the URL, and the HTTP version.

|   |
|---|
|parseRequest()|
|{|
|local REQUEST="$(echo "$1" \| tr -d '\r')"|
||
|TYPE="$(echo "$REQUEST" \| cut -f 1 -d ' ')"|
|URL="$(echo "$REQUEST" \| cut -f 2 -d ' ')"|
|VERSION="$(echo "$REQUEST" \| cut -f 3 -d ' ')"|
||
|echo "GOT REQUEST: $REQUEST" 1>&2|
|}|

Before you can actually interpret the request, however, you must split off the query string if it is there. For example, the URL `http://example.org/foo.cgi?bar` contains a host part (`example.org`), a path part (`/foo.cgi`), and a query string (`bar`). This code does not split off the host part because it is sent separately from the HTTP query string in HTTP/1.1 and is omitted entirely in HTTP/1.0.

|   |
|---|
|splitURL()|
|{|
|URL="$1"|
|PATHPART="$(echo "$URL" \| sed 's/?.*$//g')"|
|local PATHLEN="$(strlen "$PATHPART")";|
|local CUTPOS="$(expr "$PATHLEN" "+" "2")"|
||
|PARMPART="$(echo "$URL" \| cut -c "$CUTPOS-")"|
|}|

Finally, you must parse the headers that the client sends so you can search for the `Host:` header to know what domain’s contents to serve to the client (and to possibly send some of these headers back to the client). The first snippet reads the data from the client.

|   |
|---|
|parseHeaders()|
|{|
|local FD="$1"|
|local TREENAME="$2"|
|local HEADERLINE|
||
|if [ "$TREENAME" = "" ] ; then|
|TREENAME="HEADERTREE"|
|fi|
||
|# Creates a new tree head object with the specified name.|
|newTree "$TREENAME"|
|eval $TREENAME=\"\$\(getLastNodeName\)\"|
||
|# echo "TN: $TREENAME" 1>&2|
||
|# Reads headers from the specified file descriptor until|
|# it gets a blank line, pasing each one to a parser..|
|while true ; do|
|eval read -u$FD HEADERLINE|
|HEADERLINE="$(echo "$HEADERLINE" \| tr -d '\r')"|
|# echo "GOT HEADER LINE: \"$HEADERLINE\"" 1>&2|
||
|if [ "$HEADERLINE" = "" ] ; then|
|# End of headers reached.|
|# echo "End of headers" 1>&2|
|break;|
|fi|
||
|addHeaderLine "$HEADERLINE" "$TREENAME"|
|done|
|LAST_TREE_NODE_INSERTED="$TREENAME"|
|}|

The next part, addHeaderLine, trivially parses the header line by splitting the string on the first colon (`:`) character and stripping off any leading whitespace after it. Then, it calls another function to add it to the binary tree.

|   |
|---|
|addHeaderLine()|
|{|
|local HEADERLINE="$1"|
|local TREE="$2"|
||
|local FIELDNAME="$(echo "$HEADERLINE" \| cut -f 1 -d ':')"|
|local FIELDVALUE="$(echo "$HEADERLINE" \| cut -f 2- -d ':' \| \|
|sed 's/^[[:space:]]//g')"|
||
|addHeader "$FIELDNAME" "$FIELDVALUE" "$TREE"|
|}|

The final snippet adds the header to a binary tree using the tree library described in [Working with Binary Search Trees](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/StartingPoints/StartingPoints.html#//apple_ref/doc/uid/TP40004268-CH2-SW17).

|   |
|---|
|addHeader()|
|{|
|local FIELDNAME="$1"|
|local FIELDVALUE="$2"|
|local TREE="$3"|
||
|# echo "Inserting $FIELDNAME with value $FIELDVALUE into $TREE" 1>&2|
|insertKey "$TREE" "$FIELDNAME"|
|NODE="$(getLastNodeName)"|
|setTreeField "$NODE" "Contents" "$FIELDVALUE"|
|}|

All that remains is to tie the code together and actually handle the requests. To see the code in action, download the Companion Files zip archive associated with this document. (See the table of contents in the HTML version of this document at developer.apple.com.)

Within the Companion Files archive, you can find the sample at `scripts/BB_Starting_Points/networking/shttpd`.

This script requires a modified version of the OS X version of netcat that provides enhanced functionality and error recovery capabilities beyond what standard netcat versions provide. The Makefile (in the Companion Files archive) downloads, builds, and installs this modified version of netcat. The patch should also be easy to apply to the OpenBSD version of netcat.

**Warning:** This script is not suitable for use in a production environment.

## Text Manipulation

- [Listing 10-3](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/PortingScriptstoMacOSX/PortingScriptstoMacOSX.html#//apple_ref/doc/uid/TP40004268-TP40003517-SW43)—Shows an alternative to the nonportable `head -c` syntax.
    
- [Listing 11-6](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/AdvancedTechniques/AdvancedTechniques.html#//apple_ref/doc/uid/TP40004268-TP40003521-SW15)—Shows how to truncate a string of text to a given number of characters.
    
- [Listing 10-1](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/PortingScriptstoMacOSX/PortingScriptstoMacOSX.html#//apple_ref/doc/uid/TP40004268-TP40003517-SW47) and [Listing 10-2](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/PortingScriptstoMacOSX/PortingScriptstoMacOSX.html#//apple_ref/doc/uid/TP40004268-TP40003517-SW48) show how to convert between the line ending formats used for text files on various platforms.
    
- [Regular Expressions Unfettered](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/RegularExpressionsUnfettered/RegularExpressionsUnfettered.html#//apple_ref/doc/uid/TP40004268-CH238-SW7) covers more complex text manipulation in detail, with examples.
    

## Data Management

### Working with Binary Search Trees

Occasionally, it is useful to keep an array of dictionaries of key-value pairs and to be able to rapidly search through that array. [Listing D-9](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/StartingPoints/StartingPoints.html#//apple_ref/doc/uid/TP40004268-CH2-SW22) provides such functionality in the form of a binary tree.

**Note:** You can find the complete version of this script in the `BB_Starting_Points/networking/shttpd/shttpd/shttpd_subs` directory in the companion files archive.

You can find complete reference documentation in the `BB_Starting_Points/networking/shttpd/shttpd_docs` directory in the companion files archive.

This binary tree library contains a number of key functions:

- **General tree functions:**
    
    `newTree(optional_tree_name)`
    
    Creates a new binary tree.
    
    `deleteTree(tree_name)`
    
    Deletes a binary tree, freeing resources associated with it.
    
    `iterateTree(tree_name, callback, call_on_root=0)`
    
    Iterates through a subtree, calling a function for each node.
    
    `mergeTrees(source_tree_name, dest_tree_name)`
    
    Copies all of the keys in one tree into another. In the event of a collision for a given key, the new values take precedence.
    
- **Insertion Functions:**
    
    `insertKey(tree_name, key)`
    
    Inserts a new key into a binary tree using string comparisons.
    
    `insertKeyNumeric(tree_name, key)`
    
    Inserts a new key into a binary tree using numerical comparisons.
    
    `getLastNodeName()`
    
    Retrieves the last node inserted.
    
- **Node Functions:**
    
    `treeKey(node_name)`
    
    Retrieves the key associated with a node object.
    
    `treeField(node_name, field_name)`
    
    Retrieves a field value for a node in the tree.
    
    `setTreeField(node_name, field_name, new_value)`
    
    Sets a field value for a node in the tree.
    
- **Search Functions:**
    
    `treeSearch(tree_name, key)`
    
    Searches a binary tree for a given key using string comparisons.
    
    `treeSearchNumeric(tree_name, key)`
    
    Searches a binary tree for a given key using numerical comparisons.
    

The following code demonstrates how to use this binary tree library:

**Listing D-8**  Binary tree example

|   |
|---|
|# Tell the binary tree library to not run its tests.|
|DISABLE_TESTS=true|
|. binary_tree.sh|
||
|# Create a new binary tree and obtain its name.|
|newTree|
|TESTTREE="$(getLastNodeName)"|
||
|# Insert three nodes into the tree|
|# with keys 1, 3, and 7.|
|insertKeyNumeric "$TESTTREE" 3|
|insertKeyNumeric "$TESTTREE" 7|
|insertKeyNumeric "$TESTTREE" 1|
||
|# Add an attribute to the last node inserted (1)|
|ONENODE="$(getLastNodeName)"|
|setTreeField "$ONENODE" "MyFieldName" "42"|
||
|# Takes a node and prints the key value and|
|# the value of MyFieldName|
|echokeyandmyfieldname()|
|{|
|echo "$(treeKey "$1") -> $(treeField "$1" "MyFieldName")"|
|}|
||
|# Iterate the tree in key order and call|
|# echokeyandmyfieldname on each node|
|iterateTree "$TESTTREE" "echokeyandmyfieldname"|

Without further introduction, here is the binary tree code library. (The version in the companion files archive also includes some test code.)

**Listing D-9**  binary_tree.sh from shttpd

|   |
|---|
|#!/bin/sh|
||
|# /*!|
|#     @header|
|#         A binary tree algorithm written in a shell script.  The main|
|#         functions of interest are {@link newTree}, {@link deleteTree},|
|#         {@link insertKey}, {@link insertKeyNumeric}, {@link treeSearch},|
|#         {@link treeSearchNumeric}, {@link iterateTree}, and|
|#         {@link mergeTrees}.|
|#|
|#         This is a minimal binary tree implementation that does not support|
|#         removing existing values from the tree once inserted.  However, such|
|#         functionality can be trivially retrofitted on top by adding or|
|#         clearing a "deleted" attribute on nodes using {@link setTreeField} if|
|#         desired.|
|#|
|#         To use this shell script, source it after setting DISABLE_TESTS to|
|#         "true".  To run tests, execute the script directly.|
|#  */|
||
|# /*! @group Global Variables|
|#         Variables used internally.  No user-serviceable parts inside.|
|#  */|
||
|# /*!|
|#     @abstract The starting object ID.  This is an internal counter.|
|#  */|
|OID=0|
||
|# /*!|
|#     @abstract A newline character.|
|#  */|
|NEWLINE="|
|"|
||
|# /*!|
|#     @abstract|
|#         Field separator.  Do not change.|
|#  */|
|IFS="$NEWLINE"|
||
|# /*! @group Node Functions|
|#         Functions that operate on a single node in the tree.|
|#  */|
||
|# /*!|
|#     @abstract Retrieves the key associated with a node object.|
|#     @result|
|#         Returns the key via <code>stdout</code>.|
|#     @param NODE|
|#         The node object.|
|#  */|
|treeKey()|
|{|
|local NODE="$1"|
||
|eval echo "\$$NODE"_KEY|
|}|
||
|# /*!|
|#     @abstract|
|#         Retrieves the left subtree for a node in the tree.|
|#     @result|
|#         Returns the node name of the left subtree via <code>stdout</code>.|
|#     @discussion|
|#         This is mainly an internal function, though you can use|
|#         it for debugging purposes.|
|#     @param NODE|
|#         The node object.|
|#  */|
|treeLeft()|
|{|
|local NODE="$1"|
||
|eval echo "\$$NODE"_LEFT|
|}|
||
|# /*!|
|#     @abstract|
|#         Sets the left subtree for a node in the tree.|
|#     @discussion|
|#         This is an internal function.  Do not call it directly.  Use|
|#         {@link insertKey} or {@link insertKeyNumeric} instead.|
|#     @param NODE|
|#         The node object.|
|#     @param VAL|
|#         The new left value.|
|#  */|
|setTreeLeft()|
|{|
|local NODE="$1"|
|local VAL="$2"|
||
|eval "$NODE"_LEFT=\"$VAL\"|
|}|
||
|# /*!|
|#     @abstract|
|#         Retrieves the right subtree for a node in the tree.|
|#     @result|
|#         Returns the node name of the right subtree via <code>stdout</code>.|
|#     @discussion|
|#         This is mainly an internal function, though you can use|
|#         it for debugging purposes.|
|#     @param NODE|
|#         The node object.|
|#  */|
|treeRight()|
|{|
|local NODE="$1"|
||
|eval echo "\$$NODE"_RIGHT|
|}|
||
|# /*!|
|#     @abstract|
|#         Sets the right subtree for a node in the tree.|
|#     @discussion|
|#         This is an internal function.  Do not call it directly.  Use|
|#         {@link insertKey} or {@link insertKeyNumeric} instead.|
|#     @param NODE|
|#         The node object.|
|#     @param VAL|
|#         The new right value.|
|#  */|
|setTreeRight()|
|{|
|local NODE="$1"|
|local VAL="$2"|
||
|eval "$NODE"_RIGHT=\"$VAL\"|
|}|
||
|# /*!|
|#     @abstract|
|#         Retrieves a field value for a node in the tree.|
|#     @result|
|#         Returns the requested field value via <code>stdout</code> or|
|#         an empty string.|
|#     @seealso setTreeField|
|#     @param NODE|
|#         The node object.|
|#     @param FIELDNAME|
|#         The field name.|
|#  */|
|treeField()|
|{|
|local NODE="$1"|
|local FIELDNAME="$2"|
||
|eval echo "\$$NODE"_DATAFIELD_"$FIELDNAME"|
|}|
||
|# /*!|
|#     @abstract|
|#         Sets a field value for a node in the tree.|
|#     @discussion|
|#         This function allows you to store arbitrary attributes in a tree node.|
|#         If a value already exists for the specified field name, the value is|
|#         overwritten.|
|#     @param NODE|
|#         The node object.|
|#     @param FIELDNAME|
|#         The field name.|
|#     @param VAL|
|#         The new field value.|
|#  */|
|setTreeField()|
|{|
|local NODE="$1"|
|local FIELDNAME="$2"|
|local VAL="$3"|
||
|eval "$NODE"_DATAFIELD_"$FIELDNAME"=\"$VAL\"|
|local DATAFIELDS="$(eval echo "\$$NODE"_DATAFIELDS)"|
|eval "$NODE"_DATAFIELDS="\"$DATAFIELDS$NEWLINE$FIELDNAME\""|
|}|
||
||
|# /*! @group General Tree Functions|
|#         Operations that create, delete, iterate, and merge trees.|
|#  */|
||
|# /*!|
|#     @abstract|
|#         Iterates through a subtree, calling a function for each node.|
|#     @discussion|
|#         For each node in the tree (in sorted order), the function|
|#         specified by ACTION is called with a single parameter|
|#         containing the node name of the node being traversed.|
|#     @param TREE|
|#         The tree to traverse.|
|#     @param ACTION|
|#         The function to call on each node.|
|#     @param CALLONROOT|
|#         Set to 1 if you want to also call ACTION on the (bogus) root node.|
|#         This is usually only set for debug printing purposes.|
|#  */|
|iterateTree()|
|{|
|local TREE="$1"|
|local ACTION="$2"|
|local CALLONROOT="$3"|
||
|# echo "NAME IS $TREE"|
||
|if [ "$CALLONROOT" = "1" ] ; then|
|eval "$ACTION" "$TREE"|
|fi|
||
|iterateSubtree "$(treeLeft "$TREE")" "$ACTION"|
|}|
||
|# /*!|
|#     @abstract|
|#         Copies all of the keys in one tree into another.|
|#     @discussion|
|#         For each key in TREE_SRC, an equivalent key is|
|#         inserted in TREE_DST, including any field values|
|#         associated with it.  In the event of a collision|
|#         for a given key, the resulting set of field values|
|#         for that key is the union of the two sets of field|
|#         values, with the new values from TREE_SRC taking|
|#         precedence.|
|#     @param TREE_SRC|
|#         The source tree to copy.|
|#     @param TREE_DST|
|#         The destination tree into which the source tree is copied.|
|#  */|
|mergeTrees()|
|{|
|local TREE_SRC="$1"|
|local TREE_DST="$2"|
||
|# echo "Here SRC: $TREE_SRC (left is $(treeLeft "$TREE_SRC"))" 1>&2|
|# echo "     DST: $TREE_DST" 1>&2|
||
|iterateSubtree "$(treeLeft "$TREE_SRC")" reinsert|
|}|
||
|# /*!|
|#     @abstract|
|#         Deletes a binary tree.|
|#     @param TREE|
|#         The name of the tree to delete.|
|#  */|
|deleteTree()|
|{|
|local TREE="$1"|
||
|if [ "$TREE" = "" ] ; then|
|return;|
|fi|
||
|deleteTree "$(treeLeft "$TREE")"|
|deleteTree "$(treeRight "$TREE")"|
|deleteNode "$TREE"|
|}|
||
|# /*!|
|#     @abstract|
|#         Creates a new binary tree.|
|#     @result|
|#         Obtain the name of the tree using {@link getLastNodeName}.|
|#     @param TREE|
|#         The name of the tree to create.|
|#  */|
|newTree()|
|{|
|local TREE="$1"|
||
|newTreeNode "" "" "" "$TREE"|
|}|
||
|# /*! @group Search Functions|
|#         Functions used for searching for a key in a tree.  Be sure to|
|#         choose whether you want to use numerical or string key comparisons|
|#         for the search and choose the appropriate function accordingly.|
|#         The comparison type usde for searching must match the comparison|
|#         type used during insertion or the results are undefined.|
|#  */|
||
|# /*!|
|#     @abstract|
|#         Searches a binary tree for a given key.|
|#     @discussion|
|#         This tree search uses string comparisons.  You must use|
|#         {@link insertKey} with this function (and not|
|#         {@link insertKeyNumeric}.  For numeric searches, use|
|#         {@link treeSearchNumeric}.|
|#     @result|
|#         Returns the node name of the matching node through <code>stdout</code>|
|#         if found or an empty string otherwise.|
|#     @param TREE|
|#         The tree to search.|
|#     @param KEY|
|#         The key to search for.|
|#  */|
|treeSearch()|
|{|
|local TREE="$1"|
|local KEY="$2"|
||
|subtreeSearch "$(treeLeft "$TREE")" "$KEY"|
|}|
||
|# /*!|
|#     @abstract|
|#         Searches a binary tree for a given key.|
|#     @result|
|#         Returns the node name of the matching node through <code>stdout</code>|
|#         if found or an empty string otherwise.|
|#     @discussion|
|#         This tree search uses numeric comparisons.  You must use|
|#         {@link insertKeyNumeric} with this function (and not|
|#         {@link insertKey}.  For string searches, use {@link treeSearch}.|
|#     @param TREE|
|#         The tree to search.|
|#     @param KEY|
|#         The key to search for.|
|#  */|
|treeSearchNumeric()|
|{|
|local TREE="$1"|
|local KEY="$2"|
||
|subtreeSearchNumeric "$(treeLeft "$TREE")" "$KEY"|
|}|
||
|# /*! @group Insertion Functions|
|#         Functions used for inserting a key into a tree.  Be sure to|
|#         choose whether you want to use numerical or string key comparisons|
|#         during insertion and choose the appropriate function accordingly.|
|#|
|#         After inserting, you can use {@link getLastNodeName} to get the|
|#         node name of the resulting node if desired.|
|#  */|
||
|# /*!|
|#     @abstract|
|#         Retrieves the last node inserted.|
|#     @result|
|#         Returns the node name of the last node inserted via|
|#         <code>stdout</code>.|
|#     @discussion|
|#         After creating a new node with {@link insertKey} or a|
|#         new tree with {@link newTree}, call this to obtain its|
|#         note ID.|
|#  */|
|getLastNodeName()|
|{|
|echo "$LAST_TREE_NODE_INSERTED"|
|}|
||
|# /*!|
|#     @abstract|
|#         Inserts a new key into a binary tree.|
|#     @discussion|
|#         If a node already exists with this value, the|
|#         existing node is returned.|
|#|
|#         This tree insertion uses string comparisons.  You must use|
|#         {@link treeSearch} with this function (and not|
|#         {@link treeSearchNumeric}.  For numeric searches, use|
|#         {@link insertKeyNumeric}.|
|#     @result|
|#         Obtain the node name of the newly created node using|
|#         {@link getLastNodeName}.|
|#     @param TREE|
|#         The name of the binary tree.|
|#     @param KEY|
|#         The key to insert.|
|#  */|
|insertKey()|
|{|
|local TREE="$1"|
|local KEY="$2"|
||
|local LASTTREE="$TREE"|
|local DIRECTION="LEFT"|
||
|while [ "$TREE" != "" -a "$LASTTREE" != "" ] ; do|
|if [ $DIRECTION = "LEFT" ] ; then|
|TREE="$(treeLeft "$TREE")"|
|else|
|TREE="$(treeRight "$TREE")"|
|fi|
|local TREEKEY="$(treeKey "$TREE")"|
||
|if [ "$TREE" != "" ] ; then|
|if [ "$KEY" \< "$TREEKEY" ] ; then|
|DIRECTION="LEFT"|
|LASTTREE="$TREE"|
|elif [ "$KEY" \> "$TREEKEY" ] ; then|
|DIRECTION="RIGHT"|
|LASTTREE="$TREE"|
|else|
|# Matching node already exists.  Return its name.|
|LAST_TREE_NODE_INSERTED="$NODE"|
|return|
|fi|
|fi|
|done|
|newTreeNode "" "" "$KEY"|
|local NODE="$(getLastNodeName)"|
||
|if [ $DIRECTION = "LEFT" ] ; then|
|setTreeLeft "$LASTTREE" "$NODE"|
|else|
|setTreeRight "$LASTTREE" "$NODE"|
|fi|
|}|
||
|# /*!|
|#     @abstract|
|#         Inserts a new key into a binary tree.|
|#     @discussion|
|#         If a node already exists with this value, the|
|#         existing node is returned.|
|#|
|#         This tree insertion uses string comparisons.  You must use|
|#         {@link treeSearch} with this function (and not|
|#         {@link treeSearchNumeric}.  For numeric searches, use|
|#         {@link insertKeyNumeric}.|
|#     @result|
|#         Obtain the node name of the newly created node using|
|#         {@link getLastNodeName}.|
|#     @param TREE|
|#         The name of the binary tree.|
|#     @param KEY|
|#         The key to insert.|
|#  */|
|insertKeyNumeric()|
|{|
|local TREE="$1"|
|local KEY="$2"|
||
|# echo "IN INSNUM"|
||
|local LASTTREE="$TREE"|
|local DIRECTION="LEFT"|
||
|while [ "$TREE" != "" -a "$LASTTREE" != "" ] ; do|
|if [ $DIRECTION = "LEFT" ] ; then|
|TREE="$(treeLeft "$TREE")"|
|else|
|TREE="$(treeRight "$TREE")"|
|fi|
|local TREEKEY="$(treeKey "$TREE")"|
||
|if [ "$TREE" != "" ] ; then|
|if [ "$KEY" -lt "$TREEKEY" ] ; then|
|DIRECTION="LEFT"|
|LASTTREE="$TREE"|
|elif [ "$KEY" -gt "$TREEKEY" ] ; then|
|DIRECTION="RIGHT"|
|LASTTREE="$TREE"|
|else|
|# Matching node already exists.  Return its name.|
|LAST_TREE_NODE_INSERTED="$NODE"|
|return|
|fi|
|fi|
|done|
|newTreeNode "" "" "$KEY"|
|local NODE="$(getLastNodeName)"|
||
|if [ $DIRECTION = "LEFT" ] ; then|
|setTreeLeft "$LASTTREE" "$NODE"|
|else|
|setTreeRight "$LASTTREE" "$NODE"|
|fi|
|}|
||
||
|# /*! @group Debug Functions|
|#         Functions that print debug information about binary trees,|
|#         tree nodes, and so on.|
|#  */|
||
|# /*!|
|#     @abstract|
|#         Prints a node structure for debugging purposes.|
|#     @param NODE|
|#         The node to print.|
|#  */|
|printNode()|
|{|
|local NODE="$1"|
||
|echo "NAME:  $NODE"|
|echo "KEY:   $(treeKey "$NODE")"|
|echo "LEFT:  $(treeLeft "$NODE")"|
|echo "RIGHT: $(treeRight "$NODE")"|
|echo "DATA:"|
||
|local DATAFIELDS="$(eval echo "\$$NODE"_DATAFIELDS)"|
|local FIELDNAME|
|for FIELDNAME in $DATAFIELDS ; do|
|# Skip the empty first field.|
|if [ "$FIELDNAME" != "" ] ; then|
|eval echo "     $NODE""_DATAFIELD_$FIELDNAME"":" \|
|"\$$NODE""_DATAFIELD_$FIELDNAME"|
|fi|
|done|
|echo "-=-=-=-=-=-=-=-=-=-=-=-"|
|}|
||
|# /*!|
|#     @abstract|
|#         Prints out the contents of a tree for debugging purposes.|
|#  */|
|printTree()|
|{|
|local TREE="$1"|
||
|# echo "NAME IS $TREE"|
|iterateTree "$TREE" "printNode" 1|
|}|
||
|# /*!|
|#     @abstract|
|#         Prints a line of text in red letters.|
|#  */|
|echored()|
|{|
|printf "\e[1;31m%s\e[0;30m\n" $@|
|}|
||
|# /*!|
|#     @abstract|
|#         Prints a line of text in green letters.|
|#  */|
|echogreen()|
|{|
|printf "\e[1;32m%s\e[0;30m\n" $@|
|}|
||
|# /*!|
|#     @abstract|
|#         Prints a line of text in blue letters.|
|#  */|
|echoblue()|
|{|
|printf "\e[1;34m%s\e[0;30m\n" $@|
|}|
||
||
|# /*! @group Internal Functions|
|#         No user-serviceable parts inside.  These functions are used|
|#         internally by the other functions and should generally not|
|#         be called from outside unless you really know what you are|
|#         doing.|
|#  */|
||
|# /*!|
|#     @abstract|
|#         Iterates through a subtree, calling a function for each node.|
|#     @discussion|
|#         Do not call this directly.  Call {@link iterateTree} instead.|
|#  */|
|iterateSubtree()|
|{|
|local TREE="$1"|
|local ACTION="$2"|
||
|if [ "$TREE" = "" ] ; then|
|return;|
|fi|
|# echo "IN IST: TREE $TREE" 1>&2|
||
|iterateSubtree "$(treeLeft "$TREE")" "$ACTION"|
|eval "$ACTION $TREE"|
|iterateSubtree "$(treeRight "$TREE")" "$ACTION"|
||
|}|
||
|# /*!|
|#     @abstract|
|#         Internal helper function.|
|#     @discussion|
|#         This function is used by {@link mergeTrees} to take a node from|
|#         one tree and duplicte it in another.|
|#  */|
|reinsert()|
|{|
|local NODE="$1"|
||
|# echo "GOT NODE \"$NODE\"" 1>&2|
||
|# echo "TREE_DST: $TREE_DST" 1>&2|
||
|if [ "$NODE" = "" ] ; then|
|return;|
|fi|
||
|local VAL="$(treeKey "$NODE")"|
||
|if [ "$VAL" = "" ] ; then|
|return;|
|fi|
||
|# local NEWNODE="$(treeSearch "$TREE_DST" "$VAL")"|
|# echo "NN1: $NEWNODE"|
||
|insertKey "$TREE_DST" "$VAL"|
|local NEWNODE="$(getLastNodeName)"|
||
|# print "NN: $NEWNODE" 1>&2|
||
|local DATAFIELDS="$(eval echo "\$$NODE"_DATAFIELDS)"|
|local FIELDNAME|
||
|for FIELDNAME in $DATAFIELDS ; do|
|# Skip the empty first field.|
|if [ "$FIELDNAME" != "" ] ; then|
|# eval echo setting "$NEWNODE""_DATAFIELD_$FIELDNAME""=\"\$$NODE""_DATAFIELD_$FIELDNAME\"" 1>&2|
|eval "$NEWNODE""_DATAFIELD_$FIELDNAME""=\|
|\"\$$NODE""_DATAFIELD_$FIELDNAME\""|
|fi|
|done|
||
|# printNode "$NODE"|
|}|
||
|# /*!|
|#     @abstract|
|#         Creates a new node in the tree.|
|#     @discussion|
|#         This is an internal function.  Do not call it directly.  Use|
|#         {@link insertKey} or {@link insertKeyNumeric} instead.|
|#     @param LEFT|
|#         The initial left value for the node (usually empty).|
|#     @param RIGHT|
|#         The initial right value for the node (usually empty).|
|#     @param KEY|
|#         The key for the new node.|
|#     @param TREE|
|#         The desired name for the node (usually empty).|
|#  */|
|newTreeNode()|
|{|
|local LEFT="$1"|
|local RIGHT="$2"|
|local KEY="$3"|
|local TREE="$4"|
||
|if [ "$TREE" = "" ] ; then|
|TREE="TREENODE_$OID"|
|OID="$(expr "$OID" "+" "1")"|
|# echo "$TREE"|
|# else|
|# echo "Using explicit name \"$TREE\"" 1>&2|
|fi|
||
|eval "$TREE"_LEFT=\"$LEFT\"|
|eval "$TREE"_RIGHT=\"$RIGHT\"|
|eval "$TREE"_KEY=\"$KEY\"|
|LAST_TREE_NODE_INSERTED="$TREE"|
|}|
||
||
|# /*!|
|#     @abstract|
|#         Searches a binary tree for a given key.|
|#     @discussion|
|#         This is an internal function.  Do not call it directly.  Use|
|#         {@link treeSearch} instead.|
|#     @result|
|#         Returns the node name of the matching node through <code>stdout</code>|
|#         if found or an empty string otherwise.|
|#     @param TREE|
|#         The subtree to search.|
|#     @param KEY|
|#         The key to search for.|
|#  */|
|subtreeSearch()|
|{|
|local TREE="$1"|
|local KEY="$2"|
||
|if [ "$TREE" = "" ] ; then|
|return;|
|fi|
||
|local TREEKEY="$(treeKey "$TREE")"|
||
|if [ "$KEY" \< "$TREEKEY" ] ; then|
|subtreeSearch "$(treeLeft "$TREE")" "$KEY"|
|elif [ "$KEY" \> "$TREEKEY" ] ; then|
|subtreeSearch "$(treeRight "$TREE")" "$KEY"|
|else|
|echo $TREE|
|fi|
|}|
||
|# /*!|
|#     @abstract|
|#         Searches a binary tree for a given key.|
|#     @discussion|
|#         This is an internal function.  Do not call it directly.  Use|
|#         {@link treeSearch} instead.|
|#     @result|
|#         Returns the node name of the matching node through <code>stdout</code>|
|#         if found or an empty string otherwise.|
|#     @param TREE|
|#         The subtree to search.|
|#     @param KEY|
|#         The key to search for.|
|#  */|
|subtreeSearchNumeric()|
|{|
|local TREE="$1"|
|local KEY="$2"|
||
|if [ "$TREE" = "" ] ; then|
|return;|
|fi|
||
|local TREEKEY="$(treeKey "$TREE")"|
||
|if [ "$KEY" -lt "$TREEKEY" ] ; then|
|subtreeSearchNumeric "$(treeLeft "$TREE")" "$KEY"|
|elif [ "$KEY" -gt "$TREEKEY" ] ; then|
|subtreeSearchNumeric "$(treeRight "$TREE")" "$KEY"|
|else|
|echo $TREE|
|fi|
|}|
||
|# /*!|
|#     @abstract|
|#         Deletes a node in a tree.|
|#     @discussion|
|#         This algorithm does not support deleting arbitrry nodes.|
|#         This is an internal function that is used by {@link deleteTree}.|
|#     @param NODE|
|#         The node to delete.|
|#  */|
|deleteNode()|
|{|
|local NODE="$1"|
||
|local DATAFIELDS="$(eval echo "\$$NODE"_DATAFIELDS)"|
||
|local FIELDNAME|
|for FIELDNAME in $DATAFIELDS ; do|
|# Skip the empty first field.|
|if [ "$FIELDNAME" != "" ] ; then|
|eval unset "$NODE"_DATAFIELD_$FIELDNAME|
|fi|
|done|
|eval unset "$NODE"_LEFT|
|eval unset "$NODE"_RIGHT|
|}|

## User and Group Management

OS X provides significant GUI tools for managing users and groups. Sometimes, however, you may need to do things the hard way (from the command line). For the occasional hand addition, you can manually add a user or group using the `dscl` (directory service command line) tool. However, if you regularly need to add users, it can be advantageous to script the task.

The code listings here (which are also included in the Companion Files archive) show how to create a new user and a new group, including choosing unused user and group IDs.

**Listing D-10**  Script for adding a new user using dscl (adduser.sh)

|   |
|---|
|#!/bin/sh|
||
|# Usage:|
|#|
|# adduser [-a] <USERNAME> <LONGNAME> <PRIMARY_GID> [ <HOME_DIRECTORY> [ <UID> ]]|
|#|
|# -a: Make the user an admin user.|
|# USERNAME: The OS X "short name", e.g. jdoe|
|# LONGNAME: The OS X "real name", e.g. "John Doe"|
|# PRIMARY_GID: The primary group ID.|
|# HOME_DIRECTORY: The user's home directory.  Leave blank to use /Users/username.|
|#                 The script attempts to create this directory if it does not exist.A|
|# UID: The user ID for the new user.  Leave blank for the script to automatically|
|#      choose the first unused ID at or above MINUID (currently 501).|
||
|ADMIN="user"|
|if [ "$1" = "-a" ] ; then|
|ADMIN="admin user"|
|shift|
|fi|
||
|USERNAME="$1"|
|LONGNAME="$2"|
|PRIMARY_GID="$3"|
|HOMEDIR="$4" # Optional|
|NEWUID="$5" # Optional|
||
|MINUID=501|
|DOMAIN="."|
||
|# Must have newline here.|
|IFS="|
|"|
||
|# /*!|
|#     @abstract Checks to see if a long name is reasonable.|
|#     @discussion Ideally, this should do more checks.|
|#  */|
|valid_username()|
|{|
|local NAME="$1"|
||
|if [ "$NAME" = "" ] ; then|
|return 1;|
|fi|
||
|return 0;|
|}|
||
|# /*!|
|#     @abstract Checks to see if a long name is reasonable.|
|#     @discussion|
|#         Checking for non-empty strings is good enough for now,|
|#         but ideally, this should also check for duplicates.|
|#         The code doesn't do this because there's no good way|
|#         that doesn't involve a huge file and grep.|
|#  */|
|valid_longname()|
|{|
|local NAME="$1"|
||
|if [ "$NAME" = "" ] ; then|
|return 1;|
|fi|
|return 0|
|}|
||
|# /*!|
|#     @abstract Checks to see if a (numeric) group ID is reasonable.|
|#  */|
|valid_gid()|
|{|
|local NEWGID="$1"|
||
|# Empty primary GID is illegal.|
|if [ "$NEWGID" = "" ] ; then|
|return 1;|
|fi|
||
|local NEWGIDSTR="$(printf "%d" "$NEWGID" 2> /dev/null)"|
||
|if [ "$NEWGIDSTR" != "$NEWGID" ] ; then|
|return 1;|
|fi|
||
|return 0;|
|}|
||
|# /*!|
|#     @abstract Checks to see if a (numeric) user ID is reasonable.|
|#  */|
|valid_uid()|
|{|
|local NEWUID="$1"|
||
|# Empty UID means "choose one for me"|
|if [ "$NEWUID" = "" ] ; then|
|return 0;|
|fi|
||
|local NEWUIDSTR="$(printf "%d" "$NEWUID" 2> /dev/null)"|
||
|if [ "$NEWUIDSTR" != "$NEWUID" ] ; then|
|return 1;|
|fi|
||
|return 0;|
|}|
||
|# /*!|
|#     @abstract Creates an associative pseudo-array for UID to username mapping.|
|#  */|
|initUIDMap()|
|{|
|local SKIPUSER="$1"|
||
|local USERS="$(dscl "$DOMAIN" -list /Users)"|
||
|for i in $USERS ; do|
|if [ "$i" != "$SKIPUSER" ] ; then|
|eval "UID_$(dscl "$DOMAIN" -read /Users/"$i" UniqueID 2>/dev/null \| sed 's/UniqueID: //' \| sed 's/-/MINUS/')=\"$i\""|
|fi|
|done|
|}|
||
|# /*!|
|#     @abstract Looks up a UID in the pseudo-array and maps it to a username|
|#  */|
|uidToName()|
|{|
|local CHECKUID="$1"|
||
|local CHECKUID_ENCODED="$(echo "$CHECKUID" \| sed 's/-/MINUS/')"|
||
||
|eval echo '$UID_'$CHECKUID_ENCODED|
|}|
||
|# /*!|
|#     @abstract Finds the next unused UID.|
|#  */|
|assignUID()|
|{|
|initUIDMap|
||
|# An error here means somebody screwed up MINUID.|
|local POS=$MINUID|
||
|while true ; do|
|# echo "Trying $POS" 1>&2|
|local TEMPNAME="$(uidToName $POS)"|
|if [ "$TEMPNAME" = "" ] ; then|
|echo $POS|
|return;|
|fi|
|POS="$(expr $POS '+' 1)"|
|done|
|}|
||
|# /*!|
|#     @abstract Returns success if no other user has the chosen UID.|
|#  */|
|uid_not_conflicting()|
|{|
|local NEWUID="$1"|
|local NEWUSER="$2"|
||
|initUIDMap "$NEWUSER"|
||
|local TEMPNAME="$(uidToName "$NEWUID")"|
||
|if [ "$TEMPNAME" != "" ] ; then|
|return 1;|
|fi|
||
|return 0|
|}|
||
||
|while ! valid_username "$USERNAME" ; do|
|printf "Enter username: "|
|read USERNAME|
|done|
||
|while ! valid_uid  "$NEWUID" ; do|
|printf "Invalid UID specified.  Enter desired UID: "|
|read NEWUID|
|done|
||
|while ! valid_gid  "$PRIMARY_GID" ; do|
|printf "Invalid group ID specified.  Enter desired GID: "|
|read PRIMARY_GID|
|done|
||
|while ! valid_longname "$LONGNAME" ; do|
|printf "Invalid long name specified.  Enter desired long name: "|
|read LONGNAME|
|done|
||
||
|# Test code|
|### echo "UID Conflict check:"|
|### uid_not_conflicting "501" "dg" # Test this first or else.|
|### echo "$? should be 0"|
|### uid_not_conflicting "501" "Schlomo"|
|### echo "$? should be 1"|
||
|### echo "First free UID is $(assignUID)"|
||
|dscl $DOMAIN -read /Users/"$USERNAME" > /dev/null 2>&1|
|if [ $? = 0 ] ; then|
|echo "Failed.  A user with that name already exists.." 1>&2|
|exit -1|
|fi|
||
|dscl $DOMAIN -create /Users/"$USERNAME"|
||
|if [ $? != 0 ] ; then|
|echo "Failed.  User could not be created." 1>&2|
|exit -1|
|fi|
||
|dscl $DOMAIN -create /Users/"$USERNAME" UserShell /bin/bash|
|dscl $DOMAIN -create /Users/"$USERNAME" RealName "$LONGNAME"|
|if [ "$NEWUID" = "" ] ; then|
|NEWUID="$(assignUID)"|
|fi|
|dscl $DOMAIN -create /Users/"$USERNAME" UniqueID $NEWUID|
||
|while ! uid_not_conflicting "$NEWUID" "$USERNAME"; do|
|echo "A user with ID $NEWUID exists already.  Assigning a new UID." 1>&2|
|OLDUID="$NEWUID"|
|NEWUID="$(assignUID)"|
|dscl $DOMAIN -change /Users/"$USERNAME" UniqueID "$OLDUID" "$NEWUID"|
|done|
||
|dscl $DOMAIN -create /Users/"$USERNAME" PrimaryGroupID $PRIMARY_GID|
||
|if [ "$HOMEDIR" = "" ] ; then|
|dscl $DOMAIN -create /Users/"$USERNAME" NFSHomeDirectory /Users/"$USERNAME"|
|if [ ! -d "/Users/$USERNAME" ] ; then|
|mkdir "/Users/$USERNAME"|
|fi|
|else|
|dscl $DOMAIN -create /Users/"$USERNAME" NFSHomeDirectory "$HOMEDIR";|
|fi|
||
|dscl $DOMAIN -passwd /Users/"$USERNAME" "*"|
|# passwd "$USERNAME"|
||
|UUID="$(/usr/bin/uuidgen)"|
|dscl $DOMAIN -create /Users/"$USERNAME" GeneratedUID "$UUID"|
||
|if [ "$ADMIN" = "admin user" ] ; then|
|dscl $DOMAIN -append /Groups/admin GroupMembership "$USERNAME"|
|dscl $DOMAIN -append /Groups/admin GroupMembers "$UUID"|
|fi|
||
|echo "Added $ADMIN $USERNAME with ID $NEWUID and UID $UUID.  Please remember to set a password for the user."|
||

**Listing D-11**  Script for adding a new group using dscl (addgroup.sh)

|   |
|---|
|#!/bin/sh|
||
|# Usage:|
|#|
|# addgroup <GROUPNAME> <LONGNAME> [<GID> ]|
|#|
|# GROUPNAME: The OS X "short name", e.g. admin|
|# LONGNAME: The OS X "real name", e.g. "Administrators"|
|# GID: The group ID for the new group.  Leave blank for the script to automatically|
|#      choose the first unused ID at or above MINGID (currently 501).|
|#|
||
||
|GROUPNAME="$1"|
|LONGNAME="$2"|
|NEWGID="$3" # Optional|
||
|MINGID=501|
||
|DOMAIN="."|
||
|# Must have newline here.|
|IFS="|
|"|
||
|ADDGROUP="./addgroup.sh"|
||
|if [ -f "/usr/local/bin/addgroup" ] ; then|
|ADDGROUP="/usr/local/bin/addgroup"|
|fi|
||
|# /*!|
|#     @abstract Checks to see if a group long name is reasonable.|
|#     @discussion|
|#         Checking for non-empty strings is good enough for now,|
|#         but ideally, this should also check for duplicates.|
|#         The code doesn't do this because there's no good way|
|#         that doesn't involve a huge file and grep.|
|#  */|
|valid_longname()|
|{|
|local NAME="$1"|
||
|if [ "$NAME" = "" ] ; then|
|return 1;|
|fi|
||
|return 0;|
|}|
||
|# /*!|
|#     @abstract Checks to see if a group name is reasonable.|
|#     @discussion Ideally, this should do more checks.|
|#  */|
|valid_groupname()|
|{|
|local NAME="$1"|
||
|if [ "$NAME" = "" ] ; then|
|return 1;|
|fi|
|return 0|
|}|
||
||
|# /*!|
|#     @abstract Checks to see if a (numeric) group ID is reasonable.|
|#  */|
|valid_gid()|
|{|
|local NEWGID="$1"|
||
|# Empty primary GID means "choose one for me"|
|if [ "$NEWGID" = "" ] ; then|
|return 0;|
|fi|
||
|local NEWGIDSTR="$(printf "%d" "$NEWGID" 2> /dev/null)"|
||
|if [ "$NEWGIDSTR" != "$NEWGID" ] ; then|
|return 1;|
|fi|
||
|return 0;|
|}|
||
||
|# /*!|
|#     @abstract Creates an associative pseudo-array for GID to username mapping.|
|#  */|
|initGIDMap()|
|{|
|local SKIPGROUP="$1"|
||
|# GROUPS is BASH reserved word|
|local ALLGROUPS="$(dscl "$DOMAIN" -list /Groups)"|
||
|for i in $ALLGROUPS ; do|
|if [ "$i" != "$SKIPGROUP" ] ; then|
|eval "GID_$(dscl "$DOMAIN" -read /Groups/"$i" PrimaryGroupID 2>/dev/null \| sed 's/PrimaryGroupID: //' \| sed 's/-/MINUS/')=\"$i\""|
|fi|
|done|
|}|
||
||
|# /*!|
|#     @abstract Looks up a GID in the pseudo-array and maps it to a group name|
|#  */|
|gidToName()|
|{|
|local CHECKGID="$1"|
||
|local CHECKGID_ENCODED="$(echo "$CHECKGID" \| sed 's/-/MINUS/')"|
||
||
|eval echo '$GID_'$CHECKGID_ENCODED|
|}|
||
|# /*!|
|#     @abstract Finds the next unused UID.|
|#  */|
|assignGID()|
|{|
|initGIDMap|
||
|# An error here means somebody screwed up MINGID.|
|local POS=$MINGID|
||
|while true ; do|
|# echo "Trying $POS" 1>&2|
|local TEMPNAME="$(gidToName $POS)"|
|if [ "$TEMPNAME" = "" ] ; then|
|echo $POS|
|return;|
|fi|
|POS="$(expr $POS '+' 1)"|
|done|
|}|
||
|# /*!|
|#     @abstract Returns success if no other group has the chosen GID.|
|#  */|
|gid_not_conflicting()|
|{|
|local NEWGID="$1"|
|local NEWGROUP="$2"|
||
|initGIDMap "$NEWGROUP"|
||
|local TEMPNAME="$(gidToName "$NEWGID")"|
||
|if [ "$TEMPNAME" != "" ] ; then|
|return 1;|
|fi|
||
|return 0|
|}|
||
||
|while ! valid_groupname "$GROUPNAME" ; do|
|printf "Enter group name: "|
|read GROUPNAME|
|done|
||
|while ! valid_gid  "$NEWGID" ; do|
|printf "Invalid or no group ID specified.  Enter desired GID: "|
|read NEWGID|
|done|
||
|while ! valid_longname "$LONGNAME" ; do|
|printf "Invalid long name specified.  Enter desired long name: "|
|read LONGNAME|
|done|
||
||
|# Test code|
|# echo "GID Conflict check:"|
|# gid_not_conflicting "80" "admin" # Test this first or else.|
|# echo "$? should be 0"|
|# gid_not_conflicting "80" "Schlomo"|
|# echo "$? should be 1"|
||
|echo "First free GID is $(assignGID)"|
||
|dscl $DOMAIN -read /Groups/"$GROUPNAME" > /dev/null 2>&1|
|if [ $? = 0 ] ; then|
|echo "Failed.  A group with that name already exists.." 1>&2|
|exit -1|
|fi|
||
|dscl $DOMAIN -create /Groups/"$GROUPNAME"|
||
|if [ $? != 0 ] ; then|
|echo "Failed.  Group could not be created." 1>&2|
|exit -1|
|fi|
||
|dscl $DOMAIN -create /Groups/"$GROUPNAME" RealName "$LONGNAME"|
|if [ "$NEWGID" = "" ] ; then|
|NEWGID="$(assignGID)"|
|fi|
|dscl $DOMAIN -create /Groups/"$GROUPNAME" PrimaryGroupID $NEWGID|
||
|while ! gid_not_conflicting "$NEWGID" "$GROUPNAME"; do|
|echo "A user with ID $NEWGID exists already.  Assigning a new GID." 1>&2|
|OLDGID="$NEWGID"|
|NEWGID="$(assignGID)"|
|dscl $DOMAIN -change /Groups/"$GROUPNAME" PrimaryGroupID "$OLDGID" "$NEWGID"|
|done|
||
|UUID="$(/usr/bin/uuidgen)"|
|dscl $DOMAIN -create /Groups/"$GROUPNAME" GeneratedUID "$UUID";|
||
|# Legacy UNIX group password|
|dscl $DOMAIN -create /Groups/"$GROUPNAME" Password "*"|
||
|echo "Added $GROUPNAME with ID $NEWGID and UUID $UUID.  Please remember to set a password for the user."|

[Next](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/AnExtremeExample/AnExtremeExample.html)[Previous](https://developer.apple.com/library/archive/documentation/OpenSource/Conceptual/ShellScripting/ForMoreInformation/ForMoreInformation.html)