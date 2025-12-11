---
Tags: 
Created: 2023-10-03 00:29:34
Links: "[[Secure Programming]]"
---
(Links:: [[Secure Programming]])
> [!example] Vulnerable Example
> ```c
> char welcome[] = "Welcome to our system!";
> void echo(int fd) {
> 	long len;
> 	char name[64], reply[128];
> 	len = strlen(welcome);
> 	memcpy(reply, welcome, len);
> 	write(fd, "Type your name: ", 16);
> 	read(fd, name, 128);
> 	memcpy(reply + len, name, 64);
> 	write(fd, reply, len + 64);
> }
> void server(int socketfd) {
> 	for(;;) echo(socketfd);
> }
> ```
> Stack layout:
> ```
>   +8(%rbp) return address
>     (%rbp) frame pointer
>   -8(%rbp) len
>  -72(%rbp) name
> -200(%rbp) reply
> ```
> **Attack**: Provide name of more than 80 characters to overwrite return address
# Stack Canaries
- Place a value between local variables and return address -> Compiler adds initialization in function prologue
- Check whether value is still the same before function returns
- *Idea*: Attacker corrupting return address will also corrupt the canary
	- Only works if the attacker does not know canary or cannot write correct canary

> [!question]- Can We Still Exploit This?
> ```c
> char welcome[] = "Welcome to our system!";
> void echo(int fd) {
> 	long len;
> 	char name[64], reply[128];
> 	len = strlen(welcome);
> 	memcpy(reply, welcome, len);
> 	write(fd, "Type your name: ", 16);
> 	read(fd, name, 128);
> 	memcpy(reply + len, name, 64);
> 	write(fd, reply, len + 64);
> }
> void server(int socketfd) {
> 	for(;;) echo(socketfd);
> }
> ```
> Stack layout:
> ```c
>   +8(%rbp) return address
>     (%rbp) frame pointer
>   -8(%rbp) canary
>  -16(%rbp) len
>  -80(%rbp) name
> -208(%rbp) reply
> ```
> **Attack**: Overwrite `len` to avoid overwriting the canary, then write pointer to shellcode over return address

The previous example does not change the canary, since it simply *jumps* over it to write to the return address.
- Corrupt pointer/index used to write to memory, to have write target address past canary
- Write additional data at new target

We can however also read the canary value, and later when overwriting the return address, use it when overwriting the canary.
- Leak vulnerability (example: buffer overread) 
- Ability to provide more input after leak  
- Buffer overflow using new input

Another option is to simply brute force guessing the value of the canary:
- $2^{64}$ possibilities on modern systems
- we can sometimes corrupt part of the canary
- some programs reuse the same canary
- Approach
	- overflow one byte and check if it does not crash; retry for next bytes
	- at most 2024 attempts

# W$\bigoplus$X
- Write xor eXecute - ensure that no memory is both writable and executable -> prevents attacker from injecting code and then executing it
	- shellcode attack does not work

## Attack
- Attacker can instead reuse existing code
- Use shared library functions (return-to-libc)
	- With 32-bit system we were able to write address of `system()` and specify `%rdi` with stack
- Chain together parts of the program into a new program (return-oriented programming, ROP)
	- small snippets of code ending with RET instruction, can be chained together into a full control flow

## Ret2libc (32 bit)
## Ret2libc (64 bit)

# Address Space Layout Randomization
- Randomizes the memory addresses of code, data, heap, and stack
- Prevents attacker from finding a code pointer to overwrite, or knowing what to overwrite it with

> [!question]- Can We Still Exploit This?
> ```c
> char welcome[] = "Welcome to our system!";
> void echo(int fd) {
> 	long len;
> 	char name[64], reply[128];
> 	len = strlen(welcome);
> 	memcpy(reply, welcome, len);
> 	write(fd, "Type your name: ", 16);
> 	read(fd, name, 128);
> 	memcpy(reply + len, name, 64);
> 	write(fd, reply, len + 64);
> }
> void server(int socketfd) {
> 	for(;;) echo(socketfd);
> }
> ```
> Stack layout:
> ```c
>   +8(%rbp) return address
>     (%rbp) frame pointer
>   -8(%rbp) canary
>  -16(%rbp) len
>  -80(%rbp) name
> -208(%rbp) reply
> ```
> **Attack**: Overwrite `len` so `reply`+`len` skips over the `retaddr` and write leaks it. Compute ROP addresses and use in the second iteration.

## Attack against ASLR
- Leak addresses OR use side channel or oracles to recover complete address space layout
- Typically ASLR only randomizes the base address of each part of the address space (code+globals, stack, heap). If we know one pointer, *we know them all*
# Control Flow Integrity (CFI)
- limits possible targets for indirect branches, such as
	- returns from functions
	- calls to function pointers
	- calls to C++ virtual methods
- changes the program, either source or binary
	- Static analysis of control flow
	- places checks on each indirect branch to ensure control flow follows the possible options according to the analysis
- Variants:
	- **Fine-grained**
		- Indirect branch can only reach targets statically known to be valid
		- most secure but slow
	- **Coarse-grained**
		- Each indirect call can only reach start of functions
		- Each return can only reach after call site
		- Faster but less secure
	- **Shadow stack**
		- Return addresses on a separate stack
		- Fast and secure, but only for returns

> [!example]-
> ```c
> void hello(char *name) { 
> 	printf("hello %s\n", name); 
> 	/* vulnerability here */
> }
> void bye(void) {  
> 	printf("goodbye\n");  
> 	/* vulnerability here */
> }
> int main(int argc, char **argv) {
> 	/* location A */
> 	hello("world");
> 	/* location B */
> 	hello(argv[1]);
> 	/* location C */
> 	bye();
> 	/* location D */
> 	return 0;
> }
> ```
> > [!question]- Where can hello return?
> > Fine-grained CFI
> > - Locations B and C 
> > 
> > Coarse-grained CFI
> > - Locations B, C, and D
> > 
> > Shadow stack
> > - First call: location B
> > - Second call: location C

## Attack against CFI
- Use incompleteness of analysis and checks
- Find ways to use the remaining code fragments
- Attack data flow rather than control flow

> [!summary] Conclusion
> - Application security is a cat and mouse game
> - These attacks are only some of the most common ones 
> 	- More advanced attacks in MSc program (Software Security)
> 	- New attacks published frequently  
> 	- No doubt, more are known only to intelligence agencies
> - More defenses are constantly developed  
> 	- Most never adopted in practice due to overhead 
> 	- Attackers always find a way to bypass them
> - Bar getting higher, but cannot stop resourceful attacker 
> 	- This is why secure programming will remain important


---
References: