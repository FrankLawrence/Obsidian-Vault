---
Tags: 
Created: 2023-03-04 00:46:01
---
(Links:: [[Computer Organization]])
# Memory Locations and Addresses
- memory is organized so that a group of $n$ bits can be stored or retrieved in a single, basic operation -> *word*
## Byte Addressability
- use successive addresses to refer to successive byte locations in the memory
- Byte location have addresses 0, 1, 2, ...
- If word length of the machine is 32 bits, successive words are located at addresses 0, 4, 8, ..., with each word consisting of four bytes
## Big-Endian and Little-Endian Assignments
- *big-endian*: lower byte addresses are used for the more significatn bytes (the leftmost bytes) of the word
- *little-endian*: lower byte addresses are used for the less significant bytes (the rightmost bytes) of the word
- [[Byte and word addressing.png]]
## Word Alignment
- word locations have *aligned* addresses if they begin at a byte address that is a multiple of the number of bytes in a word
- for a word length of 64 ($2^3$ bytes), aligned words begin at byte addresses 0, 8, 16, ...
## Accessing Numbers and Characters
- numbers occupy one word
# Instructions and Instruction Sequencing
- *Register Transfer Notation*: right-hand side of an RTN expression always denotes a value, and the left-hand side is the name of a location where the value is to be places, overwriting the old contents of that location: $$R4 \leftarrow [R2]+[R3]$$

---
References: