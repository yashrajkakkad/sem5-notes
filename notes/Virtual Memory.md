---
attachments: [Clipboard_2020-10-05-09-18-31.png, Clipboard_2020-10-05-09-18-32.png, Clipboard_2020-10-05-09-18-33.png, Clipboard_2020-10-05-09-18-35.png, Clipboard_2020-10-10-08-12-58.png, Clipboard_2020-10-10-08-12-59.png, Clipboard_2020-10-10-08-13-01.png, Clipboard_2020-10-10-08-13-03.png, Clipboard_2020-10-10-08-21-02.png, Clipboard_2020-10-10-08-38-35.png, Clipboard_2020-10-12-08-48-57.png]
tags: [operating-systems]
title: Virtual Memory
created: '2020-10-05T02:38:29.681Z'
modified: '2020-10-26T16:29:17.962Z'
---

# Virtual Memory
A storage allocation scheme in which secondary memory can be addressed as though it were some part of main memory. 

## Virtual Address
Address assigned to a location in virtual memory to allow that location to be accessed as though it were a part of main memory.

Virtual address is a logical address. 
- Segmentation example. Can you have an address larger than physical address? Therefore it is not relative

## Virtual Address Space
The virtual storage assigned to a process.
Virtual address space should be larger than physical space, otherwise there's no meaning to it.

## Hardware and Control Structures
- Two characteristics fundamental to memory management:
  - All memory references are logical addresses that are dynamically translated into physical addresses at run time.
  - A process may be broken up into a number of pieces that don't need to be contiguously located in main memory during execution.

- If these two 

## Execution of a Process
- Resident set
  - Portion of process that is in main memory

- It is possible that a referenced location by a branched instruction is not loaded in main memory.
  - Known as page fault.
  - Time consuming. $\therefore$ process must be blocked.

- Piece of process that contains the logical address is brought into main memory.
  - Operating system issues a disk I/O read request.
  - Another process is dispatched to run while disk I/O 

## Implications
- More processes may be maintained in main memory.
- A process may be larger than all of main memory.

- Overlaying is a responsibility of the programmer, but virtual memory is the responsibility of the Operating System.

## Virtual Paging and Segmentation
- Intel processors support both.
- Outer segmentation and then inner paging.
- Segment Number + Page Number + Offset.

## Thrashing
- A state in which the system spends most of its time swapping process pieces rather than executing instructions.
 - To avoid this, the OS tries to guess, based on recent history, which pieces are least likely to be used in the near future.
- Processes playing ચલક ચલાનુ.
- Software part of virtual memory. Rest all was the responsibility of the hardware.

## Principle of Locality
- Program and data references within a process tend to cluster.
- Only a few pieces of a process will be needed over a short period of time.
- Therefore it is possible 

## Support Needed For Virtual Memory
- Hardware must support paging and segmentation.
- OS must include software for managing the movement of pages and/or segments between secondary memory and main memory.

## Paging
- The term virtual memory is usually associated with systems that employ paging.
- Use of paging to achieve virtual memory was first reported for the Atlas computer.
- Each process has its own page table
  - Each page table entry (PTE) contains the frame nuumber
- Flags
  - P flag - Present flag
  - M flag - Modified flag - Write into secondary storage
  - Privilege bits - 2 

## Two Level Hierarchical Page Table
![](@attachment/Clipboard_2020-10-05-09-18-35.png)

- We are considering 4K page size.
![](@attachment/Clipboard_2020-10-10-08-13-03.png)

- Overheads
- Page faults
- Slower

## Inverted Page Table
- Page number portion of a virtual address is mapped into a hash value
  - Hash value points to inverted page table.

![](@attachment/Clipboard_2020-10-10-08-21-02.png)

## Translation Lookaside Buffer (TLB)
- Each virtual memory reference can cause two physical memory accesses:
  - One to fetch the page table entry.
  - One to fetch the data.

- To overcome the effect of doubling the memory access time, most virtual memory schemes make use of a special high-speed cache called a translation lookaside buffer (TLB).
  - This cache functions in the same way as a memory cache and contains those page table entities that have been most recently used.

![](@attachment/Clipboard_2020-10-10-08-38-35.png)


- Cache entries are very small, typically 32.

- We are exploiting the idea of "Locality of Reference"

Replacement strategies:
- Least Recently Used
- Page Fault

## Page Size
- The smaller the page size, the lesser the amount of internal fragmentation
  - However, more pages are required per process.

