---
attachments: [Clipboard_2020-10-19-08-36-22.png, Clipboard_2020-10-19-09-06-02.png, Clipboard_2020-10-21-19-15-29.png, Clipboard_2020-10-21-19-21-52.png, Clipboard_2020-10-21-19-27-59.png, Clipboard_2020-10-21-19-30-17.png, Clipboard_2020-10-21-19-59-20.png, Clipboard_2020-10-21-20-33-43.png]
tags: [operating-systems]
title: IO Management and Disk Scheduling
created: '2020-10-19T02:43:17.677Z'
modified: '2020-10-21T15:03:44.037Z'
---

# IO Management and Disk Scheduling
## IO Devices
### Human Readable
Suitable for communicating with the computer user. Examples include printers and terminals, the latter consisting of video display, keyboard, mouse.
### Machine Readable
Suitable for communicationg with electronic equipment. Examples are disk drivers, USB keys, sensors, controllers and actuators.
### Communication
Suitable for communication with remote devices. Examples are digital line drivers and modems.

## Differences in I/O Devices
### Data Rate
There may be differences of magnitude between the data transfer values.
### Application
The use to which a device is put has an influence on the software
### Complexity of Control
The effect on the operating system is filtered by the complexity of the I/O module that controls the device.
### Unit of Transfer
Data may be transferred as a stream of bytes or characters or in large blocks.
### Data Representation
Different data encoding schemes are used by different devices
### Error Conditions
The nature of errors, the way in which they are reported, their consequences and the available range of responses differs from one device to another.

## Organization of IO function
Three techniques for performing IO:

### Programmed IO
The processor issues an IO command on behalf of a process to an IO module; that process then busy waits for the operation to be completed before proceeding.

### Interrupt-driven IO
The processor issues an IO command on behalf of a process
- If non-blocking processor continues to execute instructions from the process that issued the IO command.
- If blocking - the next instruction the processor executes is from the OS, which will put the curent process in a blocked state and schedule another process.

### Direct Memory Access (DMA)
- A DMA module controls the exchange of data between main memory and an IO module.
- Bulk transfer of data

## IO Techniques
![](@attachment/Clipboard_2020-10-19-08-36-22.png)

## Evolution of the IO function
- Processor directly controls a peripheral device
  - Major overhead for a processor as there are different types of IO devices.
- A controller or IO module is added
- Same configuration as above, but now interrupts are employed
  - Intel 8255A
- The IO module is given direct control of memory via DMA
- The IO module is enhanced to become a separate processor, with a specialized instruction set tailored for IO
- The IO module has a local memory of its own, and is, in fact, a computer in its own right.
  - Enables multiple terminals buffer.

## Design Objectives
### Efficiency
- Major effort in IO design
- Important because IO operations often form a bottleneck
- Most IO devices are extremely slow compared with main memory and the processor
- The area that has received the most attention is disk IO.

### Generality
- Desirable to handle all devices in a uniform manner.
- Applies to the way processes view IO devices and the way the operating system manages IO devices and operations.
- Diversity of devices makes it difficult to achieve true generality.
- Use a hierarchical, modular approach to the design of the IO function.

## Logical Structure of the IO function
![](@attachment/Clipboard_2020-10-19-09-06-02.png)

- Logical IO is a set of APIs
- Communications Architecture might be TCP/IP stack for example.
- Directory Management manages your files and folders.
- Physical Organization helps convert logical references to files to physical addresses in memory.
- inode in unix helps manage files.
  - The inode (index node) is a data structure in a Unix-style file system that describes a file-system object such as a file or a directory.

## Buffering
- To avoid overheads and inefficiencies, it is sometimes convenient to perform input transfers in advance of requests being made, and to perform output transfers some time after the request is made.

- Buffers decouple the process and I/O.

### Block oriented device
- Stores information in blocks that are usually of fixed size
- Transfer one block at a time
- Possible to reference data by its block number
- Disks and USB keys are examples.

### Stream oriented device
- Transfers data in and out as a stream of bytes
- No block structure
- Terminals, printers, communications ports, 

### No Buffer
- Without the buffer, the OS directly accesses the device when it needs.

![](@attachment/Clipboard_2020-10-21-19-15-29.png)

- We want to minimize number of I/O requests.
- Single process deadlock - If a process issues an I/O command, is suspended awaiting the result, and then is swapped out prior to the beginning of the operation, the process is blocked waiting on the I/O event and the I/O operation is blocked waiting for the process to be swapped in.

### Single Buffer
![](@attachment/Clipboard_2020-10-21-19-21-52.png)
- The simplest type of support that the operating system can provide.
- When a user process issues an IO request, the OS assigns a buffer in the system portion in the system portion of main memory to the operation.

- Solves SPD + reduces overheads.
- Part of Device IO layer.

### Double Buffer
![](@attachment/Clipboard_2020-10-21-19-27-59.png)
- Assigning two system buffers to the operation
- A process now transfers data to or from one buffer while the operating system empties or fills the other buffer.
- Also known as buffer swapping.

- Still slow, as CPU is much faster than IO.

### Circular Buffer
![](@attachment/Clipboard_2020-10-21-19-30-17.png)
- When more than two buffers are used, the collection of buffers is itself referred to as a circular buffer.
- Each individual buffer is one unit in the circular buffer.

- DOS commands have 16 character buffer.

## Disk Performance Parameters
The actual details of disk I/O operations depend on the:
- Computer system
- Operating system
- Nature of the I/O channel and disk controller hardware
![](@attachment/Clipboard_2020-10-21-19-59-20.png)

Seek Time - Time taken to reach desired portion in the track.

## Disk Scheduling Algorithms
![](@attachment/Clipboard_2020-10-21-20-33-43.png)


