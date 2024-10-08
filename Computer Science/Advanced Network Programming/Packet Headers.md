---
Tags: 
Created: 2024-09-10 01:00:12
---
(Links:: [[Advanced Network Programming]])
# TCP Header Format


```
    0                   1                   2                   3
    0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |          Source Port          |       Destination Port        |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                        Sequence Number                        |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                    Acknowledgment Number                      |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |  Data |           |U|A|P|R|S|F|                               |
   | Offset| Reserved  |R|C|S|S|Y|I|            Window             |
   |       |           |G|K|H|T|N|N|                               |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |           Checksum            |         Urgent Pointer        |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                    Options                    |    Padding    |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                             data                              |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

                            TCP Header Format

          Note that one tick mark represents one bit position.
```

## Source Port:  16 bits
The source port number.
## Destination Port:  16 bits
The destination port number.

## Sequence Number:  32 bits

The sequence number of the first data octet in this segment (except
when SYN is present). If SYN is present the sequence number is the
initial sequence number (ISN) and the first data octet is ISN+1.

## Acknowledgment Number:  32 bits

If the ACK control bit is set this field contains the value of the
next sequence number the sender of the segment is expecting to
receive.  Once a connection is established this is always sent.

Data Offset:  4 bits

The number of 32 bit words in the TCP Header.  This indicates where
the data begins.  The TCP header (even one including options) is an
integral number of 32 bits long.

Reserved:  6 bits

Reserved for future use.  Must be zero.

Control Bits:  6 bits (from left to right):

URG:  Urgent Pointer field significant
ACK:  Acknowledgment field significant
PSH:  Push Function
RST:  Reset the connection
SYN:  Synchronize sequence numbers
FIN:  No more data from sender

Window:  16 bits

The number of data octets beginning with the one indicated in the
acknowledgment field which the sender of this segment is willing to
accept.

Checksum:  16 bits

The checksum field is the 16 bit one's complement of the one's
complement sum of all 16 bit words in the header and text.  If a
segment contains an odd number of header and text octets to be
checksummed, the last octet is padded on the right with zeros to
form a 16 bit word for checksum purposes.  The pad is not
transmitted as part of the segment.  While computing the checksum,
the checksum field itself is replaced with zeros.

The checksum also covers a 96 bit pseudo header conceptually prefixed to the TCP header.  This pseudo header contains the Source
Address, the Destination Address, the Protocol, and TCP length.
This gives the TCP protection against misrouted segments.  This
information is carried in the Internet Protocol and is transferred
across the TCP/Network interface in the arguments or results of
calls by the TCP on the IP.

```
                     +--------+--------+--------+--------+
                     |           Source Address          |
                     +--------+--------+--------+--------+
                     |         Destination Address       |
                     +--------+--------+--------+--------+
                     |  zero  |  PTCL  |    TCP Length   |
                     +--------+--------+--------+--------+
```

The TCP Length is the TCP header length plus the data length in
octets (this is not an explicitly transmitted quantity, but is
computed), and it does not count the 12 octets of the pseudo
header.

Urgent Pointer:  16 bits

This field communicates the current value of the urgent pointer as a
positive offset from the sequence number in this segment.  The
urgent pointer points to the sequence number of the octet following
the urgent data.  This field is only be interpreted in segments with
the URG control bit set.

Options:  variable

Options may occupy space at the end of the TCP header and are a
multiple of 8 bits in length.  All options are included in the
checksum.  An option may begin on any octet boundary.  There are two
cases for the format of an option:

  Case 1:  A single octet of option-kind.

  Case 2:  An octet of option-kind, an octet of option-length, and
		   the actual option-data octets.

The option-length counts the two octets of option-kind and
option-length as well as the option-data octets.

Note that the list of options may be shorter than the data offset
field might imply.  The content of the header beyond the
End-of-Option option must be header padding (i.e., zero).

A TCP must implement all options.
Currently defined options include (kind indicated in octal):

```
      Kind     Length    Meaning
      ----     ------    -------
       0         -       End of option list.
       1         -       No-Operation.
       2         4       Maximum Segment Size.
```


Specific Option Definitions

End of Option List

```
        +--------+
        |00000000|
        +--------+
         Kind=0
```

This option code indicates the end of the option list.  This
might not coincide with the end of the TCP header according to
the Data Offset field.  This is used at the end of all options,
not the end of each option, and need only be used if the end of
the options would not otherwise coincide with the end of the TCP
header.

No-Operation

```
        +--------+
        |00000001|
        +--------+
         Kind=1
```

This option code may be used between options, for example, to
align the beginning of a subsequent option on a word boundary.
There is no guarantee that senders will use this option, so
receivers must be prepared to process options even if they do
not begin on a word boundary.

Maximum Segment Size

```
        +--------+--------+---------+--------+
        |00000010|00000100|   max seg size   |
        +--------+--------+---------+--------+
         Kind=2   Length=4
		Maximum Segment Size Option Data:  16 bits
```

If this option is present, then it communicates the maximum
receive segment size at the TCP which sends this segment.
This field must only be sent in the initial connection request
(i.e., in segments with the SYN control bit set).  If this
option is not used, any segment size is allowed.

Padding:  variable

The TCP header padding is used to ensure that the TCP header ends
and data begins on a 32 bit boundary.  The padding is composed of
zeros.
# ICMP
Message Formats

```
    0                   1                   2                   3
    0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |     Type      |     Code      |          Checksum             |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                             unused                            |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |      Internet Header + 64 bits of Original Data Datagram      |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

## Type
11

## Code
0 = time to live exceeded in transit;
1 = fragment reassembly time exceeded.

## Checksum

The checksum is the 16-bit ones's complement of the one's
complement sum of the ICMP message starting with the ICMP Type.
For computing the checksum , the checksum field should be zero.
This checksum may be replaced in the future.

## Internet Header + 64 bits of Data Datagram

The internet header plus the first 64 bits of the original
datagram's data.  This data is used by the host to match the
message to the appropriate process.  If a higher level protocol
uses port numbers, they are assumed to be in the first 64 data
bits of the original datagram's data.

## Description

If the gateway processing a datagram finds the time to live field is zero it must discard the datagram.  The gateway may also notify
the source host via the time exceeded message.

If a host reassembling a fragmented datagram cannot complete the
reassembly due to missing fragments within its time limit it
discards the datagram, and it may send a time exceeded message.

If fragment zero is not available then no time exceeded need be
sent at all.

Code 0 may be received from a gateway.  Code 1 may be received
from a host.

---

ICMP messages are sent using the basic IP header.  The first octet of
the data portion of the datagram is a ICMP type field; the value of
this field determines the format of the remaining data.  Any field
labeled "unused" is reserved for later extensions and must be zero
when sent, but receivers should not use these fields (except to
include them in the checksum).  Unless otherwise noted under the
individual format descriptions, the values of the internet header
fields are as follows:

## Version

4

## IHL

Internet header length in 32-bit words.

## Type of Service

0

## Total Length

Length of internet header and data in octets.

## Identification, Flags, Fragment Offset

Used in fragmentation, see [[1](https://datatracker.ietf.org/doc/html/rfc792#ref-1)].

## Time to Live

Time to live in seconds; as this field is decremented at each
machine in which the datagram is processed, the value in this
field should be at least as great as the number of gateways which
this datagram will traverse.

## Protocol

ICMP = 1

## Header Checksum

The 16 bit one's complement of the one's complement sum of all 16
bit words in the header.  For computing the checksum, the checksum
field should be zero.  This checksum may be replaced in the
future.

## Source Address

The address of the gateway or host that composes the ICMP message.
Unless otherwise noted, this can be any of a gateway's addresses.

## Destination Address

The address of the gateway or host to which the message should be
sent.
# IPv4
[3.1](https://datatracker.ietf.org/doc/html/rfc791#section-3.1).  Internet Header Format

  A summary of the contents of the internet header follows:

```
    0                   1                   2                   3
    0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |Version|  IHL  |Type of Service|          Total Length         |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |         Identification        |Flags|      Fragment Offset    |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |  Time to Live |    Protocol   |         Header Checksum       |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                       Source Address                          |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                    Destination Address                        |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
   |                    Options                    |    Padding    |
   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

                    Example Internet Datagram Header

                               Figure 4.

  Note that each tick mark represents one bit position.
```

  Version:  4 bits

    The Version field indicates the format of the internet header.  This
    document describes version 4.

  IHL:  4 bits

    Internet Header Length is the length of the internet header in 32
    bit words, and thus points to the beginning of the data.  Note that
    the minimum value for a correct header is 5.

                                                               [Page 11]

---

                                                          September 1981
Internet Protocol
Specification



  Type of Service:  8 bits

    The Type of Service provides an indication of the abstract
    parameters of the quality of service desired.  These parameters are
    to be used to guide the selection of the actual service parameters
    when transmitting a datagram through a particular network.  Several
    networks offer service precedence, which somehow treats high
    precedence traffic as more important than other traffic (generally
    by accepting only traffic above a certain precedence at time of high
    load).  The major choice is a three way tradeoff between low-delay,
    high-reliability, and high-throughput.

      Bits 0-2:  Precedence.
      Bit    3:  0 = Normal Delay,      1 = Low Delay.
      Bits   4:  0 = Normal Throughput, 1 = High Throughput.
      Bits   5:  0 = Normal Relibility, 1 = High Relibility.
      Bit  6-7:  Reserved for Future Use.

         0     1     2     3     4     5     6     7
      +-----+-----+-----+-----+-----+-----+-----+-----+
      |                 |     |     |     |     |     |
      |   PRECEDENCE    |  D  |  T  |  R  |  0  |  0  |
      |                 |     |     |     |     |     |
      +-----+-----+-----+-----+-----+-----+-----+-----+

        Precedence

          111 - Network Control
          110 - Internetwork Control
          101 - CRITIC/ECP
          100 - Flash Override
          011 - Flash
          010 - Immediate
          001 - Priority
          000 - Routine

    The use of the Delay, Throughput, and Reliability indications may
    increase the cost (in some sense) of the service.  In many networks
    better performance for one of these parameters is coupled with worse
    performance on another.  Except for very unusual cases at most two
    of these three indications should be set.

    The type of service is used to specify the treatment of the datagram
    during its transmission through the internet system.  Example
    mappings of the internet type of service to the actual service
    provided on networks such as AUTODIN II, ARPANET, SATNET, and PRNET
    is given in "Service Mappings" [[8](https://datatracker.ietf.org/doc/html/rfc791#ref-8)].



[Page 12]

---

September 1981
                                                       Internet Protocol
                                                           Specification



    The Network Control precedence designation is intended to be used
    within a network only.  The actual use and control of that
    designation is up to each network. The Internetwork Control
    designation is intended for use by gateway control originators only.
    If the actual use of these precedence designations is of concern to
    a particular network, it is the responsibility of that network to
    control the access to, and use of, those precedence designations.

  Total Length:  16 bits

    Total Length is the length of the datagram, measured in octets,
    including internet header and data.  This field allows the length of
    a datagram to be up to 65,535 octets.  Such long datagrams are
    impractical for most hosts and networks.  All hosts must be prepared
    to accept datagrams of up to 576 octets (whether they arrive whole
    or in fragments).  It is recommended that hosts only send datagrams
    larger than 576 octets if they have assurance that the destination
    is prepared to accept the larger datagrams.

    The number 576 is selected to allow a reasonable sized data block to
    be transmitted in addition to the required header information.  For
    example, this size allows a data block of 512 octets plus 64 header
    octets to fit in a datagram.  The maximal internet header is 60
    octets, and a typical internet header is 20 octets, allowing a
    margin for headers of higher level protocols.

  Identification:  16 bits

    An identifying value assigned by the sender to aid in assembling the
    fragments of a datagram.

  Flags:  3 bits

    Various Control Flags.

      Bit 0: reserved, must be zero
      Bit 1: (DF) 0 = May Fragment,  1 = Don't Fragment.
      Bit 2: (MF) 0 = Last Fragment, 1 = More Fragments.

          0   1   2
        +---+---+---+
        |   | D | M |
        | 0 | F | F |
        +---+---+---+

  Fragment Offset:  13 bits

    This field indicates where in the datagram this fragment belongs.

                                                               [Page 13]

---

                                                          September 1981
Internet Protocol
Specification



    The fragment offset is measured in units of 8 octets (64 bits).  The
    first fragment has offset zero.

  Time to Live:  8 bits

    This field indicates the maximum time the datagram is allowed to
    remain in the internet system.  If this field contains the value
    zero, then the datagram must be destroyed.  This field is modified
    in internet header processing.  The time is measured in units of
    seconds, but since every module that processes a datagram must
    decrease the TTL by at least one even if it process the datagram in
    less than a second, the TTL must be thought of only as an upper
    bound on the time a datagram may exist.  The intention is to cause
    undeliverable datagrams to be discarded, and to bound the maximum
    datagram lifetime.

  Protocol:  8 bits

    This field indicates the next level protocol used in the data
    portion of the internet datagram.  The values for various protocols
    are specified in "Assigned Numbers" [[9](https://datatracker.ietf.org/doc/html/rfc791#ref-9)].

  Header Checksum:  16 bits

    A checksum on the header only.  Since some header fields change
    (e.g., time to live), this is recomputed and verified at each point
    that the internet header is processed.

    The checksum algorithm is:

      The checksum field is the 16 bit one's complement of the one's
      complement sum of all 16 bit words in the header.  For purposes of
      computing the checksum, the value of the checksum field is zero.

    This is a simple to compute checksum and experimental evidence
    indicates it is adequate, but it is provisional and may be replaced
    by a CRC procedure, depending on further experience.

  Source Address:  32 bits

    The source address.  See [section 3.2](https://datatracker.ietf.org/doc/html/rfc791#section-3.2).

  Destination Address:  32 bits

    The destination address.  See [section 3.2](https://datatracker.ietf.org/doc/html/rfc791#section-3.2).





[Page 14]

---

September 1981
                                                       Internet Protocol
                                                           Specification



  Options:  variable

    The options may appear or not in datagrams.  They must be
    implemented by all IP modules (host and gateways).  What is optional
    is their transmission in any particular datagram, not their
    implementation.

    In some environments the security option may be required in all
    datagrams.

    The option field is variable in length.  There may be zero or more
    options.  There are two cases for the format of an option:

      Case 1:  A single octet of option-type.

      Case 2:  An option-type octet, an option-length octet, and the
               actual option-data octets.

    The option-length octet counts the option-type octet and the
    option-length octet as well as the option-data octets.

    The option-type octet is viewed as having 3 fields:

      1 bit   copied flag,
      2 bits  option class,
      5 bits  option number.

    The copied flag indicates that this option is copied into all
    fragments on fragmentation.

      0 = not copied
      1 = copied

    The option classes are:

      0 = control
      1 = reserved for future use
      2 = debugging and measurement
      3 = reserved for future use

                                                               [Page 15]

---

                                                          September 1981
Internet Protocol
Specification



    The following internet options are defined:

      CLASS NUMBER LENGTH DESCRIPTION
      ----- ------ ------ -----------
        0     0      -    End of Option list.  This option occupies only
                          1 octet; it has no length octet.
        0     1      -    No Operation.  This option occupies only 1
                          octet; it has no length octet.
        0     2     11    Security.  Used to carry Security,
                          Compartmentation, User Group (TCC), and
                          Handling Restriction Codes compatible with DOD
                          requirements.
        0     3     var.  Loose Source Routing.  Used to route the
                          internet datagram based on information
                          supplied by the source.
        0     9     var.  Strict Source Routing.  Used to route the
                          internet datagram based on information
                          supplied by the source.
        0     7     var.  Record Route.  Used to trace the route an
                          internet datagram takes.
        0     8      4    Stream ID.  Used to carry the stream
                          identifier.
        2     4     var.  Internet Timestamp.



    Specific Option Definitions

      End of Option List

        +--------+
        |00000000|
        +--------+
          Type=0

        This option indicates the end of the option list.  This might
        not coincide with the end of the internet header according to
        the internet header length.  This is used at the end of all
        options, not the end of each option, and need only be used if
        the end of the options would not otherwise coincide with the end
        of the internet header.

        May be copied, introduced, or deleted on fragmentation, or for
        any other reason.






[Page 16]

---

September 1981
                                                       Internet Protocol
                                                           Specification



      No Operation

        +--------+
        |00000001|
        +--------+
          Type=1

        This option may be used between options, for example, to align
        the beginning of a subsequent option on a 32 bit boundary.

        May be copied, introduced, or deleted on fragmentation, or for
        any other reason.

      Security

        This option provides a way for hosts to send security,
        compartmentation, handling restrictions, and TCC (closed user
        group) parameters.  The format for this option is as follows:

          +--------+--------+---//---+---//---+---//---+---//---+
          |10000010|00001011|SSS  SSS|CCC  CCC|HHH  HHH|  TCC   |
          +--------+--------+---//---+---//---+---//---+---//---+
           Type=130 Length=11

        Security (S field):  16 bits

          Specifies one of 16 levels of security (eight of which are
          reserved for future use).

            00000000 00000000 - Unclassified
            11110001 00110101 - Confidential
            01111000 10011010 - EFTO
            10111100 01001101 - MMMM
            01011110 00100110 - PROG
            10101111 00010011 - Restricted
            11010111 10001000 - Secret
            01101011 11000101 - Top Secret
            00110101 11100010 - (Reserved for future use)
            10011010 11110001 - (Reserved for future use)
            01001101 01111000 - (Reserved for future use)
            00100100 10111101 - (Reserved for future use)
            00010011 01011110 - (Reserved for future use)
            10001001 10101111 - (Reserved for future use)
            11000100 11010110 - (Reserved for future use)
            11100010 01101011 - (Reserved for future use)

                                                               [Page 17]

---

                                                          September 1981
Internet Protocol
Specification



        Compartments (C field):  16 bits

          An all zero value is used when the information transmitted is
          not compartmented.  Other values for the compartments field
          may be obtained from the Defense Intelligence Agency.

        Handling Restrictions (H field):  16 bits

          The values for the control and release markings are
          alphanumeric digraphs and are defined in the Defense
          Intelligence Agency Manual DIAM 65-19, "Standard Security
          Markings".

        Transmission Control Code (TCC field):  24 bits

          Provides a means to segregate traffic and define controlled
          communities of interest among subscribers. The TCC values are
          trigraphs, and are available from HQ DCA Code 530.

        Must be copied on fragmentation.  This option appears at most
        once in a datagram.

      Loose Source and Record Route

        +--------+--------+--------+---------//--------+
        |10000011| length | pointer|     route data    |
        +--------+--------+--------+---------//--------+
         Type=131

        The loose source and record route (LSRR) option provides a means
        for the source of an internet datagram to supply routing
        information to be used by the gateways in forwarding the
        datagram to the destination, and to record the route
        information.

        The option begins with the option type code.  The second octet
        is the option length which includes the option type code and the
        length octet, the pointer octet, and length-3 octets of route
        data.  The third octet is the pointer into the route data
        indicating the octet which begins the next source address to be
        processed.  The pointer is relative to this option, and the
        smallest legal value for the pointer is 4.

        A route data is composed of a series of internet addresses.
        Each internet address is 32 bits or 4 octets.  If the pointer is
        greater than the length, the source route is empty (and the
        recorded route full) and the routing is to be based on the
        destination address field.


[Page 18]

---

September 1981
                                                       Internet Protocol
                                                           Specification



        If the address in destination address field has been reached and
        the pointer is not greater than the length, the next address in
        the source route replaces the address in the destination address
        field, and the recorded route address replaces the source
        address just used, and pointer is increased by four.

        The recorded route address is the internet module's own internet
        address as known in the environment into which this datagram is
        being forwarded.

        This procedure of replacing the source route with the recorded
        route (though it is in the reverse of the order it must be in to
        be used as a source route) means the option (and the IP header
        as a whole) remains a constant length as the datagram progresses
        through the internet.

        This option is a loose source route because the gateway or host
        IP is allowed to use any route of any number of other
        intermediate gateways to reach the next address in the route.

        Must be copied on fragmentation.  Appears at most once in a
        datagram.

      Strict Source and Record Route

        +--------+--------+--------+---------//--------+
        |10001001| length | pointer|     route data    |
        +--------+--------+--------+---------//--------+
         Type=137

        The strict source and record route (SSRR) option provides a
        means for the source of an internet datagram to supply routing
        information to be used by the gateways in forwarding the
        datagram to the destination, and to record the route
        information.

        The option begins with the option type code.  The second octet
        is the option length which includes the option type code and the
        length octet, the pointer octet, and length-3 octets of route
        data.  The third octet is the pointer into the route data
        indicating the octet which begins the next source address to be
        processed.  The pointer is relative to this option, and the
        smallest legal value for the pointer is 4.

        A route data is composed of a series of internet addresses.
        Each internet address is 32 bits or 4 octets.  If the pointer is
        greater than the length, the source route is empty (and the

                                                               [Page 19]

---

                                                          September 1981
Internet Protocol
Specification



        recorded route full) and the routing is to be based on the
        destination address field.

        If the address in destination address field has been reached and
        the pointer is not greater than the length, the next address in
        the source route replaces the address in the destination address
        field, and the recorded route address replaces the source
        address just used, and pointer is increased by four.

        The recorded route address is the internet module's own internet
        address as known in the environment into which this datagram is
        being forwarded.

        This procedure of replacing the source route with the recorded
        route (though it is in the reverse of the order it must be in to
        be used as a source route) means the option (and the IP header
        as a whole) remains a constant length as the datagram progresses
        through the internet.

        This option is a strict source route because the gateway or host
        IP must send the datagram directly to the next address in the
        source route through only the directly connected network
        indicated in the next address to reach the next gateway or host
        specified in the route.

        Must be copied on fragmentation.  Appears at most once in a
        datagram.

      Record Route

        +--------+--------+--------+---------//--------+
        |00000111| length | pointer|     route data    |
        +--------+--------+--------+---------//--------+
          Type=7

        The record route option provides a means to record the route of
        an internet datagram.

        The option begins with the option type code.  The second octet
        is the option length which includes the option type code and the
        length octet, the pointer octet, and length-3 octets of route
        data.  The third octet is the pointer into the route data
        indicating the octet which begins the next area to store a route
        address.  The pointer is relative to this option, and the
        smallest legal value for the pointer is 4.

        A recorded route is composed of a series of internet addresses.
        Each internet address is 32 bits or 4 octets.  If the pointer is


[Page 20]

---

September 1981
                                                       Internet Protocol
                                                           Specification



        greater than the length, the recorded route data area is full.
        The originating host must compose this option with a large
        enough route data area to hold all the address expected.  The
        size of the option does not change due to adding addresses.  The
        intitial contents of the route data area must be zero.

        When an internet module routes a datagram it checks to see if
        the record route option is present.  If it is, it inserts its
        own internet address as known in the environment into which this
        datagram is being forwarded into the recorded route begining at
        the octet indicated by the pointer, and increments the pointer
        by four.

        If the route data area is already full (the pointer exceeds the
        length) the datagram is forwarded without inserting the address
        into the recorded route.  If there is some room but not enough
        room for a full address to be inserted, the original datagram is
        considered to be in error and is discarded.  In either case an
        ICMP parameter problem message may be sent to the source
        host [[3](https://datatracker.ietf.org/doc/html/rfc791#ref-3)].

        Not copied on fragmentation, goes in first fragment only.
        Appears at most once in a datagram.

      Stream Identifier

        +--------+--------+--------+--------+
        |10001000|00000010|    Stream ID    |
        +--------+--------+--------+--------+
         Type=136 Length=4

        This option provides a way for the 16-bit SATNET stream
        identifier to be carried through networks that do not support
        the stream concept.

        Must be copied on fragmentation.  Appears at most once in a
        datagram.

                                                               [Page 21]

---

                                                          September 1981
Internet Protocol
Specification



      Internet Timestamp

        +--------+--------+--------+--------+
        |01000100| length | pointer|oflw|flg|
        +--------+--------+--------+--------+
        |         internet address          |
        +--------+--------+--------+--------+
        |             timestamp             |
        +--------+--------+--------+--------+
        |                 .                 |
                          .
                          .
        Type = 68

        The Option Length is the number of octets in the option counting
        the type, length, pointer, and overflow/flag octets (maximum
        length 40).

        The Pointer is the number of octets from the beginning of this
        option to the end of timestamps plus one (i.e., it points to the
        octet beginning the space for next timestamp).  The smallest
        legal value is 5.  The timestamp area is full when the pointer
        is greater than the length.

        The Overflow (oflw) [4 bits] is the number of IP modules that
        cannot register timestamps due to lack of space.

        The Flag (flg) [4 bits] values are

          0 -- time stamps only, stored in consecutive 32-bit words,

          1 -- each timestamp is preceded with internet address of the
               registering entity,

          3 -- the internet address fields are prespecified.  An IP
               module only registers its timestamp if it matches its own
               address with the next specified internet address.

        The Timestamp is a right-justified, 32-bit timestamp in
        milliseconds since midnight UT.  If the time is not available in
        milliseconds or cannot be provided with respect to midnight UT
        then any time may be inserted as a timestamp provided the high
        order bit of the timestamp field is set to one to indicate the
        use of a non-standard value.

        The originating host must compose this option with a large
        enough timestamp data area to hold all the timestamp information
        expected.  The size of the option does not change due to adding


[Page 22]

---

September 1981
                                                       Internet Protocol
                                                           Specification



        timestamps.  The intitial contents of the timestamp data area
        must be zero or internet address/zero pairs.

        If the timestamp data area is already full (the pointer exceeds
        the length) the datagram is forwarded without inserting the
        timestamp, but the overflow count is incremented by one.

        If there is some room but not enough room for a full timestamp
        to be inserted, or the overflow count itself overflows, the
        original datagram is considered to be in error and is discarded.
        In either case an ICMP parameter problem message may be sent to
        the source host [[3](https://datatracker.ietf.org/doc/html/rfc791#ref-3)].

        The timestamp option is not copied upon fragmentation.  It is
        carried in the first fragment.  Appears at most once in a
        datagram.

  Padding:  variable

    The internet header padding is used to ensure that the internet
    header ends on a 32 bit boundary.  The padding is zero.
# IPv6
![400](https://www.blackmagicboxes.com/wp-content/uploads/2016/12/ip-header-v6_1.png)
![400](https://www.blackmagicboxes.com/wp-content/uploads/2016/12/ip-header-v4_1.png)

---
References: 
- https://datatracker.ietf.org/doc/html/rfc793
- https://datatracker.ietf.org/doc/html/rfc792
- https://datatracker.ietf.org/doc/html/rfc791