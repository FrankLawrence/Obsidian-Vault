---
Tags: 
Created: 2023-10-02 17:23:25
Links: "[[Secure Programming]]"
---
# What can go wrong?
There exists three types of memory: local variables on the [[Stack]], heap memory allocated by programmer, and global memory used for storing global and static variables.

If values are stored on the stack in an uncontrolled manner (such as when saving too much information in a buffer), the return address or frame pointer can be overwritten. It is the programmer's responsibility to check for *array bounds*. Attacker's can take advantage of this and return to a different memory location.
# Terminology
- **Fault**: An incorrect step, process, or data definition in a program.
- **Error**: The difference between a computed, observed, or measured value or condition and the true, specified, or theoretically correct value or condition. 
- **Failure**: The inability of a system or component to perform its required function within the specified performance requirement.

Programmers make mistakes which lead to software faults. Those with faults that impact security are known as **vulnerabilities**. Attackers can take advantage of vulnerable code by using inputs (**exploits**) that trigger an error.
The impact of the exploit can vary: 
- **Denial of service**: Interruption of [[CIA Security Triad#^89ca31|availability]]
- **Privilege escalation**: Breaking [[CIA Security Triad#^001825|confidentiality]] and/or [[CIA Security Triad#^a67d8f|integrity]]

We usually are more concerned about privilege escalation since programs can always be restarted. 
The attacker can either perform injected code or code that's already in the program on behalf of the owner or another user with more/different access rights.
# Overview and vulnerabilities
A buffer overflow belongs to the group of memory errors. There are many other groups such as (but not exclusive to):
- Authentication erros
- Injection vulnerabilities
- Race conditions
- Side channels
- Unsecured communication/storage
- Users
- and even: hardware vulnerabilities 
## Memory Errors 
There are many types of memory errors such as:
- Buffer underflow
- Buffer over-read
- Integer overflow
- Unterminated string
- Uninitialized string
- Uninitialized variable
- Use-after-free
- Double free
- Memory leak
- Format string bug
- Type confusion

Reason is that programmers using low level languages have to manually manage memory, allowing for efficient but potentially harmful code with bugs.
## Authentication Errors
Authentication is the process of determining the user's identity. This can easily be exploited if the user uses default usernames and passwords. 
## Authorization Errors


---
References: