<!-- Reference: https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=147 -->

# Overview

| Detail                | Notes                                                             |
| ----------------------| ------------------------------------------------------------------|
| Instruction Encoding  | 16-bit Variable Length Little Endian, *or* <br> 32-bit Fixed Size Little Endian (e.g. no "C" extension)
| Endian                | Little <br> (with big/bi-endian as nonstandard variants / privileged ISA modes)
| Bits                  | 32, 64, and 128-bit variants
| Registers             | 1 "zero" integer register, 15 ("E") or 31 ("I") general purpouse integer registers, 1 instruction pointer, optionally 32 floating point registers


## Registers

Per [Chapter 25: RISC-V Assembly Programmer’s Handbook](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=155) Table 25.1:

| I/E   | C     | Register  | ABI Name  | Description                                   | Register  | ABI Name  | Description   |
| -----:| -----:| ---------:| ----------| ----------------------------------------------| ----------| ----------| --------------|
| 00000 |       |  x0       | zero      | hard-wired **zero**                           |  f0       | ft0       | **T**emporary
| 00001 |       |  x1       | ra        | **R**eturn **A**ddress                        |  f1       | ft1
| 00010 |       |  x2       | sp        | **S**tack **P**ointer                         |  f2       | ft2
| 00011 |       |  x3       | gp        | **G**lobal **P**ointer                        |  f3       | ft3
| 00100 |       |  x4       | tp        | **T**hread **P**ointer                        |  f4       | ft4
| 00101 |       |  x5       | t0        | Temporary / alternate link register           |  f5       | ft5
| 00110 |       |  x6       | t1        | **T**emporary                                 |  f6       | ft6
| 00111 |       |  x7       | t2        |                                               |  f7       | ft7
| 01000 | 000   |  x8       | s0/fp     | **F**rame **P**ointer                         |  f8       | fs0       | **S**aved
| 01001 | 001   |  x9       | s1        | **S**aved registers                           |  f9       | fs1
| 01010 | 010   | x10       | a0        | function **A**rguments/return values          | f10       | fa0       | **A**rgument / return
| 01011 | 011   | x11       | a1        |                                               | f11       | fa1
| 01100 | 100   | x12       | a2        |                                               | f12       | fa2
| 01101 | 101   | x13       | a3        |                                               | f13       | fa3
| 01110 | 110   | x14       | a4        |                                               | f14       | fa4
| 01111 | 111   | x15       | a5        |                                               | f15       | fa5
|       |       |           |           | Registers beyond this point require "I" base ISA (default), unavailable on "E" variant
| 10000 |       | x16       | a6        |                                               | f16       | fa6
| 10001 |       | x17       | a7        |                                               | f17       | fa7
| 10010 |       | x18       | s2        |                                               | f18       | fs2       | Saved
| 10011 |       | x19       | s3        |                                               | f19       | fs3
| 10100 |       | x20       | s4        |                                               | f20       | fs4
| 10101 |       | x21       | s5        |                                               | f21       | fs5
| 10110 |       | x22       | s6        |                                               | f22       | fs6
| 10111 |       | x23       | s7        |                                               | f23       | fs7
| 11000 |       | x24       | s8        |                                               | f24       | fs8
| 11001 |       | x25       | s9        |                                               | f25       | fs9
| 11010 |       | x26       | s10       |                                               | f26       | fs10
| 11011 |       | x27       | s11       |                                               | f27       | fs11
| 11100 |       | x28       | t3        | Temporary                                     | f28       | ft8       | Temporary
| 11101 |       | x29       | t4        |                                               | f29       | ft9
| 11110 |       | x30       | t5        |                                               | f30       | ft10
| 11111 |       | x31       | t6        |                                               | f31       | ft11

# Decoding

## Instruction Length

> [Chapter 1: Introduction](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=19)
> § [1.5 Base Instruction-Length Encoding](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=25)
>
> \[...\] the standard RISC-V encoding scheme is designed to support ISA extensions
> with variable-length instructions, where each instruction can be any number of 16-bit instruction
> parcels in length and parcels are naturally aligned on 16-bit boundaries. \[...\]
>
> #### Expanded Instruction-Length Encoding
>
> \[...\] the following proposal for encoding instructions longer than 32 bits is not considered frozen. \[...\]
>
> | base\[4..\]   | base\[2..4\]      | base\[0..2\]      | Instruction Length    |
> | -------------:| -----------------:| -----------------:| ----------------------|
> |               |                   | `xxxxxxxxxxxxxxaa`| 16-bit (aa ≠ 11)
> |               | `xxxxxxxxxxxxxxxx`| `xxxxxxxxxxxbbb11`| 32-bit (bbb ≠ 111)
> |    `··· xxxx` | `xxxxxxxxxxxxxxxx`| `xxxxxxxxxx011111`| 48-bit
> |    `··· xxxx` | `xxxxxxxxxxxxxxxx`| `xxxxxxxxx0111111`| 64-bit
> |    `··· xxxx` | `xxxxxxxxxxxxxxxx`| `xnnnxxxxx1111111`| (80+16*nnn)-bit, nnn ≠ 111
> |    `··· xxxx` | `xxxxxxxxxxxxxxxx`| `x111xxxxx1111111`| Reserved for ≥192-bits

## Decoding 32-bit Instructions (inst\[1:0\]=11)

[Chapter 24: RV32/64G Instruction Set Listings](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=147)
• Table 24.1: RISC-V base opcode map, inst\[1:0\]=11

| inst\[4:2\] <hr> inst\[6:5\]  | 000       | 001       | 010       | 011       | 100       | 101       | 110               | 111 <br> (> 32*b*) |
| ------------------------------| ----------| ----------| ----------| ----------| ----------| ----------| ------------------| -----------------|
| 00                            | LOAD      | LOAD-FP   | *custom-0*| MISC-MEM  | OP-IMM    | AUIPC     | OP-IMM-32         | 48*b*
| 01                            | STORE     | STORE-FP  | *custom-1*| AMO       | OP        | LUI       | OP-32             | 64*b*
| 10                            | MADD      | MSUB      | NMSUB     | NMADD     | OP-FP     | *reserved*| *custom-2/rv128*  | 48*b*
| 11                            | BRANCH    | JALR      | *reserved*| JAL       | SYSTEM    | *reserved*| *custom-3/rv128*  | ≥ 80*b*

Or, flattened:

| inst\[6:5\] | inst\[4:2\] | inst\[1:0\] | Opcode     |
| -----------:| -----------:| -----------:| -----------|
| 00 | 000 | 11 | LOAD
| 00 | 001 | 11 | LOAD-FP
| 00 | 010 | 11 | *custom-0*
| 00 | 011 | 11 | MISC-MEM
| 00 | 100 | 11 | OP-IMM
| 00 | 101 | 11 | AUIPC
| 00 | 110 | 11 | OP-IMM-32
| 00 | 111 | 11 | 48*b*
| 01 | 000 | 11 | STORE
| 01 | 001 | 11 | STORE-FP
| 01 | 010 | 11 | *custom-1*
| 01 | 011 | 11 | AMO
| 01 | 100 | 11 | OP
| 01 | 101 | 11 | LUI
| 01 | 110 | 11 | OP-32
| 01 | 111 | 11 | 64*b*
| 10 | 000 | 11 | MADD
| 10 | 001 | 11 | MSUB
| 10 | 010 | 11 | NMSUB
| 10 | 011 | 11 | NMADD
| 10 | 100 | 11 | OP-FP
| 10 | 101 | 11 | *reserved*
| 10 | 110 | 11 | *custom-2/rv128*
| 10 | 111 | 11 | 48*b*
| 11 | 000 | 11 | BRANCH
| 11 | 001 | 11 | JALR
| 11 | 010 | 11 | *reserved*
| 11 | 011 | 11 | JAL
| 11 | 100 | 11 | SYSTEM
| 11 | 101 | 11 | *reserved*
| 11 | 110 | 11 | *custom-3/rv128*
| 11 | 111 | 11 | ≥ 80*b*

## Decoding 16-bit Compressed Instruction (inst\[1:0\]≠11, "C" extension)

See [Chapter 16: “C” Standard Extension for Compressed Instructions, Version 2.0](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=115):
*   [16.1 Overview](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=115)
*   [16.2 Compressed Instruction Formats](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=117)
*   [16.3 Load and Store Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=119)
*   [16.4 Control Transfer Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=122)
*   [16.5 Integer Computational Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=124)
*   [16.6 Usage of C Instructions in LR/SC Sequences](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=128)
*   [16.7 HINT Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=128)
*   [16.8 RVC Instruction Set Listings](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=130)

# Bibliography

*   [Volume I: RISC-V Unprivileged ISA V20191213](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf)
    *   [Chapter 1: Introduction](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=19)
        *   [1.1 RISC-V Hardware Platform Terminology](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=20)
        *   [1.2 RISC-V Software Execution Environments and Harts](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=21)
        *   [1.3 RISC-V ISA Overview](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=22)
        *   [1.4 Memory](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=24)
        *   [1.5 Base Instruction-Length Encoding](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=25)
        *   [1.6 Exceptions, Traps, and Interrupts](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=28)
        *   [1.7 UNSPECIFIED Behaviors and Values](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=29)
    *   [Chapter 2: RV32I Base Integer Instruction Set, Version 2.1](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=31)
        *   [2.1 Programmers’ Model for Base Integer ISA](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=31)
        *   [2.2 Base Instruction Formats](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=33)
        *   [2.3 Immediate Encoding Variants](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=34)
        *   [2.4 Integer Computational Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=35)
        *   [2.5 Control Transfer Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=38)
        *   [2.6 Load and Store Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=42)
        *   [2.7 Memory Ordering Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=44)
        *   [2.8 Environment Call and Breakpoints](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=45)
        *   [2.9 HINT Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=46)
    *   [Chapter 3: “Zifencei” Instruction-Fetch Fence, Version 2.0](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=49)
    *   [Chapter 4: RV32E Base Integer Instruction Set, Version 1.9](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=51)
        *   [4.1 RV32E Programmers’ Model](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=51)
        *   [4.2 RV32E Instruction Set](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=52)
    *   [Chapter 5: RV64I Base Integer Instruction Set, Version 2.1](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=53)
        *   [5.1 Register State](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=53)
        *   [5.2 Integer Computational Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=53)
        *   [5.3 Load and Store Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=55)
        *   [5.4 HINT Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=56)
    *   [Chapter 6: RV128I Base Integer Instruction Set, Version 1.7](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=59)
    *   [Chapter 7 “M” Standard Extension for Integer Multiplication and Division, Version 2.0](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=61)
        *   [7.1 Multiplication Operations](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=61)
        *   [7.2 Division Operations](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=62)
    *   [Chapter 8: “A” Standard Extension for Atomic Instructions, Version 2.1](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=65)
        *   [8.1 Specifying Ordering of Atomic Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=65)
        *   [8.2 Load-Reserved/Store-Conditional Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=66)
        *   [8.3 Eventual Success of Store-Conditional Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=69)
        *   [8.4 Atomic Memory Operations](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=70)
    *   [Chapter 9: “Zicsr”, Control and Status Register (CSR) Instructions, Version 2.0](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=73)
        *   [9.1 CSR Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=73)
    *   [Chapter 10: Counters](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=77)
        *   [10.1 Base Counters and Timers](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=77)
        *   [10.2 Hardware Performance Counters](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=79)
    *   [Chapter 11: “F” Standard Extension for Single-Precision Floating-Point, Version 2.2](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=81)
        *   [11.1 F Register State](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=81)
        *   [11.2 Floating-Point Control and Status Register](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=83)
        *   [11.3 NaN Generation and Propagation](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=84)
        *   [11.4 Subnormal Arithmetic](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=85)
        *   [11.5 Single-Precision Load and Store Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=85)
        *   [11.6 Single-Precision Floating-Point Computational Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=85)
        *   [11.7 Single-Precision Floating-Point Conversion and Move Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=87)
        *   [11.8 Single-Precision Floating-Point Compare Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=89)
        *   [11.9 Single-Precision Floating-Point Classify Instruction](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=89)
    *   [Chapter 12: “D” Standard Extension for Double-Precision Floating-Point, Version 2.2](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=91)
        *   [12.1 D Register State](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=91)
        *   [12.2 NaN Boxing of Narrower Values](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=91)
        *   [12.3 Double-Precision Load and Store Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=92)
        *   [12.4 Double-Precision Floating-Point Computational Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=93)
        *   [12.5 Double-Precision Floating-Point Conversion and Move Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=93)
        *   [12.6 Double-Precision Floating-Point Compare Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=95)
        *   [12.7 Double-Precision Floating-Point Classify Instruction](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=95)
    *   [Chapter 13: “Q” Standard Extension for Quad-Precision Floating-Point, Version 2.2](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=97)
        *   [13.1 Quad-Precision Load and Store Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=97)
        *   [13.2 Quad-Precision Computational Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=98)
        *   [13.3 Quad-Precision Convert and Move Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=98)
        *   [13.4 Quad-Precision Floating-Point Compare Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=99)
        *   [13.5 Quad-Precision Floating-Point Classify Instruction](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=100)
    *   [Chapter 14: RVWMO Memory Consistency Model, Version 0.1](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=101)
        *   [14.1 Definition of the RVWMO Memory Model](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=102)
        *   [14.2 CSR Dependency Tracking Granularity](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=106)
        *   [14.3 Source and Destination Register Listings](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=106)
    *   [Chapter 15: “L” Standard Extension for Decimal Floating-Point, Version 0.0](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=113)
        *   [15.1 Decimal Floating-Point Registers](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=113)
    *   [Chapter 16: “C” Standard Extension for Compressed Instructions, Version 2.0](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=115)
        *   [16.1 Overview](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=115)
        *   [16.2 Compressed Instruction Formats](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=117)
        *   [16.3 Load and Store Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=119)
        *   [16.4 Control Transfer Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=122)
        *   [16.5 Integer Computational Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=124)
        *   [16.6 Usage of C Instructions in LR/SC Sequences](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=128)
        *   [16.7 HINT Instructions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=128)
        *   [16.8 RVC Instruction Set Listings](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=130)
    *   [Chapter 17: “B” Standard Extension for Bit Manipulation, Version 0.0](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=133)
    *   [Chapter 18: “J” Standard Extension for Dynamically Translated Languages, Version 0.0](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=135)
    *   [Chapter 19: “T” Standard Extension for Transactional Memory, Version 0.0](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=137)
    *   [Chapter 20: “P” Standard Extension for Packed-SIMD Instructions, Version 0.2](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=139)
    *   [Chapter 21: “V” Standard Extension for Vector Operations, Version 0.7](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=141)
    *   [Chapter 22: “Zam” Standard Extension for Misaligned Atomics, v0.1](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=143)
    *   [Chapter 23: “Ztso” Standard Extension for Total Store Ordering, v0.1](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=145)
    *   [Chapter 24: RV32/64G Instruction Set Listings](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=147)
    *   [Chapter 25: RISC-V Assembly Programmer’s Handbook](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=155)
    *   [Chapter 26: Extending RISC-V](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=159)
        *   [26.1 Extension Terminology](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=159)
        *   [26.2 RISC-V Extension Design Philosophy](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=162)
        *   [26.3 Extensions within fixed-width 32-bit instruction format](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=162)
        *   [26.4 Adding aligned 64-bit instruction extensions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=164)
        *   [26.5 Supporting VLIW encodings](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=164)
    *   [Chapter 27: ISA Extension Naming Conventions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=167)
        *   [27.1 Case Sensitivity](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=167)
        *   [27.2 Base Integer ISA](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=167)
        *   [27.3 Instruction-Set Extension Names](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=167)
        *   [27.4 Version Numbers](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=168)
        *   [27.5 Underscores](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=168)
        *   [27.6 Additional Standard Extension Names](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=168)
        *   [27.7 Supervisor-level Instruction-Set Extensions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=169)
        *   [27.8 Hypervisor-level Instruction-Set Extensions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=169)
        *   [27.9 Machine-level Instruction-Set Extensions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=169)
        *   [27.10 Non-Standard Extension Names](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=169)
        *   [27.11 Subset Naming Convention](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=170)
    *   [Chapter 28: History and Acknowledgments](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=171)
        *   [28.1 “Why Develop a new ISA?” Rationale from Berkeley Group](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=171)
        *   [28.2 History from Revision 1.0 of ISA manual](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=173)
        *   [28.3 History from Revision 2.0 of ISA manual](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=174)
        *   [28.4 History from Revision 2.1](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=176)
        *   [28.5 History from Revision 2.2](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=176)
        *   [28.6 History for Revision 2.3](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=177)
        *   [28.7 Funding](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=177)
    *   [Appendix A: RVWMO Explanatory Material, Version 0.1](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=179)
        *   [A.1 Why RVWMO?](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=179)
        *   [A.2 Litmus Tests](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=180)
        *   [A.3 Explaining the RVWMO Rules](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=181)
            *   [A.3.1 Preserved Program Order and Global Memory Order](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=181)
            *   [A.3.2 Load Value Axiom](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=182)
            *   [A.3.3 Atomicity Axiom](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=185)
            *   [A.3.4 Progress Axiom](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=186)
            *   [A.3.5 Overlapping-Address Orderings (Rules 1–3)](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=186)
            *   [A.3.6 Fences (Rule 4)](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=188)
            *   [A.3.7 Explicit Synchronization (Rules 5–8)](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=189)
            *   [A.3.8 Syntactic Dependencies (Rules 9–11)](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=191)
            *   [A.3.9 Pipeline Dependencies (Rules 12–13)](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=194)
        *   [A.4 Beyond Main Memory](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=195)
            *   [A.4.1 Coherence and Cacheability](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=196)
            *   [A.4.2 I/O Ordering](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=196)
        *   [A.5 Code Porting and Mapping Guidelines](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=198)
        *   [A.6 Implementation Guidelines](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=202)
            *   [A.6.1 Possible Future Extensions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=205)
        *   [A.7 Known Issues](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=206)
            *   [A.7.1 Mixed-size RSW](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=206)
    *   [Appendix B: Formal Memory Model Specifications, Version 0.1](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=209)
        *   [B.1 Formal Axiomatic Specification in Alloy](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=210)
        *   [B.2 Formal Axiomatic Specification in Herd](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=215)
        *   [B.3 An Operational Memory Model](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=219)
            *   [B.3.1 Intra-instruction Pseudocode Execution](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=222)
            *   [B.3.2 Instruction Instance State](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=224)
            *   [B.3.3 Hart State](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=225)
            *   [B.3.4 Shared Memory State](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=225)
            *   [B.3.5 Transitions](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=226)
            *   [B.3.6 Limitations](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=234)
    *   [Bibliography](https://riscv.org/wp-content/uploads/2019/12/riscv-spec-20191213.pdf#page=237)
