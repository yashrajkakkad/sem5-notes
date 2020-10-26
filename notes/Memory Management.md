---
attachments: [Clipboard_2020-10-03-08-24-35.png, Clipboard_2020-10-03-08-38-16.png, Clipboard_2020-10-03-09-29-39.png]
tags: [operating-systems]
title: Memory Management
created: '1970-01-01T00:00:00.000Z'
modified: '2020-10-19T02:55:08.860Z'
---

# Memory Management 


## Interprocess Communication

Problems:

- Interprocess Communication must occur somehow.  For example, when two processes use the same resource.
- Deadlocks need to be handled. There needs to be a mechanism to identify and release deadlocks.

This part will be covered later in theory.

# Memory Management

In our discussion, we are going to consider multi-user systems.

**Objective:** There should be sufficient number of processes in the memory that memory utilisation is good.

**IBM's OS:** System automatically decides the number of processes it can handle.

## Requirements

- Relocation
- Protection
- Sharing
- Logical organization
- Physical organization

**Virtual Memory management:** Latest scheme used by contemporary operating systems.

### Relocation

In 8085 programming, writing the program started from the same location (3000).

In systems like Unix, we do not do it that way. We do not decide the  locations of memory we access.

In 8086 programming, did we put addresses everywhere? Offsets for different values for data segment were decided by the compiler itself.

At runtime, segment and offset will combine to form a physical address.

Processes are swapped in and out of memory. Once the program is swapped back, can you guarantee that it will be returned to the same memory address? No.  May need to relocate to a different area of memory.

#### Addressing requirements for a process

- Relative difference in addresses inside the program instructions is constant.
- Addresses can be constructed using Segment + Offset.

### Protection

**How is monitor program protected?** Put in ROM.

- Processors need to acquire permission to reference memory locations for reading or writing purposes.
- Location of a program in main memory is unpredictable.
- Memory locations can be defined with descriptors. eg., a data segment can be defined as read only. Every segment essentially has a security option based on which permissions are granted. You cannot allow changing code segment anytime. Segmentation helps a lot here. 
- Protection is purely a hardware issue and not software issue. 
- Memory references must be checked at runtime. No process should be allowed to access a region outside the memory area.
- Mechanisms that support relocation also support protection.
- Extra reading: Descriptors on 80286

Unix supports 4 privilege levels for execution. Lower the number, higher the privilege. 

0 - Kernel

3 - User

These permissions are also stored in a descriptor.

### Sharing

- Can I share same code with multiple process? Yes, using different threads which will have a copy each.
- How can five processes access same data block? They can initialize to same data segment.
- Example? OS functions are shared amongst various processes.

**Above three come from hardware.** OS can do memory management only if hardware supports it.

### Logical Organization

- Memory is linear, programs are written in modules.
  - Protection should be at the module level. 
  - Modules can be written and compiled independently.
- Segmentation is the tool that most readily satisfied requirements.

- Cohesion and coupling. Less frequently calling each other = loosely coupled which is highly desirable.
- Benefit of modularization? 
  - Maintenance. 
  - Rapid application development. 

### Physical Organization

- Memory available for a program plus its data may be insufficient.
  - 8086 code segment is limited to 64KB which is the upper limit on my code size.
- Overlaying allows various modules to be assigned the same region of memory but it is time consuming to program.
  - I will divide my program into modules. First module may make some reference to second module. 
  - Then I remove first module, load second, and call the respective function.
  - Second module may call some location from the third module.
  - Programmer has to design program in a fashion that overlaying can be performed. 
- Programmer does not know how much space will be available.

Is automatic from compiler support, system and hardware.

Compiler is going to support logical organization and physical organization.

## Memory partitioning

### Fixed partitioning

- Partition sizes decided when OS is started. 

#### Equal partitioning

- Flaws:
  - Some modules may require less size than the partition size. (Internal fragmentation)
  - Some modules may require more.
    - Overlaying, which comes with its own flaws.
  - Putting an upper limit on the degree of multiprogramming = number of partitions.

#### Unequal partitioning
Implementation:
- One process queue per partition
- Single queue
  - Can allocate to minimize fragmentation.

### Dynamic partitioning

- Dynamic sizing based on size of the process.
- Used by IBM's mainframe OS-MVT.
- Disadvantage:
  - External fragmentation. Small, non contiguous free blocks
    - Compaction: Reduce external fragmentation by shifting processes.
      - Compaction is on primary memory, defragmentation is on secondary memory. Essentially, they are the same thing
- Placement policy:
  - First fit
  - Next fit
  - Best fit

Both fixed and dynamic partitioning support all the requirements discussed previously.

### Buddy System
- Comprised of fixed and dynamic partitioning schemes.
- Space available for allocation treated as a single block.
- Memory blocks are available for size $2^K$ words, $L \leq K \leq U$ where
  - $2^L =$ smallest size block that is allocated
  - $2^U =$ largest size block that is allocated, generally $2^U$ is the size of entire memory available for allocation.

  ![](@attachment/Clipboard_2020-10-03-08-24-35.png)
- On release, blocks of the same sibling can be combined to create a larger unallocated space.
- Only leaves are allocated in the tree 

![](@attachment/Clipboard_2020-10-03-08-38-16.png)

### Address
#### Logical
- Independent of current assignment of data to memory.
- Distance from base address

#### Relative
- Address is expressed as a location relative to some known point.
- A relative address is an example of logical address.
- Distance from page boundary

#### Physical or Absolute
Actual location in main memory

- 8086 offsets are relative as well as logical.
- 80286 segments don't hide appending zeros and therefore you can create segments of any size.

#### Relocation
- Bounds Register stores the last possible value.
- Greater than range - out of bounds, protection error.
- Why is hardware support required?


### Paging
- Entire program is not used at one go.
  - Execution can be said to be confined to a particular region at a time.
- Partition memory into equal fixed-size chunks that are relatively small.
- Process is also divided into small fixed-size chunks of the same size.
- Page size - size of a process chunk
- Frame size - size of a location in memory
- Base size and frame size must be power of 2 and same.

![](@attachment/Clipboard_2020-10-03-09-29-39.png)

