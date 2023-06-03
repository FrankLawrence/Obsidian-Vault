---
Tags: 
Created: 2023-06-02 13:46:45
---
(Links:: [[Computer Networks]])
# 7.6
Describe the process of client mapping, by which some part of the DNS infrastructure would identify a content server that is close to the client that issued the DNS query. Explain any assumptions involved in determining the location of the client.

To enable this, the authoritative DNS server must know where in the world, geographically, the client is located to return a server that is close by. Sometimes it is sufficient to get the IP address of the local DNS resolver, but sometimes the client is further away. EDNS0 CS therefore uses the included IP subnet of the query to find the clients location.
# 7.12
The number of companies with a Web site has grown explosively in recent years. As a result, thousands of companies are registered in the com domain, causing a heavy load on the top-level server for this domain. Suggest a way to alleviate this problem without changing the naming scheme (i.e., without introducing new top-level domain names). It is permitted that your solution requires changes to the client code.

A popular method to solve this is to use CDN (Content delivery Networks) cache nodes. Copy's of webpages are placed in nodes at different locations where nearby clients then have access. This means that not all clients will be fetching the page from the origin server, reducing the load. Instead of holding a webpages however, the servers would hold the addresses of the domains under the '.com' domain. These servers can also determine the address of websites that are close to the client, instead of all, by looking at their IP address.
# 7.18
A 100-byte ASCII string is encoded using base64. How long is the resulting string?

100 bytes = 800 bits
800/6=133,333...
This means we have 133 character with an additional two bits.
We need to use two '=' as padding to add an additional four bits to create the last character.
The string has 134+2=136 characters.
Using ASCII this would evaluate to 136 bytes.
# 7.24
IMAP allows users to fetch and download email from a remote mailbox. Does this mean that the internal format of mailboxes has to be standardised so any IMAP program on the client side can read the mailbox on any mail server? Discuss your answer.

Yes, IMAP has a list of commands that are sent to the server by the user agent, which then performs the actions based on the command given. The commands are given in text format. If it were to change, then the IMAP (or even POP3) commands wouldn't work.
# 7.30
The If-Modified-Since header can be used to check whether a cached page is still valid. Requests can be made for pages containing images, sound, video, and so on, as well as HTML. Do you think the effectiveness of this technique is better or worse for JPEG images as compared to HTML? Think carefully about what ‘‘effectiveness’’ means and explain your answer.

No it's not effective since the individual bits and bytes in a JPEG image won't change but rather the entire image itself. Therefore it's not necessary to check if parts of the image have changed.
# 7.36
A server hosting a popular chat room sends data to its clients at a rate of 32 kbps. If this data arrives at the clients every 100 miliseconds, what is the packet size used by the server? What is the packet size if the clients receive data every second?

10 packets arrive every seconds which means that the packets must be 3,2 Kb large. If they arrive every seconds, then they are 32 Kb large.
# 7.42
What is the bit rate for transmitting uncompressed 1200 × 800 pixel colour frames with 16 bits/pixel at 50 frames/sec?

$1200\times800=960000$ pixels
$960000\times16=15.360Kb$ per frame
$15.360Kb \times50=768Mbps=96MB/s$
# 7.48
Consider a 50,000-customer video server, where each customer watches three movies per month. Two-thirds of the movies are served at 9 P.M. How many movies does the server have to transmit at once during this time period? If each movie requires 6 Mbps, how many OC-12 connections does the server need to the network?

$50.000\times3\times\frac{2}{3}=150.000\times\frac{2}{3}=100.000$ movies at 9 P.M.
Since it's distributed over 1 month: $100.000/30=3.333,333$
OC-12 has a data rate of roughly 600 Mbps
$3.333,33\times6/600=33,333..$
We need at least 34 OC-12 connections
# 7.54
Explain some reasons why a BitTorrent client might cheat or lie, and how it might do so.

BitTorrent rewards peers that are seeding by providing them with other peers that have high upload rates, allowing the peers to have fast download rates. Clients can lie about the rarity and the chunks of a file or even the ratio of upload to download to get higher download speeds.

---
References: