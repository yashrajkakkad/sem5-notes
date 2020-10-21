---
tags: [computer-networks]
title: Ethernet
created: '2020-10-01T04:02:51.978Z'
modified: '2020-10-01T04:36:45.914Z'
---

# Ethernet

- The most successful local area networking technology of the last 40 years.

- DC and Intel joined Xerox to define a 10 Mbps Ethernet standard in 1978 called DIX Ethernet which formed the basis for the present IEEE 802.3. 
  -  Established a company called 3COM which manufactured millions of adapters and became very rich.

- IEEE 802.3
  - 100-Mbps - Fast Ethernet
  - 1000-Mbps - Gigabit Ethernet

## Classic (Broadcast) Ethernet
- Ethernet's MAC uses CSMA/CD
  - Implemented in hardware on the network adaptor.
  - A set of nodes send and receive frames over a shared link.
  - Carrier sense: nodes can distinguish between an idle & busy link.
  - Collission detection: a node listens as it transmits and can therefore detect when a frame it is transmitting has collided with a frame transmitted by another node.

- Book has Classic Ethernet, whose addressing and frames is only relevant now.

- Uses ALOHA protocol.

- An Ethernet segment was initially implemented on a coaxial cable of up to 500m length.
  - This cable is similar to the type used for cable TV except that it typically has an impedance of 50 ohms instead of cable TV's 75 ohms.

- Host connected to an Ethernet segment by tapping into to.

## Repeaters
- Multiple Ethernet segments joined together.
- A repeater is a device that forwards digital signals.
0 No more than 4 repeaters may be positioned between any pair of nodes.

## Ethernet transmitter algorithm
- It is possible for two or more adapters to begin transmitting at the same time.
  - Either because both found the line to be idle
  - Or both had been waiting for a busy line to become idle.

- When an adaptor detects that its frame is colliding with another, it first transmits a 32-bit jamming sequence and stops transmisison.
  - Thus, a transmitter will minimally send 96 bits in the case of collision.
    - 64-bit preamble + 32-bit jamming sequence.
    - Called a runt frame. This is not enough


- Ethernet is 1 persistent protocol.

### Exponential backoff
- Wait for a certain amount of time before trying again.
- Each time the adaptor tries to transmit but fails, it exponentially increases the maximum amount of wait time.
  - $ [0, 2^n-1] * 51.2 \mus $

### Switched Ethernet
- Modern Ethernet is switched., i.e., has point-to-point links  - no shared medium access.
  - Two or more pair of nodes connected to a switch can communicate simultaneously.
    - Since collisions don't occur, CSMA/CD is not used.
  - The limitations on length also don't exist anymore.

## Ethernet frame format.
- Same for classic and switched ethernet.
- Preamble (64 bit)
- Host and Destination Address (48 bit each) - MAC address
- Packet type (16 bit)
  - If number < 1536: Length
  - > 1536: Well defined demux key
- Data (up to 1500 bytes)
  - Minimally a frame must contain atleast 46 bytes of data. 
  - If less than that, append zeros.
- CRC (32 bit)
  - CRC not for appended zeros.

## Ethernet addresses
- Every Ethernet host in the world has a unique Ethernet Address (burned in ROM).
  - The addres belongs to the adaptor, not the host.

- Ethernet addresses are:
  - A sequence of six numbers separated by colons.
  - Each number is a pair of hexadecimal digits.
    - eg 8:0:2b:e4:b1:2 
      - There's a tradition of not putting zeros in the front.

- Every manufacturer is allocated a prefix.

- An Ethernet adaptor accepts:
  - Frames addressed to its own (MAC) address
  - Frames addressed to its broadcast address (1....1)
  - Frames addresse to a multicast address if instructed.
  - All frames if running in promiscuous mode.
    - Adapter passes all traffic receives to its CPU, even the frames it is not programmed to receive.

  


