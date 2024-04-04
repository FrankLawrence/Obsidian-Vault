Status:
Tags: [[Programming]]
Links:
___
## Metaprogramming
## Debugging and Profiling
## Version Control
## Command-line enviroment
## Data Wrangling
## Editors
### Movement

You should spend most of your time in Normal mode, using movement commands to navigate the buffer. Movements in Vim are also called “nouns” because they refer to chunks of text.

- Basic movement: ```hjkl``` (left, down, up, right) 
- Words: ```w``` (next word), b (beginning of word), e (end of word)
- Lines: ```0``` (beginning of line), ```^``` (first non-blank character), ```$``` (end of line)
- Screen: ```H``` (top of screen), ```M``` (middle of screen), ```L``` (bottom of screen)
- Scroll: ```Ctrl-u``` (up), ```Ctrl-d``` (down)
- File: ```gg``` (beginning of file), ```G``` (end of file)
- Line numbers:```:{number}\<CR>``` or ```{number}G``` (line {number})
- Misc: ```%``` (corresponding item)
- Find: ```f{character}```, ```t{character}```, ```F{character}```, ```T{character}```
	- find/to forward/backward {character} on the current line
	- , / ; for navigating matches
- Search: /{regex}, n / N for navigating matches
### Edits

Everything that you used to do with the mouse, you now do with the keyboard using editing commands that compose with movement commands. Here’s where Vim’s interface starts to look like a programming language. Vim’s editing commands are also called “verbs”, because verbs act on nouns.

- ```i``` enter Insert mode
	- but for manipulating/deleting text, want to use something more than backspace
- ```o``` / ```O``` insert line below / above
- ```d{motion}``` delete {motion}
- e.g. ```dw``` is delete word, ```d$``` is delete to end of line, ```d0``` is delete to beginning of line
- ```c{motion}``` change {motion}
	- e.g. ```cw``` is change word
- like ```d{motion}``` followed by ```i```
- ```x``` delete character (equal do ```dl```)
- ```s``` substitute character (equal to xi)
- Visual mode + manipulation
	- select text, ```d``` to delete it or ```c``` to change it
- ```u``` to undo, ```<C-r>``` to redo
- ```y``` to copy / “yank” (some other commands like ```d``` also copy)
- ```p``` to paste
- Lots more to learn: e.g. ```~``` flips the case of a character

### Counts

You can combine nouns and verbs with a count, which will perform a given action a number of times.

- ```3w``` move 3 words forward
- ```5j``` move 5 lines down
- ```7dw``` delete 7 words

### Modifiers

You can use modifiers to change the meaning of a noun. Some modifiers are ```i```, which means “inner” or “inside”, and ```a```, which means “around”.

- ```ci(``` change the contents inside the current pair of parentheses
- ```ci[``` change the contents inside the current pair of square brackets
- ```da'``` delete a single-quoted string, including the surrounding single quotes

## [[Bash scripting|Shell Tools and scripting]]
## Security and Cryptography
___
References:
https://missing.csail.mit.edu/
Created: 2022-05-25 02:40