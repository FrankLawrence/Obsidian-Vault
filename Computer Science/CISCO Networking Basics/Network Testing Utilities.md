---
Tags: 
Created: 2024-08-14 00:35:16
---
(Links:: [[CISCO Networking Basics]])
**Troubleshooting Commands**  
A number of software utility programs are available that can help identify network problems. Most of these utilities are provided by the operating system as CLI commands.

Some of the available utilities include:

- **ipconfig** - Displays IP configuration information.
- **ping** - Tests connections to other IP hosts.
- **netstat** - Displays network connections.
- **tracert** - Displays the route taken to the destination.
- **nslookup** - Directly queries the name server for information on a destination domain.

The **ipconfig** command is used to display the current IP configuration information for a host. Issuing this command from the command prompt will display the basic configuration information including IP address, subnet mask, and default gateway.

The command **ipconfig /all** displays additional information including the MAC address, IP addresses of the default gateway, and the DNS servers. It also indicates if DHCP is enabled, the DHCP server address, and lease information.

If IP addressing information is assigned dynamically, the command **ipconfig /release** will release the current DHCP bindings. **ipconfig /renew** will request fresh configuration information from the DHCP server. A host may contain faulty or outdated IP configuration information and a simple renewal of this information is all that is required to regain connectivity.

Probably the most commonly used network utility is ping. Most IP enabled devices support some form of the **ping** command in order to test whether or not network devices are reachable through the IP network. When a ping is sent to an IP address, a packet known as an echo request is sent across the network to the IP address specified. If the destination host receives the echo request, it responds with a packet known as an echo reply. If the source receives the echo reply, connectivity is verified by the reply from the specific IP address.

---
References: