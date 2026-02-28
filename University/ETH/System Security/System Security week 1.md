---
Tags: 
Created: 2025-09-25 16:21:27
---
(Links:: [[System Security]])
# Basic Concepts
In this task, we review some basic security concepts.  
> [!question] Explain the security goals Confidentiality, Integrity, Availability, Authentication, and Authorization. For each goal, think of an example scenario where the goal is important.
> - Confidentiality
> - Integrity
> - Availability: 
> - Authentication: Verifying what role a party has
> - Authorization: What the authenticated used can do

> [!question] What are the differences between symmetric and asymmetric cryptography? What are their respective advantages and disadvantages? Why do we use both symmetric and asymmetric cryptography?
> - asymmetric is very slow

# 2 Attacker models

You might have heard of attack models for cryptanalysis before, such as Ciphertext-only, Known plaintext, Chosen plaintext, Chosen ciphertext, Chosen cipher- and plaintext. In a ciphertext-only attack, the attacker has only access to ciphertexts created under one encryption key, and the goal is to either find out the corresponding plaintexts or even the encryption key itself. An example of a ciphertext-only attack would be a device in a cafe capturing the packets of a wireless HTTPS connection and trying to retrieve the plaintexts or the key later. The attacker only has access to the captured encrypted packets and does not have access to the user’s computer or the server.

 > [!question] Give similar descriptions and real-world examples for the rest of the attack models.
 > - known plaintext: has access to the unecrypted part of some cyphertext
 > - chosen plaintext (more stronger adversary than previous): mallory can encrypt their own text
 > - chosen cyphertext: adversary gains access to encrypted message with specific key
    
 > [!question] What is the weakest attack model, and what is the strongest attack model? Justify your answer.

In system security, we use similar models to design systems and analyze their security. Often, such attacker models are tailored to the type of system under consideration and based on assumptions about the deployment scenario. They specify what role the attacker has in the system, which capabilities they have, and what their goals are. Think of one or multiple reasonable attacker models for each of the following systems.
    
Hint: A good attacker model usually answers the following questions: 
- Who is the attacker/adversary? 
- Which devices or entities are trusted? 
- Which software is assumed secure? 
- What can the adversary do? 
- What can the adversary not do? 
- What does the adversary want to achieve? 
It might help to consider the stakeholders involved, assets that need to be protected, and applicable security goals.

> [!question]- A smartphone application monitoring a user’s daily walked kilometers.
> - Possible attackers: another person holding the phone, app developer, developer of other apps, operating system,...
> - Assets & security goals: Confidentiality and integrity of user data, but possible also things such as the integrity of the phone or other apps
> - Trusted entities: App developer, OS, user, phone,..
> - Attacker goal: Retrieving data collected by the app, retrieving user information that should not be accessible to the app/tracking the user, compromising the phone...
> - Attacker capabilities: Physical access to the phone, exploiting possible vulnerabilities of the OS or phone, app simply misbehaving and exhibiting functionality that is different from the specified functionality, ...

> [!question]- A banking website hosted by a cloud service provider.
> - Possible attackers: Cloud provider (as a whole or malicious employee), user, third parties wanting to gain access, network intermediaries, law enforcement,...
> - Assets & security goals: Confidentiality and integrity of user data, financial assets, and transactions, availability of web service
> - Trusted entities: user, server hardware, website software, second factors for authentication
> - Attacker goal: Tampering with payment information, retrieving user data, Denial of Service of the web server, gaining access to financial assets
> - Attacker capabilities: Physical access to cloud servers, subpoenas, interfering with network traffic, sending arbitrary requests to the server, sending phishing emails,...

> [!question]- A smart home equipped with temperature sensors and automatic temperature control. 
> children

> [!question]- A gaming console that only runs games approved by the manufacturer.
> User or game developer might gain hardware access.

> [!question]- A car that opens and starts when the car key is close.

# Unix Access Control

This task is about access control in Unix-based systems. If you do not have any experience with this, you might find this a helpful starting point: https://en.wikipedia.org/wiki/ File-system_permissions#POSIX_permissions.

A short refresher: File and directory permissions are denoted with nine bits. Each one of the bits represents a flag. The first three represent whether the owner of the file has read (r), write (w), and execute (x) access to a file or directory. The following six bits represent the corresponding access flags for the file group and all other users. Access permissions mean the following:

- Read (r) access means that a user is permitted to read a file or to list a directory.

- Write (w) right means that a user can modify a file or delete/add files to a directory.

- Execute (x) access represents the right of running a file as a program or traversing through a directory.

The nine bits are usually represented either in octal mode or as letters.

> [!question] Consider a file owned by user A with protection mode 644 (octal) contained in a directory with protection mode 777. How can user B compromise this file? Name a scenario in which user B could take advantage of this.
> User B can replace teh file

Given the following table of UNIX directory/file permissions, and assuming that in what follows a string `group.username` denotes a user with username `username` and `group` identity group, answer the following questions.

> [!question] Is `nobody.laind` able to create a directory `exercise2` under `/home/capkun/syssec/part1/`?
> No, `nobody.laind` has no w-access to the directory `part1`

> [!question] What can you infer about the creation of the directory `exercise1` that is already present?
> It appears to be that either fried, who is the owner of `part1`-directory, created the subdirectory `exercise1` and someone with the appropriate rights changed its ownership to `laind` or that the directory permissions were changed after the directory was created.

> [!question] Given that a user `[group].navip` created `results` without a problem, what can you infer about the value of `group`?
> Group is `part1`

> [!question] Which of the following commands will succeed when executed individually?
> 1. `nobody.fried`: `echo "grades 2023" > /home/capkun/syssec/grades/grades2023.txt`
>    No
> 2. `nobody.markk`: `ls /home/capkun/syssec/exercises`
>    Yes
> 3. `admin.ynaka`: `echo "exercise1" > /home/capkun/syssec/exercises/ex1.txt`
>    Yes
> 4. `admin.ynaka`: `chmod 007 /home/capkun/syssec/exercises/`
>    No, `ynaka` is not he owner of the folder
> 5. `nobody.fried`: `chmod 007 /home/capkun/syssec/exercises/`
>    Yes
> 6. `part2.markk`: `ls /home/capkun/syssec/part1/exercise1/results`
>    No
> 7. `nobody.fried`: `rm /home/capkun/syssec/part1/exercise1/results/*`
>    No, `fried` does not have any permissions for the directory results. `r` and `x` permissions would be required to perform the `*` wildcard expansion, `w` and `x` would be required to remove files
> 8. `nobody.supra`: `touch /home/capkun/syssec/exams/exam2023.txt`
>    No
> 9. `faculty.shinde`: `ls /home/capkun/syssec/exams`
> 
> Assuming that `laind` has read access to `/home/capkun/syssec/exercises/*`:
> 10. `nobody.laind`: `cp /home/capkun/syssec/exercises/* /home/capkun/syssec/part1/exercise1/`
> 11. `part1.laind`: `cp /home/capkun/syssec/exercises/* /home/capkun/syssec/part1/exercise1/`

# Permission Delegation: `setuid`, `sudo`, `su`
> [!question] What is the difference between the `sudo` and `su` commands in Linux? Which would you use to enable temporary root access and why?
> The sudo command is used to gain root access using the user's own password. The `su` command is used to switch between users using the target user's password, i.e., if alice executes "su bob", then alice will have to enter bob's password. Of course, it is possible to "su root" instead of using `sudo`, but this requires distributing the root password among users, which is not desirable from a security perspective. Therefore, using `sudo` is better than using `su` for granting temporary root access to different users. Additionally, sudo allows more fine-grained configuration and, thereby, more selective privilege escalation. This follows the least privilege principle.

> [!question] Which users can `sudo` on a Linux system?
> The list of users who can sudo is specified in a configuration file. It is usually under `/etc/sudoers`

> [!question] How to `setuid` and the `setuid` bit work on a Linux system?
> `setuid` allows users to run an executable with the file system permissions of the executable's owner (there exists a `setgid` flag that allows to do the same for the owner's group). It is used to allow users to run programs with temporarily elevated privileges that the executable owner *delegates* to the user: Any user able to execute the file will execute the file with the privileges of the file owner (e.g., root)

We have provided a script to set up the environment of the next task. If you want, you can run `acsetup.sh` as root on a Linux system. 
# POSIX permissions
## Read (r)
- for files: can read file contents (not its name or metadata, which are determined by the permissions of parent dir)
- for dir: read dir entry names of contained files and dirs, but not metadata or content (x permission needed)
## Write (w)
- files: modify file contents
- dirs: modify entries (create, delete and rename files or dirs), but x needed as well
## Execute (x)
- files: execute file (r permission needed)
- dirs: read metadata of files and dirs within
	- dir without execute blocks reading and writing
	- dir with x but without r is guessing game for contents of contained files and dirs

---
References: