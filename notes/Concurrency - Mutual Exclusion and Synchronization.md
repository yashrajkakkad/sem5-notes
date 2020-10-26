---
tags: [operating-systems]
title: Concurrency - Mutual Exclusion and Synchronization
created: '2020-10-26T03:27:18.276Z'
modified: '2020-10-26T16:28:57.642Z'
---

# Concurrency - Mutual Exclusion and Synchronization

## Arises in Three Different Contexts
- Multiple Applications
- Structured Applications
- Operating System Structure

### Multiple Applications
- Invented to allow processing time to be shared among active applications

### Structured Applications
- Extension of modular design and structured programming.

### Operating System Software
- OS themselves implemented as a set of processes or threads.

## Threading in Java
- Without threads, concurrency is not implemented.
- Concurrency requires synchronization

### Implement Runnable vs extend Thread?
- Multiple Inheritance is not supported.
- Java threads are user level.
  - Kernel is not aware of them.

## Synchronization
- One thread waiting for another. The other thread must notify once its over.
- Two threads accessing same variable.

## Mutual Exclusion
- If critical section is shared among multiple threads or processes, you have to ensure that if one of them enters, others should automatically remain away from that.


