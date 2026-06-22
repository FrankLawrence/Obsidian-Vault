---
Tags: lecture 
Created: 2026-01-12 22:42:30
Aliases: VPN
---
(Links:: [[Network Security]])

VPNs create a **secure channel** between two networks over an **untrusted network** (often the Internet).
- Setup phase: The gateways (tunnel endpoints) *authenticate* each other and *set up keys* (similar to TLS handshake)
- Tunneling phase: Packets are encapsulated at the first gateway and decapsulated at the second

> [!info] Properties of VPN tunnels
> - Similar security properties as the [[Transport Layer Security#TLS 1.3 Record Protocol|TLS Record Protocol]]
> 	- Authentication of the source (handshake), data integrity (MACs)
> 	- Secrecy (symmetric encryption)
> 	- Replay suppression (sequence numbers)
> - Not *all* tunneling protocols provide encryption or authentication

# VPN Setups
- **Setup 1**: secure a connection *between* two physically separated networks
	- Replaces private physical networks and leased lines
		- Even for leased lines, encryption may be desirable
	- Used, for example, to connect regional offices with the head office of a company over the internet
- **Setup 2**: secure a connection of a *remote host* to company/university network
	- Remote host can access resources in private network
		- Private IP addresses can be accessed without port forwarding, etc. 
		- Services do not need to be exposed to the Internet
	- First gateway located *at the host* -> all traffic between host and private network is secure
- **Setup 3**: VPN as a *"secure" proxy* (to get a different public IP address)
	- circumvent censorship (e.g. access Facebook in China)
	- Avoid tracking by your ISP or in a public WiFi network
	- Hide your IP address from websites
	- Spoof your location for online shopping, video streaming, etc.
	- Access restricted content (e.g. academic journals through EHT)
	- Download torrents (Debian images only, of course)

> [!important] VPN provider has access to metadata of all traffic

- **Setup 4**: VPN as overlay mesh network (e.g. [[Tailscale]])
	- Overlay manger performs key/configuration management on participants (each pair has their own Wireguard tunnel between each other)
	- Overlay manager assists with establishing connectivity (IP discovery, hole punching, ...)

> [!warning] VPN != Anonymity
> - VPNs provide some *limited* anonymity properties:
> 	- Local network and ISP only see that you send traffic through some VPN
> 		- They do not see which websites you access
> 	- Web servers do not see your real IP address
> 		- Of course, if you use cookies or log in, anonymity is lost
> - VPN server can monitor and record all traffic

# TLS vs VPN

> [!question]- Why do we need VPNs when we have TLS?
> - VPNS protect *all* traffic: "blanket" security
> 	- DNS requests
> 	- Access to services that do not support TLS
> - VPNs can give access to services in private networks or behind firewalls

> [!question]- Why do we need TLS when we have VPNs?
> - Data is only secure in the tunnel: no security outside of it
> - VPN server can see all unencrypted traffic -> TLS still necessary
> - With a VPN it is not possible to authenticate the webserver, only the tunnel endpoint

|                       | TLS                                        | VPN                       |
| --------------------- | ------------------------------------------ | ------------------------- |
| Secured Layer         | Transport Layer (L4)                       | Link/network Layer (L2/3) |
| Protection            | End-to-End                                 | Tunnel                    |
| Client authentication | not authenticated                          | Authenticated             |
| Diversity             | One / very few globally accepted standards | Many different protocols                          |

> [!question]- Why are there many VPN protocols, but only one for TLS
> VPN endpoint and host setup together and can choose what they want. TLS is a standard that has to be supported by *everyone*

# Availability and Performance
- VPNs can negatively impact performance
	- Additional cryptographic operations
	- Potential detours
	- Limited bandwidth at VPN server
- Generally, VPNs do *not* provide higher availability
	- No built-in defense against [[DDoS Attack]] or routing attacks
- VPNs *can* defend against targeted packet filtering
	- Routers can *recognize* VPN packets but not content
	- Would need to drop all VPN packets

# VPN vs VLAN
- VPN: (securely) **connect/combine** two different networks 
	- One virtual network over multiple physical networks
- VLAN: set up multiple **isolated virtual networks** on a single physical infrastructure
	- Virtual networks are identified by tags, which are added to Ethernet frames
	- Example protocol: IEEE 802.1Q
	- Often used in cloud-computing environments for isolating communication between VMs
- VXLAN (virtual extensible LAN) combines features from both systems 
# What types of VPNs are there?
- Generic Routing Encapsulation (GRE) [1994] 
- Internet Protocol Security ([[IPsec]]) [1995]  
- Point-to-point tunneling protocol (PPTP) [1999] 
- Layer-2 tunneling protocol (L2TP) [1999]
- OpenVPN [2001]  
- Secure Socket Tunneling Protocol (SSTP) [2007] 
- [[WireGuard]] [2016]  

VPNs can be classified based on [different categories](https://upload.wikimedia.org/wikipedia/commons/thumb/0/09/VPN_classification-en.svg/2560px-VPN_classification-en.svg.png) such as topology type, or technology used.

## Tunnel vs. Inner protocol
- **Inner protocol** refers to the protocol directly within the VPN packet; the *lowest layer* of encapsulated packet (often link or network layer)
- **Tunnel/outer protocol** refers to the *highest layer* used in additional header (often network or transport layer)

![[Tunnel vs Inner Protocol.svg|600]]
# VPN endpoint at hosts
- VPN creates virtual network adapter (e.g. `tun0`, `eth2`)
- Can be used like any other network adapter
- VPN interface can be used for all traffic or only selectively ("split tunnel")

> [!example]+ IP layer tunnel
> - VPN public IP is `129.132.99.164`
> - Before VPN connection:
> ```
> Kernel IP routing table
> Destination     Gateway          Genmask          Flags Metric Ref       Use  Iface
> 0.0.0.0         192.168.1.1      0.0.0.0          U     0      0           0  eth0
> 192.168.1.0     0.0.0.0          255.255.255.0    U     256    0           0  eth0
> ```
> - All traffic routed through normal interface sent to router with IP `192.168.1.1`
> - Traffic intended for local network sent directly, without routing
> - After VPN connection: 
> ```
> $ route -n
> Kernel IP routing table
> Destination     Gateway          Genmask          Flags Metric Ref       Use  Iface
> 192.168.1.0     0.0.0.0          255.255.255.0    U     256    0           0  eth0
> 129.132.99.164  192.168.1.1      255.255.255.255  U     0      0           0  eth0
> 0.0.0.0         10.6.208.1       0.0.0.0          U     0      0           0  tun0
> 10.6.208.0      0.0.0.0          255.255.240.0    U     0      0           0  tun0
> ```
> - All traffic (destination `0.0.0.0`) routed through VPN (`tun0` interface) to router at endpoint network (`10.6.208.1` is part of [[Reserved IP addresses|private IP address range]])
> - Gateway is the "intermediate" hop to arrive at destination
> - Data intended for VPN server (public IP `129.132.99.164`) sent over normal interface
> - Packets intended for other devices on the VPN's local network (destination `10.6.208.0`) go over `tun0` interface, but are then directly routed
> 
> ![[VPN processing at hosts.svg|800]]

---

> [!summary]
> - VPNs create secure channels on the network or link layer
> - VPNs and end-to-end security (TLS) complement each other
> - Many different VPN protocols and applications
> 	- IPsec has a long history and numerous configuration options
> 		- Very versatile but difficult to set up
> 	- WireGuard is a new VPN protocol with a focus on simplicity
> 		- Very few configuration parameters, no cryptographic agility
> 		- Simple to set up
> 		- Small codebase -> small attack surface