---
Tags: lecture
Created: 2023-02-02 17:17:47
---
(Links:: [[Web Technology]])
# Internet history
- **Internet:** the network of networks connected via the public backbone and communicating using TCP/IP communication protocol
- ARPANET (1968):
	- one of earliest attempts to network heterogeneous, geographically dispersed computers
	- email first available on ARPANET in 1972
# Protocols: IP, TCP, UDP, DNS
- how you answer and end call, what language you speak, etc.
- designed for use both within *local area networks* and between networks
- Communication protocol: how computers talk
```ad-definition
title: **IP address:**
- 32-bit number (IPv4)
- associated with at most one device (or "host") at a time 
- written as four dot-separated bytes
- Assigned by IANA (Internet Assigned Numbers Authority)
```
- **IP** (1983)
	- Transfer data packets from **source** to **destination** host
	- IP source software create a packet representing the data
	- If destination is unknown, packet is sent to a **gateway** that connects to more than one network
- **TCP**: IP has limitations (no guarantee of packet delivery, communication is one-way)
	- adds the notion of a **connection** implemented on top of IP
	- provides guarantee that packets delivered
	- provide two-way communication
	- more like telephone than postal network
	- TCP header contains port number representing an application program on the destination computer
	- *standard meanings:* 25 -> email transmitted using Simple Mail Transfer Protocol
- **User Datagram Protocol (UDP)**
	- provides port concept
	- no connection concept
	- no transmission guarantee
	  -> lightweight, so faster for one-time messages
- **Domain Name Service (DNS)**
	- "phone book" for the internet
	- map between host names and IP addresses
	- often uses UDP for communication
	- labels separated by dots
	- final label is *top-level domain*
```ad-example
title: Analogy to Telephone Network
- IP ~ the telephone network
- TCP ~ calling someone who answers, having a conversation, and hanging up
- UDP ~ calling someone and leaving a message
- DNS ~ directory assistance
 
```
- Higher-level Protocols
	- TCP specifies how we initiate and terminate the phone call, but some other protocol specifies how we carry on the actual conversation
	- SMTP (mail)
	- FTP (file transfer)
	- HTTP (transfer of Web documents)
# Web: URI + HTTP + HTML
- World Wide Web 
	- invented CERN 1989 be Berners-Lee
	- simplify sharing research results over the internet
	- one of several systems for organizing Internet-based information 
	- support for hypertext
	- locating documents using Uniform Resourse Locators
	- Document representation using Hypertext Markup Language
	- Communication via Hypertext Transport Protocol
- HTTP: implemented over a TCP connection 
- http://www.example.org:<span style="color:deepskyblue">56780</span><span style="color:red">/a/b/c.text</span>?<span style="color:gold">t=win&s=chess</span>#<span style="color:magenta">para5</span>
	- <span style="color:orange">host</span>
	- <span style="color:deepskyblue">port</span>
	- <span style="color:red">path</span>
	- <span style="color:gold">query</span>
	- <span style="color:magenta">fragment</span>
- web browsers
	- convert web addresses URL's to HTTP requests
	- Communicate with web servers via HTTP/TCP/IP
	- render documents returned by a server

---
References: