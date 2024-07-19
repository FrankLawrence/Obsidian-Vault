---
tags: cli
---
# A Practical Guide to GNU sed With Examples
## The Basics of sed
`sed` stands for *s*tream *ed*itor. It can:
1. Take a stream of text as input
2. Select some specific lines
3. Perform some operations on each line selected
4. Output the resulting text

Steps 2 and 3 are done with a [[##Sed Script]] 
### General Command-Line Syntax
Our stream editor takes two arguments:
```bash
sed [script] [file...]
```

> [!example]
> `sed '' nginx.conf`
> This simple example will simply print every single line of its input.

### Sed Script

A sed script can be divided in three parts:

| Name    | Description                                                         |
| ------- | ------------------------------------------------------------------- |
| Address | The lines you want to edit. It's always followed by a command.      |
| Command | The operation you want to perform. It's always a single letter.     |
| Options | A couple of commands can have options, like the substitute command. |

> [!example] Delete
> ```bash
> sed 'd' nginx.conf
> ```
> Commands operate on each line -> we will delete every single line
> ```bash
> # Delete the first line
> sed '1d' nginx.conf
> ```

Sed will not modify files/input streams but instead:
1. Copy each line of the input in a buffer
2. Do the operations described by the sed script on the buffered lines.
3. Output the (possibly modified) buffered lines.

Modifying your input file with sed is **irreversible**. You can change a file, by overwriting it with [[Redirect|Redirects]] , or with the `-i` option to modify *i*n place. You can also create a backup file of the unedited version with `sed -i.back '1d' nginx.conf`.

## The Address: Selecting Specific Lines
The address indicates what to copy in sed's buffer, to then apply a sed command to each of them. An address can be: line number, range of lines, every *n*th line, or a [[Regex]].

| Example                 | Description                                                | Type             |
| ----------------------- | ---------------------------------------------------------- | ---------------- |
| `sed '1d' nginx.conf`   | Delete the first line                                      | Line number      |
| `sed '2,5d' nginx.conf` | Delete the lines 2 to 5 (included)                         | Range of line    |
| `sed '0-2d nginx.conf`  | Delete every even line (from line 0, delete every 2 lines) | Every *n*th line |
| `sed '1-2d' nginx.conf` | Delete every odd line (from line 1, delete every 2 lines)  | Every *n*th line |

| Example                                | Description                                                                                 | Type                             |
| -------------------------------------- | ------------------------------------------------------------------------------------------- | -------------------------------- |
| `sed '/include/d' nginx.conf`          | Delete each line matching the pattern `include`                                             | Regex                            |
| `sed '3,/include/d' nginx.conf`        | Delete from the 3rd line until the first line matching the pattern `include`                | Range of line using a regex      |
| `sed '/http/,/include/d' nginx.conf`   | Delete every line, from the first line matching `http` to the first line matching `include` | Range of lines using two regexes |
| `sed '/HTTP/I,/InClUdE/Id' nginx.conf` | Case *i*nsensitive search of previous example                                               | `I` flag for case insensitive    |

#### The Extended Regex Engine

With the `-E` flag, [regex metacharacters](https://thevaluable.dev/regular-expression-basics-vim-grep/#characters-and-metacharacters) `?`, `+`, `()`, `{}` and `|` don not need to be escaped.
### Only Output the Chosen Lines
As we saw at the very beginning, sed always output every line of the input by default. You can disable this with the command-line option `-n` (for no print), or its more understandable equivalents `--quiet` or `--silent`. We can the use the `p` flag to *only* print the lines we specify.

> [!example] Lines between beginning of the file until the first line matching `include`
> ```bash
> sed --quiet '1,/include/p' nginx.conf | wc -l
> ```

### Inverting the Address
To invert the lines selected use the `!` flag after the address. As a result, the two following shell commands are equivalent:

```bash
sed '/include/!d' nginx.conf
sed -n '/include/p' nginx.conf
```
## Using Multiple Commands in a sed Script

You can separate different sed scripts in one line with `;` or with `-e`:
```bash
# Delete all lines from 1 to 10 and from 15 to the end of the file
sed '1,10d;15,$d' nginx.conf
sed -e '1,10d' -e '15,,$d$' nginx.conf
```
## The Substitute Command

### General Syntax
```bash
s/<pattern>/<replacement>/[flag]
```
This command will try to match the regular expression `<pattern>` on each line of your input, and replace these matches with the `<replacement>`. To replace every match on each line (and not just the first), you can use the *g*lobal flag. Specifying a number as a flag will replace the *nth* match, and using both, like `2g` will replace from the 2nd occurrence and on.
### Reusing the Pattern in the Replacement
With `&` you can include the original match, in the replacement, which will now simple append the replacement field.
You could also use regexes with [grouping and backreference](https://thevaluable.dev/regular-expression-basics-vim-grep/#grouping-and-backreference).

> [!example]
> ```bash
> sed -E 's/(ind)(ex)/\1-page-\2/' nginx.conf
> ```
> We use here the extended regular expression engine to avoid escaping the parentheses.
> 
> |   |   |
> |---|---|
> |**input**|`index index.html index.htm index.php`|
> |**output**|`ind-page-ex index.html index.htm index.php`|

### Using Different Separators

Using the slash `/` as a separator between your pattern, your replacement, and the optional flags can bring some problems. If you work with a pattern (or a replacement) which has already some slashes (like URLs), you’d need to escape every single one of them for sed to understand what slashes are separators and what slashes aren’t. We can however use other characters as separators like `|`, `%` or `#`.
### Only Displaying the Replaced Lines
Simple add `p` as a flag to the substitute command: `sed -n 's/index/page/gp' nginx.conf`
### Special Sequences
| Sequence | Description                                                                  |
| -------- | ---------------------------------------------------------------------------- |
| `u`      | The next character becomes uppercase                                         |
| `l`      | The next character becomes lowercase                                         |
| `\U`     | All characters following this sequence become uppercase, until the next `\E` |
| `\L`     | All characters following this sequence become lowercase, until the next `\E` |
| `\E`     | Stop the case conversion started by `\U` or `\L`                             |

> [!example]
> ```bash
> sed -E 's/(in)dex/\U\1\E-\u&-page/2g' nginx.conf
> ```
> 
> |   |   |
> |---|---|
> |**input**|`index index.html index.htm index.php;`|
> |**output**|`index IN-Index-page.html IN-Index-page.htm IN-Index-page.php;`|
