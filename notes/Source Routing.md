---
tags: [computer-networks]
title: Source Routing
created: '2020-09-30T12:59:37.979Z'
modified: '2020-09-30T13:17:22.101Z'
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

## "No-frills" bridges: the simplest strategy

- Forward frames out to all other outputs
- Used by early bridges

## Learning bridges

How does a bridge learn on which port the various hosts reside?

- (Strawman) Solution 1
  - Download a table into the bridge

- Each bridge inspects the source address in all the frames it receives.
  - Records the information at the bridge and builds the table over time.
  - A timeout is associated with each entry.
    - In case a host moves out to another LAN.
  - If the bridge receives a frame for a host not currently in the table, it forwards the frame out on all other ports.

  

