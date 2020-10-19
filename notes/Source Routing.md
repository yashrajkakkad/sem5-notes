---
attachments: [Clipboard_2020-10-06-10-08-57.png, Clipboard_2020-10-06-10-34-40.png]
tags: [computer-networks]
title: Source Routing
created: '2020-09-30T12:59:37.979Z'
modified: '2020-10-06T05:04:49.752Z'
---

# Source Routing
- Less common approach for switching.
- When Host A sends to Host B, it must specify the entire route.
- Various ways to implement. 

## Types:

- Strict (Full route)
- Loose (Subset of nodes) - Helpful when you want to ensure some kind of security.

**Chicken-Egg Problem** - Host A must know the entire route to Host B. 

- Put entire list of ports in the header as an array
- Rotation happens: [3,0,1] -> [1,3,0] -> [0,1,3] for deciding interfaces, leaving no room for ambiguity.
- There's another implemnetation where you don't rotate but strip if off.

Possible implementations:
Photo here

Problem:

- Header size not known. (main problem)
  - It might also vary if strip off happens
- Errors possible.

Rotation takes linear time. You can have a clever algorithm though which pretty much takes constant time. 

Benefits:

- Entire route stored.

Source routing can be used in both datagrams and VC networks.
 
# Bridges

- A class of switches used to forward packets between LANs such as Ethernet.
- Suppose you want to interconnect a pair of Ethernets and you put a repeater in between them.
  - It might exceed the physical limitation of Ethernet.
    - No more than 4 repeaters, no more than 2500 m
- An alternative is to put a node between the two Ethernets which forwards frames from one Ethernet to the other.
  - **Extended LAN**: A collection of LANS connected by one or more bridges.
- Types:
  - "No-frills" bridges
  - Learning (or transparent) bridges
  - Bridges that implement the spanning tree protocol (IEEE 802.1D)
    - Radia Perlman, *Interconnections*.
- Layer 2

## "No-frills" bridges: the simplest strategy

- Forward frames out to all other outputs
- Used by early bridges

## Learning (or transparent) bridges

How does a bridge learn on which port the various hosts reside?

- (Strawman) Solution 1
  - Download a table into the bridge

- Each bridge inspects the source address in all the frames it receives.
  - Records the information at the bridge and builds the table over time.
  - A timeout is associated with each entry.
    - In case a host moves out to another LAN.
  - If the bridge receives a frame for a host not currently in the table, it forwards the frame out on all other ports.

- Transparent means that the bridges won't be touching the content of the packet and a receiver has no way to know how many bridges this crossed.

## Loops in extended LAN
- Flooding strategy works fine if the extended LAN does not have a loop.
  - Frames potentially loop through the extended LAN forever.
    - Stay forever and consume bandwidth at a very high speed.

- Broadcast and multicast have this issue too.

### How does an extended LAN have a loop?
  - Multiple administrators, nobody knows the entire network.
  - Deliberately added as a redundancy in case of failures

- Solution - Distributed Spanning Tree Algorithm

## Spanning Trees
- A sub graph that covers all the vertices but contains no cycles.

- A protocol used by bridges to decide upon a spanning tree.
  - Rest of the links are decided to not be used.

### Spanning Tree Algorithm
- The algorithm is dynamic.
  - The bridges reconfigure themselves into a new spanning tree if some bridges fail.

- Messages sent to form the spanning tree are called Bridge protocol Data unit (BPDU) - sent every few (!-10) seconds.

- Denote BDPU
  - B: ID of believed root bridge
  - D: Distance to B from E
  - E: ID of sending bridge

- Each bridge has a unique ID. In practice a priority field followed by MAC address is used as bridge ID.
  - We'll use B1, B2, B3.... and so on.

- Elects the bridge with the smallest id as the root of the spanning tree. This gives us flexibility to arrange the priorities.

- A variation of Spanning Tree algorithm uses delay (inverse of bandwidth) instead of distance. The reason is to account for speed of the links.

### Root Port
- The root bridge always forwards frames out over all of its ports.
- Each bridge computes the shortest path to the root and notes which of its ports is on this path, called root port.
- Suppose bridfe B16 gets the following messages on its ports 1 to 4, respectively.
  - 15.3.22, 10.6.14, 10.4.12, 20.0.20
  - Port 3 is the root port at this iteration.

### A subtlety
![](@attachment/Clipboard_2020-10-06-10-08-57.png)
B3 - green
B4 - red
B3 can still send configuration messages to B4 but B4 won't send anything to B3

### Designated bridge
- Each bridge is connected to more than one LAN.
- All the bridges connected to a given LAN elect a single designated bridge that will be forwarding frames toward the root bridge.
  - LAN's designated bridge is teh closest bridge to the root.
  - If two or more bridges are equally close to the root then select the bridge with the smallest ID.
- When a bridge receives a configuration message from a bridge close to the root or equally far but from a smaller ID bridge -> not the designated bridge.

### Supporting Broadcast and Multicast
- Forward all broadcast/multicast frames.
  - Current practice
- Learn when no group members downstream.
  - Accomplished by having each member of group G send a frame to bridge multicast address with G in source field.

### Limitations of Bridges
- Do not scale. 
  - Spanning tree algorithm does not scale.
  - Broadcast does not scale
- Do not accomodate heterogeneity

# Virtual LAN
- LAN segments are assigned VLAN IDs or colors.
  - Packets can only be forwarded within the same VLAN ID segments.
  - Type 0x8100
![](@attachment/Clipboard_2020-10-06-10-34-40.png)

The B1 to B2 link is considered to be in both VLANs.


