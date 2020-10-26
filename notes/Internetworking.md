---
attachments: [Clipboard_2020-10-08-09-40-44.png, Clipboard_2020-10-08-09-56-04.png, Clipboard_2020-10-08-10-15-29.png, Clipboard_2020-10-08-10-21-12.png, Clipboard_2020-10-08-10-23-59.png, Clipboard_2020-10-08-10-24-04.png, Clipboard_2020-10-08-10-36-43.png, Clipboard_2020-10-13-10-00-34.png, Clipboard_2020-10-13-10-00-37.png, Clipboard_2020-10-13-10-08-25.png, Clipboard_2020-10-13-10-28-49.png, Clipboard_2020-10-13-10-47-51.png, Clipboard_2020-10-15-09-48-13.png, Clipboard_2020-10-15-09-56-12.png, Clipboard_2020-10-15-10-43-40.png]
tags: [computer-networks]
title: Internetworking
created: '2020-10-06T05:20:16.413Z'
modified: '2020-10-26T16:29:04.419Z'
---

# Internetworking

## Internet Protocol (IP)
- How do we build networks of global scale?
- How do we interconnect different types of networks to build a large global network?

## Internetwork or Internet
- An arbitrary collection of networks interconnected to provide host-host to packet delivery service.

![](@attachment/Clipboard_2020-10-08-09-40-44.png)

## Internet Protocol (IP)
- Key tools used today to build scalable, heterogenous internetworks.
- It runs on all nodes in a colelction of networks and defines the infrastructure that allows these nodes and networks to function as a single logical internetwork.
- Nodes are transparent.
- Global Addressing Scheme
  - Provides a way to identify every host in the network

## IP Service Model
- Transport layer must be aware of what kind of services IP must provide.
- In some ways application layer as transport layer is bypassed.
- IP itself is said to be at Network layer.

### Packet Delivery Model
- Connectionless model for data delivery.
- "Best-effort" or unreliable service
  - packets can get lost, delivered out of order.
    - Reasons for loss? congestion, errors.
    - Reasons for out of order? different paths, different scheduling algortihms.
  - duplicate copies of a packet can be delivered.
    - operating system faults, software bugs.
    - router sent once, and then it sent another time at the link layer (missing acknowledgement for example)
  - packets can be delayed for a long time.

## IP Packet Format
- Version (4) -  4
- Hlen (4) - number of 32-bit words in header. 
  - therefore max header length = 60 bytes
  - Minimum IP header size is 20 bytes
- TOS(8): type of service
- Length (16) - Number of bytes in this datagram (notice two different conventions Hlen vs Length)
- Ident (16) - Used by fragmentation
- Flags (3) & Offset (13) - Used by fragmentation, no. of 8 bytes
- TTL (8) - Number of hops this datagram can travel.
  - Default 64
  - Every router deducts 1
- Protocol (8) - TCP=6, UDP=17
- Checksum (16) - Of header only
- SrcAddr & DstAddr (32)
- Options (variable) - Adjust size of header according this size
- Pad (variable)

![](@attachment/Clipboard_2020-10-08-09-56-04.png)


## IP Fragmentation
- Every network has a Maximum Transmission Unit (MTU)
  - Ethernet (1500 bytes), FDDI (fiber) (4500 bytes)
  - Refers to the "IP" part specifically

- Fragmentation happens in the network, but not assembly
  - Assembly only done at the receiving host.

- All the fragments carry the same identififier in the Ident field.
- Offset is multiple of 8 bytes.
- Fragments are self-contained datagrams.
- IP does not recover from missing fragments.


![](@attachment/Clipboard_2020-10-08-10-15-29.png)

![](@attachment/Clipboard_2020-10-08-10-24-04.png)

### Fragmentation is not good
- Extra headers
- One fragment is lost, everything is lost.


### Avoiding IP Fragmentation
- Path MTU discovery is currently used
  - Set no fragmentation bit
  - ICMP message sent to the source if MTU is less than the datagram size

## Global Addresses
- Globally unique
  - hierarchical: network + host
  - 4 Billion IPv4 address
    - 1/2 class A, 1/4 class B, 1/8 class C

![](@attachment/Clipboard_2020-10-08-10-36-43.png)

- Dot notation

- Old notation, a lot has changed now.

## IP Datagram Forwarding Strategy
- If directly connected to destination network, then forward it to host.
- If not directly connected to destination network, then forward to some router.
  - each router maintains a forwarding table
  - forwarding table maps network number into next hop
  - each host has a default router

## IP datagram forwarding algorithm

## IPs Assigned are not random
![](@attachment/Clipboard_2020-10-13-10-00-37.png)
- If all IPs are random, routing table will be large.
  - We cannot afford to assign IP addresses randomly.
  - Addresses have to take account for geographic reality.
- In our figure, table for R3:
  - 2.3.5.0 - 1
  - 2.3.7.0 - 0
  - One entry for itself.

## Subnetting
- Adds antoher level to address and routing hierarchy
  - Subnet masks define variable partition of host address part
  - It is not necessary that the ones in mask be contiguous.
- Subnets are visible only within site
- Multiple subnets can exist in one physical network.

![](@attachment/Clipboard_2020-10-13-10-08-25.png)

### How does it work?
- Take the subnet IP and do bitwise AND with the address. You get the combination of network number and subnet ID.
- IP & SubnetMask = SubnetNumber --> Forward.

## Classless Addressing
- CIDR - Classless interdomain routing
- Exhaustion of IP address space centers on exhaustion of the class B network numbers
- Solution
  - Say "NO" to any Autonomous System (AS) that requests a class B address unless they can show a need for something close to 64K addresses.
  - Instead give them an appropriate number of class C addresses
  - For any AS with atleast 256 hosts, we can guarantee an address space utilization of at least 50%.

- Instead of handing out addresses at random, hand out a block of contiguous class C addresses
- Suppose we need 16 class C addresses
  - Assign the class C network numbers from 192.4.16 through 192.4.31
      - Top 20 bits are the same.
  - We therefore have created a 20-bit network number.

- The convention is to place a /X after prefix, where X is the prefix length in bits.
- Example: Class A - /8, Class B - /16, Class C - /24


## CIDR
  - CIDR tries to balance the desire to minimize the number of routes that a router needs to know against the need to hand out addresses efficiently.


![](@attachment/Clipboard_2020-10-13-10-28-49.png)

- How do the routing protocols handle this classless addresses?
  - It must understand that the netowrk number may be of any length
- Represent network number with a single pair <value, length>

- With CIDR it is possible that there are prefixes in the forwarding tables that overlap
  - E.g., both 171.69 (a 16-bit prefix) and 171.69.10 (a 24 bit prefix) may exist in the forwarding table of a router.
  - A packet destined to 171.69.10.5 matches both prefixes: 171.69.10 is used.
    - The rule is based on the principle of "longest match".
  - A packet destined to 171.69.20.5 matches 171.69

## Address Resolution Protocol (ARP)
- Problem: Map IP addresses into physical address of destination or next hop router.
  - Why?

- ARP constructs a table to IP address to physical address bindings.
  - Broadcast request if IP address not in table.
  - Target responds with its physical address
  - Table entries are discarded if not refreshed.

### ARP Packet Format
- Hardware Type - Type of Physical Network (eg. Ethernet)
- ProtocolType - type of higher layer protocol (eg IP)
- HLEN & PLEN - length of physical and protocol addresses
- Operation - request/response
- Source & Target Phyisical and Protocol addresses
![](@attachment/Clipboard_2020-10-13-10-47-51.png)

## Host Configurations
- Ethernet addresses are configured into network by manufacturer and they are unique.
- IP addresses must be unique on a given internetwork but also must reflect the structure of the internetwork.
- Most host OS provide a way to manually configure the IP information for the host.

## Dynamic Host Configuration Protocol
- How do we configure IP addresses?
  - IP addresses must be unique on a given internetwork but also must reflect the structure of the internetwork.
- Manual configuration has limitations.
- DHCP server is responsible for providing configuration information to hosts.
- One or more DHCP servers

## DHCP Message Exchanges
"DORA"
- Discover - New host broadcasts DHCPDISCOVER message
  - Why broadcast?
- Offer - DHCP offers it an IP address and informs default router address
- Request - The host confirms the offer by "requesting" the IP address. (There may be multiple DHCP servers)
- Acknowledgement - The server confirms address allocation and notifies lease-time.

## Internet Control Message Protocol
- Defines a collection of error messages that are sent back to the soruce host.
  - Destination host unreachable due to link/node failure
  - Reassembly process failed
  - TTL had reached 0
  - IP header checksum failed
- ICMP-Redirect
  - From router to source host with a better route information.

## IP address allocation
- IPs got exhausted long time ago.

## IPv6
- 128-bit addresses
- Auto-configuration
- Enhanced routing functionality, including support for mobile hosts
- Multicast
- Real-time service
- Authentication and security
- End-to-end fragmentation
- Source routing - embedded in IPv6 header

### Why IPv6 switch has not happened as yet?
- Internet is not owned by a particular entity.
- Many of these features have made its way into IPv6 in recent years, often using similar techniques in both protocols.
- NAT (Network Address Translation)
- Market Pressure

### IPv6 Addresses
- Classless addressing/routing like CIDR

- Notation
  - x : x : x : x : x : x : x (x = 16 bit)
  - contiguous 0s are compressed - 47CD::A456:0124
  - IPv6 compability IPv4 address - ::128.42.1.87

- Address assignment
  - provider-based, geographic

![](@attachment/Clipboard_2020-10-15-09-48-13.png)

### IPv6 Header
- 40 bytes "base" header
  - Version - same length as IPv4 deliberately
  - TrafficClass
  - FlowLabel
  - PayloadLen - in bytes
  - NextHeader
    - A demux key
  - HopLimit. Like TTL
- Extension headers: fixed order, mostly fixed length:
  - fragmentation
  - source routing
  - authentication
  - security
  - other options

![](@attachment/Clipboard_2020-10-15-09-56-12.png)

### IPv6 Autoconfiguration
- Autoconfiguration is built in, and there's an option of using DHCP as well.
- IPv6 unicast addresses are hierarchical
  - The least significant part is the interface ID

- Obtain a link local interface ID
  - Assign FE80 :: 48-bit MAC or a 48-bit random number
  - Use ICMP to detect uniqueness

- Obtain the correct address prefix for this subnet if globally valid address is needed
  - Can send route solicitation to router which responds with network prefix.


## Network Address Translation
- NAT mitigates IPv4 address space exhaustion by mapping one public IP address to several private IP addresses.
  - Implemented using middleware boxes
- Private IP addresses
  - 10.0/8
  - 172.16.0/12
  - 192.168.0/16

- Every connection has a unique "port" to distinguish different connections inside a LAN to the same server.

![](@attachment/Clipboard_2020-10-15-10-43-40.png)

## IP Tunnel
- Virtual Private Network (VPN)
  - Authentication
  - IP Tunnel
  - Encryption
- IP v6 transition
- Mobile IP
- Multicast