## Segmentation
- Segmentation allows the programmer to view memory as consisting of multiple address spaces or segments.

- Advantages
  - Simplifies handling of growing data structures.
  - Allows programs to be altered and recompiled independently.
  - Lends itself to sharing data among processes.
  - Lends itself to protection.

## Combined Paging and Segmentation
- Segmentation is visible to the programmer vs Paging is transparent to the programmer.
- In a combined system, a user's address space is broken up into a number of segments. Each segment is broken up into a number of fixed-size pages which are equal in length to a main memory scheme.

#### Object Oriented vs Procedural Locality of Refernece.
- In object oriented philosophy, locality of reference is reduced. 
- In procedural, it is more

## Operating System Software
The design of the memory management portion of an operating system depends on three fundamental areas of choice: 
- Whether or not to use virtual memory techniques
- The use of paging or segmentation or both
- The algorithms employed for various aspects of memory management.

### I/O Buffering
The process of temporarily storing data that is passing between a processor and a peripheral. The usual purpose is to smooth out the difference in rates at which the two devices can handle data. 

### Fetch Policy

#### Demand paging
- A page is brought into the main memory only when a reference is made to a location.

#### Prepaging
- Pages other than the one demanded by a page fault are brought in.
- Hard disks have block size, and we exploit this characteristic.

### Replacement Policy
- Deals with the selection of a page in main memory to be replaced when a new page is fetched.

#### Optimal
- Replace which is not going to be referred in the near future.
- Not practically implementable scheme.

#### Least recently used (LRU)
- Replace the one which is least recently used
- Implementation overhead
- For every page I need to maintain a timestamp.

#### FIFO
- First In First Out

#### Clock
- Multiple variants exist, we'll be learning two of these.
- 'Use' bit
- When a page is first loaded in memory or referenced, the use bit is set to 1.
- The set of frames is considered to be a circuar buffer.
- Any frame with a use of 
- Implemented (some variant) by an operating system.

### Frame Locking
When a frame is locked the page currently stored in that frame may not be replaced
  - Kernel of the OS as well as key control structures are held in lock frames.
  - I/O buffers and time-critical areas may be locked into main memory frames.
  - Locking is achieved by associating a lock bit with each frame.

### Page Buffering
- Buffers are always reserved in primary memory.
- Required when there's a speed mismatch between transmitter and receiver.
- Improves paging performance and allows the use of a simpler page replacement policy.

![](@attachment/Clipboard_2020-10-12-08-48-57.png)

### Replacement Policy and Cache Size
- Should be considered
- Left for self-study

### Cleaning Policy
- Concerned with determining when a modified page should be written out to secondary memory.

- Between write time and replacement declared, the page might be changed again.

**Demand Cleaning** - A page is written out to secondary memory only when it has been selected for replacement.

**Precleaning** - Allows the writing of pages in batches.

### Load Control
- Determines the number of processes that will be resident in main memory.
  - *Multiprogramming* level.
- Critical in effective memory management.
- Too few processes, many occasions when all processes will be blocked and much time will be spent in swapping..
- Too many processes will lead to thrashing.

### Controlling Load / Performance
- L = S criteria.

Mean time between page faults = Mean time to manage page faults => Optimal performance.

### Process Suspension

### UNIX Virtual Memory
- Self Study

## Resident Set Management
- The OS must decide how many pages to bring into main memory.
  - The smaller the amount of memory allocated to each process, the more processes can reside in memory.
  - Small number of pages loaded increases page faults.
  - Beyond a certain size, further allocations of pages will not affect the page fault rate.

### Resident Set Size
#### Fixed-Allocation
- Gives a process a fixed number of page frames.

#### Variable-Allocation
Allows the number of page frames allocated to a process to be varied over the lifetime of the process.

## Replacement Scope
Local or Global

#### Locked pages - I/O buffer, important data structures

### Fixed Allocation, Local Scope
- Necessary to decide ahead of time the amount of allocation to give a process.
- If allocation is too small, there will be a high page fault rate.

### Variable Allocation, Global Scope
- Easiest to implement, adapted in a number of OS
- OS maintains a list of free frames.
- Free frame is added to resident set of process when a page fault occurs.
- If no frames are available the OS must choose a page currently in memory.
- One way to counter potential problems is to use page buffering.

### Variable Allocation, Local Scope
- Decision to increase or decrease a resident set size is based on the assessment of the likely future demands of active processes.

#### Key elements
- Criteria used to determine resident set size
- The timing of changes


