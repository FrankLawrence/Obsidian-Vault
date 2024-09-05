---
Tags: 
Created: 2024-08-12 19:38:29
---
(Links:: [[CISCO Networking Basics]])
# IPv4 Issues
The depletion of IPv4 address space has been the motivating factor for moving to IPv6. IPv6 has a larger 128-bit address space, providing 340 undecillion possible addresses. When the IETF began its development of a successor to IPv4, it used this opportunity to fix the limitations of IPv4 and include enhancements. One example is ICMPv6, which includes address resolution and address autoconfiguration not found in ICMPv4.

Both IPv4 and IPv6 coexist and the transition to only IPv6 will take several years. The IETF has created various protocols and tools to help network administrators migrate their networks to IPv6. The migration techniques can be divided into three categories: Dual Stack, Tunneling, and Translation. Dual stack devices run both IPv4 and IPv6 protocol stacks simultaneously. **Tunneling** is a method of transporting an IPv6 packet over an IPv4 network. The IPv6 packet is encapsulated inside an IPv4 packet, similar to other types of data. **NAT64** allows IPv6-enabled devices to communicate with IPv4-enabled devices using a translation technique similar to NAT for IPv4. An IPv6 packet is translated to an IPv4 packet and an IPv4 packet is translated to an IPv6 packet.
# IPv6 Addressing
IPv6 addresses are 128 bits in length and written as a string of hexadecimal values. Every four bits is represented by a single hexadecimal digit; for a total of 32 hexadecimal values. IPv6 addresses are not case-sensitive and can be written in either lowercase or uppercase. In IPv6, a hextet that refers to a segment of 16 bits, or four hexadecimal values. Each “x” is a single hextet, which is 16 bits or four hexadecimal digits. Preferred format means that you write IPv6 address using all 32 hexadecimal digits. Here is one example - fe80:0000:0000:0000:0123:4567:89ab:cdef.

There are two rules that help to reduce the number of digits needed to represent an IPv6 address.

**Rule 1 –** Omit Leading Zeros. You can only omit leading zeros, not trailing zeros.

- 01ab can be represented as 1ab
- 09f0 can be represented as 9f0
- 0a00 can be represented as a00
- 00ab can be represented as ab

**Rule 2** – Double Colon. A double colon (::) can replace any single, contiguous string of one or more 16-bit hextets consisting of all zeros. For example, 2001:db8:cafe:1:0:0:0:1 (leading 0s omitted) could be represented as 2001:db8:cafe:1::1. The double colon (::) is used in place of the three all-0 hextets (0:0:0). The double colon (::) can only be used once within an address, otherwise there would be more than one possible resulting address. If an address has more than one contiguous string of all-0 hextets, best practice is to use the double colon (::) on the longest string. If the strings are equal, the first string should use the double colon (::).

---
References: