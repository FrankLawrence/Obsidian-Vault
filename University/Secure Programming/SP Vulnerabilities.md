---
Tags: 
Created: 2023-10-02 17:23:25
Links: "[[Secure Programming]]"
---
(Links:: [[Secure Programming]])
# What can go wrong?
There exists three types of memory: local variables on the [[Stack]], heap memory allocated by programmer, and global memory used for storing global and static variables. On the stack we store
- function parameters
- local variables
- return address - where to continue after return
- frame pointer - where is caller's stack frame
- other temporary storage needed by compiler
```c
int global1 = 42;
void func(void) {
    static int global2 = 42;
    int stack = 42;
	int *heap = malloc(sizeof(int));
	*heap = 42;
}
```
- **careful**: heap is stored on the stack

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
A buffer overflow belongs to the group of memory errors. There are many other groups which are listed below.
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
Authorization is the process of determining which resources the user may access. An example would be a web user trying to gain access to unprotected files on a server: `http://example.com/image.php?../../../../etc/passwd`
```php
<!php
header("Content-Type: image/jped");
$data = file_get_contents("images/" . $_GET["name"]);
print($data);
!>
```
## Injection Vulnerabilities
Programs are designed to build code that can be executed. Some times, this code can come from an input field. If the input isn't carefully sanitized, then an attacker could write the code.
## Race Conditions
Race conditions are situations where the timing of events makes a difference in the outcome. The time-of-check-to-time-of-use (TOCTTOU) vulnerability is typical:
```python
if(access(path, R_OK) == 0) {
	print_file(path);
}
```
If `path` is a symlink under our control, we can change the symlinks between `access` and `print_file` to print the file we wouldn't have access to.
## Side Channels
Attackers can get information about the system by observing unintended differences. For example timing, power consumption, electromagnetism,...Ex:
```php
userid = Query("SELECT userid FROM users " +
  "WHERE username=?", username);
if (userid == null) return false;
password_db = Query("SELECT password FROM passwords " +
  "WHERE userid=?", userid);
return password_db == password;
```
In this example, if the user exists, then the request would take longer. Side channels are *very* hard to get rid of.
## Unsecured Communications/Storage
We must assume that network traffic can be intercepted and data can be leaked, the impact of this must then be minimized. This means that we must make network traffic unreadable but also unforgeable for third parties. Minimizing the amount of stored sensitive data can help.
## Users
Unaware users are probably the biggest vulnerability in existence. Since your system is only as weak as your weakest link, we must educate users. Potential threats could be 
- phishing
- scams
- malware
- social engineering
- weak passwords

Users are ready to ignore security warnings/policies if it is more inconvenient to them. They have especially poor awareness of trustworthiness of information if they are in a hurry. *Users believe anything they see*.
# Questions
error = Stackoverflow
fault & vulnerability = missing size check
exploit = 65 or more characters & FILENO at least 65
impact = arbitrary code execution with privilege escalation
fix it = add bound check

---
References: