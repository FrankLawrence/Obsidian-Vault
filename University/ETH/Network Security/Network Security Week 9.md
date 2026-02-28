---
Tags: exercise
Created: 2026-01-21 20:58:05
---
(Links:: [[Network Security]])
# Exercise sheet 9: Firewalls & Middleboxes (26 points)

### Question 1 (6 points)
We'll begin with some knowledge questions.

**1.1.** (2 points)
> Briefly list reasons why middleboxes are referred to as "being harmful".

_Solution_:
- Violation of E2EE security
- Increased attack exposure
- Ossification of the internet and difficulty of introducing new protocols
- Performance bottlenecks
- Harder transparency and debugging of network problems

**1.2.** (2 points)
> Someone suggests that firewalls are not needed if all communication is
> properly encrypted.
> 
> Do you agree? Explain briefly your answer.

_Solution_:
No. Encryption provides a solution for confidentiality, and usually also integrity
(e.g. eavesdropping and data modification). However, it does not safeguard
network devices from vulnerabilities.

**1.3.** (2 points)
> What are the primary differences between a firewall and an intrusion
> detection system (IDS) as middleboxes in terms of their operation and
> placement in a network?

_Solution_:
A firewall actively blocks or allows traffic based on rules, acting as a
gatekeeper typically at the network perimeter. An IDS, on the other hand,
monitors traffic for suspicious patterns and alerts administrators but
does not block traffic directly. IDS devices often sit inside the network
for monitoring, while firewalls are usually on the border.

### Question 2 (12 points)
**2.1.** (4 points)
Shodan[1] is a web service that monitors devices connected to the
Internet. It scans IP addresses and tries to get information about their
location, their users and their possible security vulnerabilities. It’s
like a search engine for devices on the Internet. For instance,
below is a screenshot of the results page when querying for Swiss IP
addresses that expose a service that's vulnerable to Heartbleed.


![Screenshot of Shodan Results](assets/Shodan_Screenshot.png)

**2.1.1.** (2 points)
> Shodan is advertised as a useful tool to monitor your devices and ensure
> they are not accessible from unauthorized people/entities. Also, you can
> for example collect usage data for your products. Next to these positive
> usages, can you think of any negative aspects this platform inherently
> brings?

_Solution_:
Although Shodan does not do anything illegal, it might expose
information that people wish to keep hidden. Also, it is a very useful
database for attackers, say for example in case of new zero-day
exploits.


However, it must be said that Shodan does not expose any new
information: an experienced attacker would probably just use his usual
tools (NMAP etc) to scan the network, regardless of the existence of
Shodan. Also, to unlock all the potential of the engine, you need to
make an account, which is not ideal for any attacker.

**2.1.2.** (2 points)
> Refresher about NATs and firewalls: is a camera mapped by Shodan if it’s
> behind a common home router? Does it make the situation better?

_Solution_:
Devices are visible if their ports are forwarded with port forwarding in the
NAT, or if there is a remote management protocol that bypasses the NAT
barriers. It doesn’t make the situation better as far as privacy is concerned,
but the potential attack surface is reduced.

**2.2.** (8 points)
In January 2012, a blog post[1] brought many enthusiastic customers of
IP cameras back to reality. The blogger unpacked the firmware of some
cameras by US-based company Trendnet, only to discover a way that
allowed him to view the video streams.


[1] <http://console-cowboys.blogspot.com/2012/01/trendnet-cameras-i-always-feel-like.html>

**2.2.1.** (3 points)
> What was the problem with the cameras? Does changing the default
> password help?

_Solution_:
The root directory of the camera’s server had, next to the management
directory, another script called `mjpg.cgi`. This script, accessible at
`https://IP_ADDR/anony/mjpg.cgi`, streamed the captured video in real-time
without the need of any authentication. Of course, changing passwords did
not help (but changing them is still always a good idea).

**2.2.2.** (2 points)
> In this context, what was Shodan misused for?

_Solution_:
The capabilities of Shodan, together with the fact that the blogpost
mentioned the engine, brought some curious readers to peak into the
cameras of hundreds of clueless user. These peekers were able to see the
private life of many families: feeding a cat, leaving and returning
home, taking care of their infant child, etc. And not to mention,
cameras can sometimes videotape less innocent things.

**2.2.3.** (3 points)
> The company released updates to the firmware in order to fix the problem
> and the spokesperson invited customers to update in an interview. Is
> this enough? What is the weakest link in this fixing strategy?

_Solution_:
The weakest link is undoubtedly the user. In the case of IP cameras, the
user is not necessarily computer-literate, and will inevitably forget to
update, or not know that a vulnerability needs to be fixed. This is not
enough, as many devices will never get updates and remain vulnerable.
On the other hand, would an automatic update system be the best solution
here? Updates patch vulnerabilities, but they can introduce new
problems. Consider a nuclear power plant control software: do you really
want automatic updates?.


The best solution would be to nudge the user to update: automatically
send a notification to the phone via the IPCam app, showing the
changelog and a confirmation or refuse button. Upon confirmation, the
update is installed automatically, with no further user action required.

### Question 3 (4 points)
Airport Wi-Fi

**3.1.** (2 points)
> In an airport, you successfully connected to the free Wi-Fi network, but internet access is elusive 
> due to a firewall that selectively blocks all traffic except for DNS and ICMP packets. 
> How can one navigate this limitation to establish a connection to the internet?

_Solution_:
1. DNS/ICMP Tunneling Service:
DNS/ICMP tunneling involves encapsulating non-DNS/ICMP traffic within DNS/ICMP packets. 
Tools (e.g. IODINE [1], icmptunnel [2]) can create a covert communication channel, 
allowing you to route your internet traffic through DNS/ICMP.
[1] https://github.com/yarrick/iodine
[2] https://github.com/DhavalKapil/icmptunnel

2. Utilize a VPN with DNS or ICMP Support:
Certain VPN services offer the ability to operate over DNS or ICMP, 
providing a secure way to bypass restrictive firewalls.
Subscribe to a VPN service that explicitly supports DNS or ICMP encapsulation.
Connect to the VPN, enabling you to access the internet through the encrypted tunnel.

**3.2.** (2 points)
> ICMP packets (commonly associated with "ping") lack source/destination
> port information.
> How do NAT boxes and firewalls manage to block or allow these packets
> despite the absence of traditional port-based filtering?

_Solution_:
While ICMP packets don't involve traditional source/destination port
information, firewalls and NAT can keep track of number of ICMP sessions,
ensure proper stateful handling of ICMP replies, or set configurable
rules by using other ICMP information(e.g. src/dst IP addr, ICMP type,
code, etc.).

These mechanisms allow for effective security policies and the management
of ICMP communication within a network.

[3] detailed exploration of NAT for ICMP in Linux: https://devnonsense.com/posts/how-does-linux-nat-a-ping/

### Question 4 (4 points)
> The Cyber Kill Chain is a framework describing seven different steps of a successful cyber attack, used to measure the effectiveness of security controls in an organization.
> 
> The steps are:
> 
> 1. Reconnaissance: harvesting information about the attack target
> 2. Weaponization: creating the attack payload
> 3. Delivery: transmission of the attack to the target
> 4. Exploitation: exploiting a vulnerability on the target system to run the attack
> 5. Installation: if necessary, install malware onto the target system
> 6. Command & Control: establish 2-way communication with the attacker for them to remotely control
> 7. Actions on Objectives: attacker now has a foothold, and can move on to their original attack objectives
> 
> How does the Zero Trust model break the cyber kill chain and prevent attacks? Identify some of the steps above and describe how Zero Trust improves on defence compared to the traditional network architecture (where trust is based on segregation.)

_Solution_:
Zero Trust differs from the traditional network architecture based on segmentation, by explicitly authenticating at every network boundary every time. It uses various authorization mechanisms like physical multi-factor authentication keys, dynamic machine policies (e.g. OS updates must be installed to connect), and Identity Provider-based access controls.

The Cyber Kill Chain is a broad framework that tries to describe common steps attackers use to perform a cyber attack. It's often used by organizations to check each step of the chain and apply defences for each one.

Exploitation: Exploiting a vulnerability can be made more difficult when machines are required to be up-to-date or have other dynamic policies. e.g. Perhaps it's harder to have executable attachments on email clients.

Installation: Similar to above, with up-to-date security policies in place it would be difficult for attackers to install malware onto the host system to establish C&C.

Command & Control: Establishing a 2-way communication channel with the attacker from the victim machine will be impossible without getting the victim user to actually authenticate and authorize it physically.

Actions on Objectives: Performing any kind of lateral movement within the organizaiton network will require physical authorization from the victim user. Further, the IdP-based access controls means that "foothold" users often don't have the privileges to cause any kind of harm.



---
References: