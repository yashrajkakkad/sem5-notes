---
attachments: [Clipboard_2020-09-30-17-25-52.png]
tags: [computer-networks]
title: Switching
created: '2020-09-30T11:50:45.349Z'
modified: '2020-09-30T13:16:59.901Z'
---

# Switching
- Switch forms a star-topology with P2P links. 
  - Every node has its own link to the switch.
    - It may be possible for many hosts to transmit at full link speed.
- Network layer

## Assumptions:
- Globally unique addresses for nodes.
- Some identification for I/O ports of switch.

## Datagram Switching
- The destination address present in every packet is enough to enable any switch to make appropriate forwarding decision.
- To decide how to forward a packet, a switch consults its forwarding table.

### Forwarding Table

## Virtual Circuit Switching
- Uses the concept of Virtual Circuit, also called connection-oriented switching.
  - Its analogous to circuit switching, but this is still packet switching.
- First set up a virtual connection from the source host to the destination host and then send the data.

### Connection Setup

### Virtual Circuit Table
Some good self explanatory image here.

- Incoming and outgoing VCIs need not be the same.

### VC Forwarding

#### Establishing VC

- **Permanent Virtual Circuit:** Network Administrator configures the connection state.
- **Switched Virtual Circuit:** Messages from a host configure the connection state.
  - We use "signalling" to establish the VC.
    - A host may set up and delete VC dynamically without network administrator intervention.
    - Filling of outgoing VCI starts with the end-host.

#### Teardown

- When Host A no longer wants to send message to Host B, it tears down the connection by sending a tear down message to switch 1.
- The switch 1 removes the relevant entry from its table and forwards the message on to the other switches in the path.

#### Characteristics of VC
- Pne RTT of delay before data is sent as host has to wait for connection to go and come back.
- Identifier is local and won't be as large as a global one. Therefore, per packet overhead goes down.
- If a connection fails, a new one has to be established. Also, the old one needs to be deleted to free up space.
- Requirement of a routing algorithm for deciding the link on which the data should be sent.
- By the time the host gets the go-ahead to send data, it knows quite a lot about the network.
  - That there is really a route to the receiver and that the receiver is willing to receive data.
- It is also possible to allocate resources to the virtual circuit at the time it is established. (Quality of Service)
  - We try to guarantee some kind of performance-related guarantee as a connection is properly established. 
- It is possible to do hop-by-hop flow control. 

### Datagram vs VC

- Datagram: 
  - No connection establishment (connectionless), you can transmit right away
  - Each switch processes each packet independently.
  - Competition for buffer space. Drop if no availability.
- VC:
  - Each circuit can be provided a different quality of service.
    - Some kind of performance guarantee: Switches set aside the resources they need to meet this guarantee, for example, a % of outgoing link's bandwidth, latency, jitter


## X.25 Network

A packet-switched network that uses the connection-oriented model.
- Buffer allocated on initializing circuit.
- Sliding Window protocol between each pair of nodes. (hop by hop flow control)
- The circuit is rejected if not enough buffers are available when the connection request message is processed.

## Asynchronous Transfer Mode (ATM)

- A popular example of VC technology is ATM.
  - One of the past applications of VC was Frame Relay used in VPN.
- Could not take off because it is complex. Ethernet won.
- Connection oriented packet-switched network.
- Packets are called cells, primarily because they are fixed in size.
  - 5 byte header + 48 byte payload
- Fixed length packets are easier to switch in hardware.
  - Simpler to design, enables parallelism.

### ATM Cell Format

- GFC: Generic Flow Control (Have to do with flow control)
- VPI: Virtual Path Identifier (Used for aggregation - clubbing different circuits together)
- VCI: Virtual Circuit Identifier (VC is split into these two)
- Type: management, congestion control
- CLP: Cell Loss Priority
- HEC: Header Error Check (CRC-8)
- User-Network Interface (UNI) and Network-Network Interface (NNI)




