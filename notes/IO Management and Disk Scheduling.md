---
attachments: [Clipboard_2020-10-19-08-36-22.png, Clipboard_2020-10-19-09-06-02.png]
tags: [operating-systems]
title: IO Management and Disk Scheduling
created: '2020-10-19T02:43:17.677Z'
modified: '2020-10-19T03:52:33.921Z'
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

