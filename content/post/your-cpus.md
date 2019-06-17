+++
title =  "Know about your CPU"
date =  2018-05-18T09:57:55+01:00
tags = ["CPU","Hardware","linux"]
categories = ["tech"]
description = "You may get confused by the questions about your CPUs, here's some frequently questions I"
menu = ""
banner = "banners/cpus.jpg"
+++

## Find out what type is your computer's CPU
using the command `lscpu`

```
sherry@sherry:~/Documents/Blog/public$ lscpu
Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                4
On-line CPU(s) list:   0-3
Thread(s) per core:    2
Core(s) per socket:    2
Socket(s):             1
NUMA node(s):          1
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 78
Model name:            Intel(R) Core(TM) i5-6200U CPU @ 2.30GHz
Stepping:              3
CPU MHz:               1291.148
CPU max MHz:           2800,0000
CPU min MHz:           400,0000
BogoMIPS:              4800.00
Virtualization:        VT-x
L1d cache:             32K
L1i cache:             32K
L2 cache:              256K
L3 cache:              3072K
NUMA node0 CPU(s):     0-3
```
## ARM vs. x86


|  Types   |ARM | x86 |
|:---------|-----|------:|
|    ISA   |RISC| CISC|

whenever we mention the ARM vs. x86, it sounds like a fight between Apple and Intel+AMD.
**ARM** processors one of CPU's type, stands for `Advanced RICS Machine`. More commonly used `x86` processors made by Intel and AMD use the `CISC`. 


## CPU architecture types
Instruction set architecture (ISA) is a description of computer architecture based on a command
set it can execute. Usually we categorize it into two types:

1. **Complex Instruction Set Computers(CISC)**

	* Powerful, complex instructions
	* Instructions are variable in length (1 - n bytes)
2. **Reduced Instruction Set Computer(RISC)**
	* Fixed instruction length
    * Enables efficient pipelining & high clock frequencies
	* Clear distinction between data loading/storing and manipulation

##  Little Endian vs. Big Endian

**Big Endian Byte Order**: The **most significant byte** (the "big end") of the data is placed at the byte with the **lowest address**. The rest of the data is placed in order in the next three bytes in memory.

**Little Endian Byte Order**: The **least significant byte** (the "little end") of the data is placed at the byte with the **lowest address.** The rest of the data is placed in order in the next three bytes in memory.

## Glossary
**Bandwidth(B)**: Maximum data rate that can be held during a transfer

**Message Passing Interface (MPI)** : 
Message Passing Interface (MPI) is a community driven specifi-
cation for the message-passing model.

**Moore’s law**: The observation made by Gordon Moore that the number of transistors in an integrated
circuit doubles roughly every two years.

## Reference
https://www.maketecheasier.com/differences-between-arm-and-intel/
https://chortle.ccsu.edu/AssemblyTutorial/Chapter-15/ass15_3.html


