---
Tags: 
Created: 2023-10-03 00:28:12
---
(Links:: [[Secure Programming]])
# Programming Languages
Programming languages can be classified into two groups: 
- **Unsafe languages** where the programmer must know what they are doing
- **Safe languages** which reduce the impact of programmer mistakes

|                        | Unsafe       | Safe                               |
| ---------------------- | ------------ | ---------------------------------- |
| Deallocation           | Manual       | Garbage collected                  |
| Integer overflow       | Wraps around | Exception                          |
| Out-of-bounds access   | Undefined    | Exception                          |
| Pointers               | Allowed      | Only references                    |
| Unsafe typecasts       | Allowed      | Disallowed or automatic conversion |
| Variable initilization | Optional     | Enforced or automatic              |

Safe and unsafe languages try to identify unsafe code at [[compile time]]. Safe languages however also add remaining checks at [[run time]]. These could be for example bounds and range checks, type conversions, garbage collection, etc... . This can lead to performance overhead. Generally, more dynamic languages are easier for the programmer, but more static languages reduce overhead.
Safe programming languages are not always an option, but when they are you should use them. They however do not always make code safe, just the memory errors.
# Software Design
In general, shorter and more simpler code has less opportunities for introducing bugs. Less code is therefore often more secure. We must only add features which are necessary, since rarely used features aren't tested as much.

Programs are most vulnerable where the user can interact with it. In order to further minimize the **attack surface** (where user input is processed), we must: 
- reduce as many interfaces with the user
- validate the untrusted data as early as possible
- restrict interfaces to authorized users
- handle the input with as much similar code as possible

When writing code, pay close attention to these interfaces and if they are needed/ which users need these interfaces.

## Compartmentalization

^2de580

Applications consist of compartments. This means that different processes cannot access each others memory. The only way these processes can interact with each other is through explicit interfaces. This also means that with correct validation at each boundary, an attacker could only control vulnerable components, and not all. 

## Principle of Least Privilege (POLP)
To minimize the probability of loss of [[CIA Security Triad#^001825|confidentiality]], every program and every privileged user of the system should operate using the least amount of privilege necessary to complete the job.
> [!example]-
> A web server starts as root to listen on port 80. We can switch to a user that can only read the needed files (no directories). Only when modify contents is needed should it have writing privileges. 

> [!example]-
> When accessing a different component a new user should be created for accessing that database. Even better would be to control the users access to stored procedures which execute specific SQL queries. 

> [!example]-
> On windows/Linux, programs are allowed to do everything the user that started then can do. On Android, each app only get specifically assigned permissions (make calls, user camera, record audio,...)
## Secure Defaults
Many users do not take the effort to change the default settings. These should therefore be as secure as possible. Firewalls should for example not allow incoming traffic by default, and only be allowed explicitly by the user for each port. 
Ubuntu goes a step further and prevents the user from logging in as root, creating only a user account with sudo access. 
## Defence in Depth
Once an attacker has breached security, he can continue performing attacks. This means taking down one barrier expands the attack surface until the next barrier. This is why we use [[#^2de580|Compartmentalization]].
## Passwords
Short and/or simple passwords may be brute forced by trying each possible password. Passwords may be leaked by vulnerable websites. If passwords are reused on other websites, then a leak on one website will break all others. In addition, if people don't change their passwords, the chance that their accounts are compromised increases.
Users are lazy and user short passwords instead of long and difficult passwords which must be written down. Through phishing and social engineering people can give out the passwords practically for free. 
Password strength is usually misunderstood. Password strength is measured in *password entropy* which is the base 2 $\log$ of the number of possible passwords.
> [!example]+
> If the pin is 4 decimal digits, there are $10^4=10000$ possibilities and the password has an entropy of $\log_{2}10000=13.3$. With 4 characters there are $26^{4}$ possibilities.

In order to prevent hackers from brute forcing the password, we could place mechanisms such as blocking account after too many failed passcode events. The amount of failed attempts can vary depending on the password entropy. 
Since password are often leaked and hard for the user, [[Multi-factor Authentication]] is used as another barrier for attackers. 

---
References: