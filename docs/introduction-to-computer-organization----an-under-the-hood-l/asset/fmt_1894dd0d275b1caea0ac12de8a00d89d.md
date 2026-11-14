
The Linux programming environment provides a header file, `io.h`, that includes functions to use the I/O ports. The interface with our UART takes bytes, so we’ll use `inb` and `outb`. You can read about these functions on their `man` page: `man inb`.

The algorithms for port-mapped I/O are the same as for memory mapped. But instead of accessing the port as a memory address, we use a number 1. And we need to call the appropriate function to transfer bytes to and from the UART 2.

When I tried to compile the file in [Listing 20-8](#c20.xhtml#listing20-8) by using the `-masm=intel` option, I got the following error messages:

```
$ gcc -c -masm=intel -Wall -g UART_functions.c
/usr/include/x86_64-linux-gnu/sys/io.h: Assembler messages:
/usr/include/x86_64-linux-gnu/sys/io.h:47: Error: operand type mismatch for `in'
/usr/include/x86_64-linux-gnu/sys/io.h:98: Error: operand type mismatch for `out'
```

I was puzzled so decided to look at the compiler-generated assembly language by changing the `-c` option to `-S`. I won’t go through the entire file, but let’s look at the first part of the compiler-generated assembly language, as shown in [Listing 20-9](#c20.xhtml#listing20-9).

```
        .file   "UART_functions.c"
        .intel_syntax noprefix
        .text
        .type   inb, @function
1 inb:
        push    rbp
        mov     rbp, rsp
        mov     eax, edi
        mov     WORD PTR -20[rbp], ax
        movzx   eax, WORD PTR -20[rbp]
        mov     edx, eax
2 #APP
# 47 "/usr/include/x86_64-linux-gnu/sys/io.h" 1
        inb dx,al
# 0 "" 2
#NO_APP
        mov     BYTE PTR -1[rbp], al
        movzx   eax, BYTE PTR -1[rbp]
        pop     rbp
        ret
        .size   inb, .-inb
        .type   outb, @function
outb:
        push    rbp
        mov     rbp, rsp
        mov     edx, edi
        mov     eax, esi
        mov     BYTE PTR -4[rbp], dl
        mov     WORD PTR -8[rbp], ax
        movzx   eax, BYTE PTR -4[rbp]
        movzx   edx, WORD PTR -8[rbp]
#APP
# 98 "/usr/include/x86_64-linux-gnu/sys/io.h" 1
        outb al,dx
# 0 "" 2
#NO_APP
        nop
        pop     rbp
        ret
        .size   outb, .-outb
        .globl  UART_init
        .type   UART_init, @function
UART_init:
        push    rbp
        mov     rbp, rsp
        sub     rsp, 24
        mov     eax, edi
        mov     WORD PTR -20[rbp], ax
        movzx   eax, WORD PTR -20[rbp]
        mov     WORD PTR -2[rbp], ax
        movzx   eax, WORD PTR -2[rbp]
        add     eax, 1
        movzx   eax, ax
        mov     esi, eax
        mov     edi, 0
        call    outb
        movzx   eax, WORD PTR -2[rbp]
        add     eax, 2
--snip--
```

Listing 20-9: Some compiler-generated assembly language for the functions in [Listing 20-8](#c20.xhtml#listing20-8)

The first thing to note is that the compiler has included the assembly language for the `inb` and `outb` functions 1. These functions are not part of the C standard library. They are meant to be used in the operating system code, not in applications programs. They are specific to the Linux kernel running on an x86-64 computer.

Next, we see that the actual `in` and `out` instructions are inserted into the code by macros 2. The macros insert these two instructions in AT&T syntax (see AT&T Syntax at the end of Chapter 10):

```
        inb     dx, al        ## at&t syntax
        outb    al, dx
```

As we saw earlier in this chapter, the instructions are written in Intel syntax as follows:

```
        in      al, dx        ## intel syntax
        out     dx, al
```

The problem here is that the assembly language used in the Linux kernel is written using the AT&T syntax, whereas we’re using the Intel syntax for assembly language. If we were to generate the assembly language without the `-masm=intel` option, all the assembly language would be in the AT&T syntax. If we use the C functions in `io.h` for port-mapped I/O, we cannot use the `-masm=intel` compiler option.

## What You’ve Learned

1. Memory timing: Memory access is synchronized with the timing of the CPU.
2. I/O timing: I/O devices are much slower than the CPU and have a wide range of characteristics, so we need to program their access.
3. Bus timing: Buses are often arranged in a hierarchical manner to better match the differences in timing between various I/O devices.
4. Port-mapped I/O: In this technique, I/O ports have their own address space.
5. Memory-mapped I/O: In this technique, I/O ports are given a portion of the main memory address space.
6. Polled I/O: The program waits in a loop until the I/O device is ready to transfer data.
7. Interrupt-driven I/O: The I/O device interrupts the CPU when it is ready to transfer data.
8. Direct memory access: The I/O device can transfer data to and from main memory without using the CPU.

In the next chapter, you’ll learn about the CPU features that allow it to maintain control over the I/O hardware and prevent application programs from accessing the hardware without going through the operating system.

# Chapter 21: Interrupts and Exceptions

![](image_fi/book_art/chapterart.png)

Thus far, we’ve viewed each application as having exclusive use of the computer. But like most operating systems, Linux allows multiple applications to be executing concurrently. The operating system manages the hardware in an interleaved fashion, providing each application, and the operating system itself, with the use of the hardware components it needs at any given time.

There are two issues here. First, for the operating system to carry out its management tasks, it needs to maintain control over the interaction between applications and hardware. It does this by using a system of privilege levels in the CPU that allows the operating system to control a gateway between applications and the operating system. Second, we saw near the end of the previous chapter that most I/O devices can interrupt the ongoing activity of the CPU when they are ready with input or ready to accept output. The CPU has a mechanism to direct I/O interruptions through this gateway and call functions that are under the control of the operating system, thus allowing the operating system to maintain its control over the I/O devices.

We’ll start by looking at how the CPU uses privilege levels to enforce its control. Then we’ll look at how the CPU reacts to an interrupt or exception, including the three ways to notify the CPU that its services are needed by an I/O device or an application: *external interrupt*, *exception*, or *software interrupt*. We’ll end the chapter by discussing how applications can directly call upon services of the operating system by using a software interrupt.

## Privilege Levels

For the operating system to carry out its management tasks, it needs to maintain control over the interaction between applications and hardware. It does this by using a system of *privilege levels* in the CPU that the operating system uses to maintain a gateway between applications and the hardware. At any given time, the CPU is running in one of four possible privilege levels. Table 21-1 shows the levels, from most privileged to least.

Table 21-1: CPU Privilege Levels

| Level | Usage |
| --- | --- |
| 0 | Provides direct access to all hardware resources. Restricted to the lowest-level operating system functions, such as I/O devices and memory management. |
| 1 | Somewhat restricted access to hardware resources. Might be used by some library routines and software that control I/O devices not requiring full access to the hardware. |
| 2 | More restricted access to hardware resources. Might be used by some library routines and software that control I/O devices not requiring less access than level 1. |
| 3 | No direct access to hardware resources. Applications run at this level. |
