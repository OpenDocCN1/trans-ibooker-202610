
Most instructions in a program use at least one register in at least one of the register files. A program typically loads data from memory into a register, operates on the data, and stores the result in memory. Registers are also used to hold addresses of items that are stored in memory, thus serving as pointers to data or other addresses.

The remainder of this chapter is mostly devoted to describing the general-purpose registers in the x86-64 architecture. You’ll learn how to view their contents in the `gdb` debugger. Then in the next chapter, you’ll learn how to start using them in assembly language.

## x86-64 Registers

A portion of the memory in the CPU is organized into registers. Machine instructions access CPU registers by their addresses, just like they access main memory. Of course, the register addressing space is separate from the main memory addressing space. Register addresses are placed on the internal CPU bus, not on the address portion of the bus interface, since the registers are in the CPU. The difference from a programmer’s point of view is that the assembler has predefined names for the registers, whereas the programmer creates symbolic names for memory addresses. Thus, in each program that you write in assembly language, the following happens:

*   CPU registers are accessed by using the names that are predefined in the assembler.
*   Memory is accessed by the programmer providing a name for the memory location and using that name in the user program.

[Table 9-1](#c09.xhtml#table9-1) lists the x86-64 architecture registers, which groups them according to their general usage in a program. Within each general category, the columns in the table show the number of registers, the size of each register, and the usage of each register in a program.

**Table 9-1: The x86-64 Registers**

| Category | Count | Size | Usage |
| --- | --- | --- | --- |
| **Basic programming registers** | | | |
| | 16 | 64-bit | `General-purpose` |
| | 1 | 64-bit | `Flags` |
| | 1 | 64-bit | `Instruction pointer` |
| | 6 | 16-bit | `Segment` |
| **Floating-point registers** | | | |
| | 8 | 80-bit | `Floating-point data` |
| | 1 | 16-bit | `Control` |
| | 1 | 16-bit | `Status` |
| | 1 | 16-bit | `Tag` |
| | 1 | 11-bit | `Opcode` |
| | 1 | 64-bit | `FPU instruction pointer` |
| | 1 | 64-bit | `FPU data pointer` |
| **MMX registers** | | | |
| | 8 | 64-bit | `MMX` |
| **XMM registers** | | | |
| | 16 | 128-bit | `XMM` |
| | 1 | 32-bit | `MXCR` |
| **Model-specific registers (MSRs)** | Vary depending on implementation. Used only by operating system. | | |

I’ve already described the instruction pointer register. Most of the programming concepts presented in this book use only the general-purpose registers. These are used for *integral data types*, such as `int` and `char` integer values (signed and unsigned), character representations, Boolean values, and addresses. In the remainder of this section, we’ll look at the general-purpose registers and the flags register. We’ll discuss the floating-point registers near the end of the book. The MMX and XMM registers are used for more advanced programming techniques that we won’t cover.

### General-Purpose Registers

As mentioned, the general-purpose registers deal with integral data types and memory addresses. Each bit in each register is numbered from right to left, beginning with 0. So, the rightmost bit is number 0, the next one to the left is 1, and so on. Since there are 64 bits in each general-purpose register, the leftmost bit is 63.

Each instruction in a computer treats a group of bits as a single unit. In the early days, that unit was called a *word*. Each CPU architecture had a *word size*. In modern CPU architectures, different instructions operate on different numbers of bits, but the terminology has carried over from the early days of the Intel 8086 instruction set architecture to the current 64-bit instruction set architecture, x86-64. Hence, 16 bits is called a *word*, 32 bits a *doubleword*, and 64 bits a *quadword*.

You can access the bits in each general-purpose register by using the following groupings in your programs:

1.  **Quadword** All 64 bits (63–0)
2.  **Doubleword** The low-order 32 bits (31–0)
3.  **Word** The low-order 16 bits (15–0)
4.  **Byte** The low-order 8 bits (7–0), and in four registers bits (15–8)

The assembler uses a different name for each group of bits in a register. [Table 9-2](#c09.xhtml#table9-2) lists the assembler names for the groups of the bits. Each row in the table represents one register, and each column represents the name for that grouping of bits in the register.

**Table 9-2: Assembly Language Names for Portions of the General-Purpose CPU Registers**

| **Bits 63–0** | **Bits 31–0** | **Bits 15–0** | **Bits 15–8** | **Bits 7–0** |
| --- | --- | --- | --- | --- |
| `rax` | `eax` | `ax` | `ah` | `al` |
| `rbx` | `ebx` | `bx` | `bh` | `bl` |
| `rcx` | `ecx` | `cx` | `ch` | `cl` |
| `rdx` | `edx` | `dx` | `dh` | `dl` |
| `rsi` | `esi` | `si` | | `sil` |
| `rdi` | `edi` | `di` | | `dil` |
| `rbp` | `ebp` | `bp` | | `bpl` |
| `rsp` | `esp` | `sp` | | `spl` |
| `r8` | `r8d` | `r8w` | | `r8b` |
| `r9` | `r9d` | `r9w` | | `r9b` |
| `r10` | `r10d` | `r10w` | | `r10b` |
| `r11` | `r11d` | `r11w` | | `r11b` |
| `r12` | `r12d` | `r12w` | | `r12b` |
| `r13` | `r13d` | `r13w` | | `r13b` |
| `r14` | `r14d` | `r14w` | | `r14b` |
| `r15` | `r15d` | `r15w` | | `r15b` |

In 64-bit mode, writing to an 8-bit or 16-bit portion of a register doesn’t affect the other 56 or 48 bits in the register. However, when writing to the low-order 32 bits, the high-order 32 bits are set to 0. (I don’t know why the CPU designers chose this behavior, which seems odd to me.) Programs running in 32-bit mode can use only the registers above the line in [Table 9-2](#c09.xhtml#table9-2). 64-bit mode can use all the registers. The `ah`, `bh`, `ch`, and `dh` registers (bits 15–8) can’t be used in the same instruction with any of the registers below the line. For example, you cannot copy the 8-bit value in the `ah` register to the `sil` register with a single instruction, but you could copy it to the `dl` register.

Most CPU architectures name their registers `r0`, `r1`, and so on. When Intel introduced the 8086/8088 instruction set architecture, it used the names above the line in the columns for bits 15–0, 15–8, and 7–0 in [Table 9-2](#c09.xhtml#table9-2). The four 16-bit registers, `ax`, `bx`, `cx`, and `dx`, were more general-purpose than the other four and were used for most of the computations in the CPU. The programmer could access either the entire register or one-half of each register. The low-order bytes were named `al`, `bl`, `cl`, and `dl`, and the high-order bytes named `ah`, `bh`, `ch`, and `dh`.

Access to these 8-bit and 16-bit registers has been maintained in 32-bit mode for backward compatibility but is limited in 64-bit mode. Access to the 8-bit low-order portions of the `rsi`, `rdi`, `rsp`, and `rbp` registers was added along with the move to 64 bits in the x86-64 architecture but cannot be used in the same instruction with the 8-bit register portions of the `ah`, `bh`, `ch`, or `dh` registers.

The `e` prefix on the 32-bit portion of each register name comes from the history of the x86 architecture. The introduction of the 80386 in 1986 brought an increase of register size from 16 bits to 32 bits. There were no new registers; the old ones were simply “extended.” In addition to increasing the register size to 64 bits, the introduction of the x86-64 architecture added eight more registers, named `r8`–`r15`. Rather than change the names of the first eight registers, I assume that the designers decided to use the `r` prefix on the historical names.
