---
attachments: [Clipboard_2020-10-20-10-35-12.png, Clipboard_2020-10-22-10-20-46.png]
title: Routing
created: '2020-10-20T04:08:25.883Z'
modified: '2020-10-22T05:28:22.209Z'
---

# Routing

## Routing
- Network as a Graph
- The basic problem - find the lowest-cost path between any two nodes.
  - The cost of a path equals the sum of the costs of all the edges that make up the path.

### Static vs Dynamic Routing
- Distributed and dynamic protocols

Two main classes of protocols:
- Distance vector routing
- Link state routing

### Distance Vector
- Each node constructs a 1D array containing the "distances" to all other nodes and distributes that vector to its immediate neighbours.
- Starting assumption is that all nodes know the link cost to each of their directly connected neighbours.

- This algorithm is generalizable and even works with negative costs.

- Bellman-Ford
- Each router sends its table to its neighbour periodically. Each router then updates its table based on the new information.
- Problems include slow response to failures and too many update messages.

#### Count-to-infinity
- Prevents the network from stabilizing.

How to deal?
- Use a relatively small number as an approximation of infinity
  - Set the diameter as the maximum no. of hops

- Split horizon
  - When a node sends routing update to its neighbours, it does not send the routes it learned from each neighbour back to that neighbour.
  - For example, if B has the route (E,2,A) in its table, then B does  not include the route (E,2) in its update table to A.

- In our example, the above two solutions do not fix the problem.

- Split horizon with poison reverse.
  - B actually sends that back route to A, but it puts negative information in the route to ensure that A will not eventually use B to get to E.
  - For example, B sends the route (E,inf) to A.

- This is also a limited fix.

### Routing Information Protocol (RIP)
- They don't calculate route to nodes, but calculate route to networks.

### Flooding
- Forward message to all neighbouring nodes except the node that sent the message.
- If connected, always finds the destination.
- Robust but expensive.
  - There are going to be unnecessary packets.

### Link State Routing
- Strategy: Send to all nodes - not just neighbours - the information about directly connected links only.

- Chicken-Egg problem - We don't know the route but we're supposed to send to every route.

- Used in OSPF and IS-IS IGPs

- Link State Packet (LSP)
  - ID of the node that created the LSP
  - Link cost to each directly connected neighbor
  - Sequence number (SEQNO)
  - Time-to-live (TTL) for this packet.

- LSP are sent using reliable flooding
  - store most recent LSP from each node
  - forward LSP to all nodes but one that sent it

- Generate new LSP periodically
  - Increment SEQNO
  - Start SEQNO at 0 when reboot
  
- Decrement TTL of LSP before flooding

- Age the TTL of LSPs locally
  - When TTL = 0 of some LSP, flood the LSP and discard it.

### Dijkstra's Shortest Path Algorithm
- Find shortest path from some node s to all other nodes
![](@attachment/Clipboard_2020-10-20-10-35-12.png)

### Shortest Path Routing
- In practice, each switch computes its routing table directly from the LSP's it has collected using a realization of Dijkstra's algorithm called the forward search algorithm.

- Specifically each list maintains two lists, known as Tentative and Confirmed.
  - Entries of the form (Destination, Cost, NextHop)

### Open Shortest Path First Protocol (OSPF)
- Nothing conceptual, understand the tables.

### Routing Metric
- Hops, queue length [V1]
- $ f(Depart Time - Arrival Time + Transmission Delay + Propagation Delay) $
  - Reset depart time upon retransmissions.
  - Oscillations

### Router Implementation
- General purpose computer with multiple interface cards
- DMA for storing and forwarding
- Limited by the lower of half memory and I/O bandwidth.
- 133 Mhz 64 bit I/O bus can support peak rate of 4 Gbps.
- Forwarding cost needs to be considered as well.
  - Throughput = PPS * Bits/packet

#### Switching Fabric
- We can abstract a router as a set of input ports, output ports, control unit and switching fabric.

![](@attachment/Clipboard_2020-10-22-10-20-46.png)

#### Head of line blocking
- Several packets arrive at multiple input ports destined to one output port.
- Performance bottleneck
  - Limits the throughput of input buffered switch to 59% of the maximum limit.
- Solution: Buffer at output ports or at both input and output ports.

#### Crossbar switch
- N^2 switching elements
- Each output port needs memory bandwidth equal to the total switch throughput

#### Self routing fabrics
- Add a self routing header which is used by the switching fabric to forward the packet.
- Provides scalability.

#### Banyan Network
- 2x2 crossbars
- log2N levels
- If the input is sorted, 


