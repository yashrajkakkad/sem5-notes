---
tags: ['8086', os-lab]
title: String Processing in 8086
created: '2020-09-30T13:35:40.062Z'
modified: '2020-09-30T14:59:49.136Z'
---

# String Processing in 8086
- Examples:
  - Compiler
  - grep, awk etc.
  - Word processing software

String processing must be supported at hardware level as it is very ubiquitous. You can perform string processing in 8085 also, but it will be very complex.

8086 supports an extensive set of instructions. The design of the instructions is remarkable.

Common string operations:
- Copy
  - SI and DI can store the locations of source and destination.
  - Copy character by character and increment pointers.
- Compute length
- Comparison

## Instructions
MOVS/MOVSB/MOVSW

```
mov si, offset str1
mov di, offset str2
mov cx, 10
cld ; clear direction flag

movs str2, str1 ; both types are bytes so not a problem
or 
movsb
movsw ; increment by 2
```

```
rep movsb ; special provision for one instruction
```

To go from R->L, use `cld`. `mov` instructions will decrement SI and DI automatically.

- When you start PC, a jump instruction is executed by POST. Stored like `op f1 f2 s1 s2`.  POST will get bootloader. Bootloader will get OS. OS will do mode switch from single tasking to multi tasking.

BIOS doesn't just contain code but also contains data.
FFFF:0005 to consecutive 8 locations in a particular format. (DD-MM-YY - 8 characters)

## Load and store instructions

## Repeat Instructions
rep - Repeat until cx == 0
repe - Repeat until CX == 0 and ZF = 1
repne - Rpeat until CX == 0 and ZF = 0

## Multiple processes
& after command - command runs as long as terminal is on.
nohup command - process runs even after closing terminal

