---
title: "Reserved IP addresses - Wikipedia"
source: "https://en.wikipedia.org/wiki/Reserved_IP_addresses"
author:
  - "[[Contributors to Wikimedia projects]]"
published: 2006-03-07
created: 2026-01-13
description:
tags:
  - "clippings"
---
In the Internet addressing architecture, the Internet Engineering Task Force (IETF) and the Internet Assigned Numbers Authority (IANA) have reserved various Internet Protocol (IP) addresses for special purposes.

## IPv4

IPv4 designates special usage or applications for various addresses or address blocks:

| Address block (CIDR) | Address range               | Number of addresses | Scope           | Description                                                                                                                                                               |
| -------------------- | --------------------------- | ------------------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0.0.0.0/8            | 0.0.0.0–0.255.255.255       | 16 777 216          | Software        | Current (local, "this") network                                                                                                                                           |
| 10.0.0.0/8           | 10.0.0.0–10.255.255.255     | 16 777 216          | Private network | Used for local communications within a private network                                                                                                                    |
| 100.64.0.0/10        | 100.64.0.0–100.127.255.255  | 4 194 304           | Private network | Shared address space  for communications between a service provider and its subscribers when using a carrier-grade NAT                                                    |
| 127.0.0.0/8          | 127.0.0.0–127.255.255.255   | 16 777 216          | Host            | Used for loopback addresses to the localhost                                                                                                                              |
| 169.254.0.0/16       | 169.254.0.0–169.254.255.255 | 65 536              | Subnet          | Used for link-local addresses between two hosts on a single link when no IP address is otherwise specified, such as would have normally been retrieved from a DHCP server |
| 172.16.0.0/12        | 172.16.0.0–172.31.255.255   | 1 048 576           | Private network | Used for local communications within a private network                                                                                                                    |
| 192.0.0.0/24         | 192.0.0.0–192.0.0.255       | 256                 | Private network | IETF Protocol Assignments, DS-Lite (/29)                                                                                                                                  |
| 192.0.2.0/24         | 192.0.2.0–192.0.2.255       | 256                 | Documentation   | Assigned as TEST-NET-1, documentation and examples                                                                                                                        |
| 192.88.99.0/24       | 192.88.99.0–192.88.99.255   | 256                 | Internet        | Reserved. Formerly used for IPv6 to IPv4 relay  (included IPv6 address block 2002::/16).                                                                                  |
| 192.168.0.0/16       | 192.168.0.0–192.168.255.255 | 65 536              | Private network | Used for local communications within a private network                                                                                                                    |
| 198.18.0.0/15        | 198.18.0.0–198.19.255.255   | 131 072             | Private network | Used for benchmark testing of inter-network communications between two separate subnets                                                                                   |
| 198.51.100.0/24      | 198.51.100.0–198.51.100.255 | 256                 | Documentation   | Assigned as TEST-NET-2, documentation and examples                                                                                                                        |
| 203.0.113.0/24       | 203.0.113.0–203.0.113.255   | 256                 | Documentation   | Assigned as TEST-NET-3, documentation and examples                                                                                                                        |
| 224.0.0.0/4          | 224.0.0.0–239.255.255.255   | 268 435 456         | Internet        | In use for multicast (former Class D network)                                                                                                                             |
| 233.252.0.0/24       | 233.252.0.0–233.252.0.255   | 256                 | Documentation   | Assigned as MCAST-TEST-NET, documentation and examples (This is part of the above multicast space.)                                                                       |
| 240.0.0.0/4          | 240.0.0.0–255.255.255.254   | 268 435 455         | Internet        | Reserved for future use  (former Class E network)                                                                                                                         |
| 255.255.255.255/32   | 255.255.255.255             | 1                   | Subnet          | Reserved for the "limited broadcast" destination address                                                                                                                  |

## IPv6

IPv6 assigns special uses or applications for various IP addresses:

| Address block (CIDR)     | First address                | Last address                               | Usage               | Purpose                                                                                   |
| ------------------------ | ---------------------------- | ------------------------------------------ | ------------------- | ----------------------------------------------------------------------------------------- |
| ::/128                   | ::                           | ::                                         | Software            | Unspecified address                                                                       |
| ::1/128                  | ::1                          | ::1                                        | Host                | Loopback address—a virtual interface that loops all traffic back to itself, the localhost |
| ::ffff:0:0/96            | ::ffff:0.0.0.0::ffff:0:0     | ::ffff:255.255.255.255::ffff:ffff:ffff     | Software            | IPv4-mapped addresses                                                                     |
| 64:ff9b::/96             | 64:ff9b::0.0.0.064:ff9b::0:0 | 64:ff9b::255.255.255.25564:ff9b::ffff:ffff | The global Internet | NAT64 IPv4/IPv6 translation                                                               |
| 64:ff9b:1::/48           | 64:ff9b:1::                  | 64:ff9b:1:ffff:ffff:ffff:ffff:ffff         | Private internets   | local-use IPv4/IPv6 translation                                                           |
| 100::/64                 | 100::                        | 100::ffff:ffff:ffff:ffff                   | Routing             | Discard prefix                                                                            |
| 2001::/32                | 2001::                       | 2001:0:ffff:ffff:ffff:ffff:ffff:ffff       | The global Internet | Teredo tunneling                                                                          |
| 2001:20::/28             | 2001:20::                    | 2001:2f:ffff:ffff:ffff:ffff:ffff:ffff      | Software            | ORCHIDv2                                                                                  |
| 2001:db8::/32            | 2001:db8::                   | 2001:db8:ffff:ffff:ffff:ffff:ffff:ffff     | Documentation       | Addresses used in documentation and example source code                                   |
| 2002::/16                | 2002::                       | 2002:ffff:ffff:ffff:ffff:ffff:ffff:ffff    | The global Internet | The 6to4 addressing scheme                                                                |
| 3fff::/20                | 3fff::                       | 3fff:fff:ffff:ffff:ffff:ffff:ffff:ffff     | Documentation       | Addresses used in documentation and example source code                                   |
| 5f00::/16                | 5f00::                       | 5f00:ffff:ffff:ffff:ffff:ffff:ffff:ffff    | Routing             | IPv6 Segment Routing (SRv6)                                                               |
| fc00::/7                 | fc00::                       | fdff:ffff:ffff:ffff:ffff:ffff:ffff:ffff    | Private internets   | Unique local address                                                                      |
| fe80::/64 from fe80::/10 | fe80::                       | fe80::ffff:ffff:ffff:ffff                  | Link                | Link-local address                                                                        |
| ff00::/8                 | ff00::                       | ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff    | The global Internet | Multicast address                                                                         |

## External links

- [IANA IPv4 Special-Purpose Address Registry](https://www.iana.org/assignments/iana-ipv4-special-registry/iana-ipv4-special-registry.xhtml)
- [IANA IPv6 Special-Purpose Address Registry](https://www.iana.org/assignments/iana-ipv6-special-registry/iana-ipv6-special-registry.xhtml)
