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
    
Hint: A good attacker model usually answers the following questions: Who is the attacker/adversary? Which devices or entities are trusted? Which software is assumed secure? What can the adversary do? What can the adversary not do? What does the adversary want to achieve? It might help to consider the stakeholders involved, assets that need to be protected, and applicable security goals.

> [!question] A smartphone application monitoring a user’s daily walked kilometers.

> [!question] A banking website hosted by a cloud service provider.
> 

> [!question] A smart home equipped with temperature sensors and automatic temperature control. 
> children

> [!question] A gaming console that only runs games approved by the manufacturer.
> User or game developer might gain hardware access.

> [!question] A car that opens and starts when the car key is close.

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

> [!question] Isnobody.laindabletocreateadirectoryexercise2under/home/capkun/syssec/part1/?

> [!question] 1. What can you infer about the creation of the directory exercise1 that is already present?
> Modified by the owner

> [!quesiton] 1. Given that a user [group].navip created results without a problem, what can you infer about the value of group?
> part1

---
References: