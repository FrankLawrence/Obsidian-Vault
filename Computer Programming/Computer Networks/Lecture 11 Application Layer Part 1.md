---
Tags: lecture
Created: 2023-05-31 13:33:56
---
(Links:: [[Lecture 10 Transport Layer Part 2|Lecture 10]] <- [[Computer Networks]] -> [[Lecture 12 Application Layer Part 2|Lecture 12]])
Provides functions needed by users
# Domain Name System
- Application used by the network itself
- Machines on the internet are identified by their **IP address**
- DNS translate **human readable names** to IP addresses
- Top level domains controlled by Internet Corporation for Assigned Names and Numbers (ICANN)
	- Organisations can request second-level domains from **registrars**
	- Countries have their own second-level domains
	- '.edu' and '.gov' are generic domains, but mostly used by organisations in the United States
- If you control a domain, you can specify arbitrary subdomains (e.g. ac.uk. or co.uk.)
- You ask a **name server** for the name of an IP address

> [!question]- How does a machine know where to find the name server?
> There are root server addresses hardcoded into the machines operating system

- Hosts learn about the location of name servers via [[Lecture 8 Network Layer Part 2#Dynamic Host Configuration (DHCP)|DHCP]]
- Machines query a local name server
	- Local name server asks the root name server for the location of the top level domain -> ask next name server (recursive)

> [!example]
> ```mermaid
> graph TD
> a[Alice] -- 'rooster.vu.nl' in browser --> b[Alice's machine]
> b -- Query --> A[Local name server]
> A --> b
> b --> a
> A -- rooster.vu.nl? --> B[Root name server]
> B -- .nl. name server --> A
> A -- rooster.vu.nl? --> C[NL name server]
> C -- .vu.nl. name server --> A
> A -- rooster.vu.nl? --> D[VU name server]
> D -- rooster.vu.nl. is at a.b.c.d --> A
> ```

## Content Delivery Networks
- popular content can be moved to servers closer to users than the original server
- **Front end** forwards requests and distributes load
- **DNS** can be used for load balancing
# Email
- Messages contain:
	1. An envelope
	   ```
	   From: Alice@mail.com
	   To: Bob@mail.com
	   Encryption: None
	   ```
	2. A header
	   ```
	   From: Alice@mail.com
	   To: Bob@mail.com
	   Subject: How does email work?
	   ```
	3. A body
	   ```
	   Hi Bob,
	   
	   I really want to know how email works.
	   Do you know of any CS courses I could to learn more about it?
	   ```
- Multipurpose Internet Mail Extensions
	- People want to send more than just **plain text** messages
	- Additional header introduced (e.g. Content-Type) to indicate type of data in message
	- **Multipart**: Used to create messages with multiple data types
- Servers used ASCII data for plain text messages -> no support for extensions

> [!question] How to we send binary via a server that can only handle ASCII?
> Use Base64 encoding!

## Base64 encoding
- group 6 bits together into an ascii character (reference ascii table)
- Alphabet: [A-Za-z0-9+/] goes from 0 to 64
- If the binary data is not a multiple of 6, add two 0's and append an equals '=' sign

## How It works
- Users use **POP3** or **IMAP** to interact with their **mailbox**
- **Message Transfer Agent** stores emails
	- Client request emails -> copy emails onto the local machine
- Users and Message Transfer Agent use **SMTP** to send email from source to a destination

> [!question] How do Message Transfer Agents know where to send the email?
> With [[#Domain Name System|DNS]] servers!

---
(Links:: [[Lecture 10 Transport Layer Part 2|Lecture 10]] <- [[Computer Networks]] -> [[Lecture 12 Application Layer Part 2|Lecture 12]])
References: