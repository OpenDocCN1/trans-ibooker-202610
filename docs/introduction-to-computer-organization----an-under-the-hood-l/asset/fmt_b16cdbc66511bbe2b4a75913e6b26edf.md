
In this chapter, we'll break down this process. We'll do this by discussing how to write characters on the screen and read characters from the keyboard in our `main` function. Starting with this chapter, we'll usually bypass the C standard library functions, `printf` and `scanf`, and use the system call functions `write` to output to the screen and `read` to input from the keyboard.

We'll start by discussing the `write` and `read` functions. Then we'll look at how arguments are passed to a function in the CPU registers. We'll next look at how the CPU can determine an address to pass to a function when that's needed. Then we'll look at how a data structure called the `call stack` is used for creating local variables within a function.

## The write and read System Call Functions

In Chapter 2 we used `printf` and `scanf`, from the C standard library, for writing to the screen and reading from the keyboard. As shown in [Figure 2-1](#c02.xhtml#figure2-1) (in Chapter 2), `printf` converts data from its memory storage format to a character format and calls the `write` system call function to display the characters on the screen. When reading characters from the keyboard, `scanf` calls the `read` system call function and converts the characters to a memory storage format.

Linux sees the screen and keyboard as files. When a program is first launched, the operating system opens three files—`standard in`, `standard out`, and `standard error`—and assigns an integer to each file that is called a `file descriptor`. The program interacts with each file by using the `file descriptor`. The C interfaces for calling `read` and `write` are specified in the `Portable Operating System Interface` (`POSIX`) standard. The general formats for calling these two functions are

```c
int write(int fd, char *buf, int n);
int read(int fd, char *buf, int n);
```

where `fd` is a file descriptor, `buf` is the address of the character storage, and `n` is the number of characters to read or write. You can see more details in the `man` pages for `write` and `read`:

```
man 2 write
man 2 read
```

[Table 11-1](#c11.xhtml#table11-1) shows the file descriptors we'll use and the device each is normally associated with.

**Table 11-1: File Descriptors for `write` and `read` System Call Functions**

| Name | Number | Use |
|------|--------|-----|
| `STDIN_FILENO` | 0 | Read characters from keyboard |
| `STDOUT_FILENO` | 1 | Write characters to screen |
| `STDERR_FILENO` | 2 | Write error messages to screen |

These names are defined in the system header file, `unistd.h`, which is located at `/usr/include/unistd.h` on my Ubuntu system. (The location on your system may be different.)

Let's look at how to pass the appropriate arguments to the `write` function to write text on the screen.

## Passing Arguments in Registers

Up to six arguments can be passed in registers from one function to another in our environment. We'll look at how to pass more than six arguments in Chapter 14, and I'll note here that the Windows C environment allows only four arguments to be passed in registers.

Let's start with a program that does something very simple. We'll write "Hello, World!" on the screen by using the `write` system call function ([Listing 11-1](#c11.xhtml#listing11-1)).

```c
/* helloWorld.c
 * Hello World program using the write() system call.
 */

#include <unistd.h>

int main(void)
{

  write(STDOUT_FILENO, "Hello, World!\n", 14);

  return 0;
}
```

**Listing 11-1: "Hello, World!" program using the `write` system call function**

This function passes three arguments to `write`. In principle, the C compiler—or you, when you're writing in assembly language—could use any of the 16 general-purpose registers, except `rsp`, to pass arguments from one function to another. (The reason you can't use `rsp` will be explained in a moment.) Just store the arguments in the registers and call the desired function. Of course, the compiler, or a person writing in assembly language, needs to know exactly which register each argument is in when it comes to the called function.

The best way to avoid making mistakes is to develop a standard set of rules and follow them. This is especially important if more than one person is writing code for a program. Other people have realized the importance of having such standards and have given a good set of standards for passing arguments in `System V Application Binary Interface AMD64 Architecture Processor Supplement (with LP64 and ILP32 Programming Models) Version 1.0`. I found the January 28, 2018, version in PDF format at [https://github.com/hjl-tools/x86-psABI/wiki/x86-64-psABI-1.0.pdf](https://github.com/hjl-tools/x86-psABI/wiki/x86-64-psABI-1.0.pdf). (The latest version is maintained in LaTeX source at [https://gitlab.com/x86-psABIs/x86-64-ABI](https://gitlab.com/x86-psABIs/x86-64-ABI)`*/*`, but you need `pdflatex` to build a PDF version.) The compiler we're using, `gcc`, follows the rules in the System V standards, and we'll do the same for the assembly language we write.

[Table 11-2](#c11.xhtml#table11-2) summarizes the System V standards for using registers.

**Table 11-2: General-Purpose Register Usage**

| Register | Special usage | Save? |
|----------|---------------|-------|
| `rax` | Return first value from function | No |
| `rbx` | General-purpose | Yes |
| `rcx` | Pass fourth argument to function | No |
| `rdx` | Pass third argument to function; return second value from function | No |
| `rsp` | Stack pointer | Yes |
| `rbp` | Optional frame pointer | Yes |
| `rdi` | Pass first argument to function | No |
| `rsi` | Pass second argument to function | No |
| `r8` | Pass fifth argument to function | No |
| `r9` | Pass sixth argument to function | No |
| `r10` | Pass function's static chain pointer | No |
| `r11` | None | No |
| `r12` | None | Yes |
| `r13` | None | Yes |
| `r14` | None | Yes |
| `r15` | None | Yes |

The Save? column shows whether a called function needs to preserve the value in that register for the calling function. You'll learn how to do this in the next few sections.

The first six arguments are passed in registers `rdi`, `rsi`, `rdx`, `rcx`, `r8`, and `r9`, reading from left to right in a C function. [Listing 11-2](#c11.xhtml#listing11-2) shows the assembly language generated by `gcc` for the C function in [Listing 11-1](#c11.xhtml#listing11-1). This illustrates how to pass the three required arguments to the `write` function.

```nasm
        .file   "helloWorld.c"
        .intel_syntax noprefix
        .text
      1 .section  .rodata
2 .LC0:
        .string "Hello, World!\n"
        .text
        .globl  main
        .type   main, @function
main:
        push    rbp
        mov     rbp, rsp
        mov     edx, 314         ## number of chars
        lea     rsi, 4.LC0[rip]  ## address of string
        mov     edi, 51          ## STDOUT_FILENO
        call  6 write@PLT
        mov     eax, 0
        pop     rbp
        ret
        .size   main, .-main
        .ident  " GCC: (Ubuntu 9.3.0-17ubuntu1~20.04) 9.3.0"
        .section  .note.GNU-stack,"",@progbits
```

**Listing 11-2: Assembly language generated by `gcc` for program in [Listing 11-1](#c11.xhtml#listing11-1)**

When programming in assembly language, it's common to store the arguments in the registers starting with the last argument in the argument list, working your way to the first argument. In [Listing 11-2](#c11.xhtml#listing11-2), the third argument to `write`, the number of characters 3, is stored first in the register for the third argument, `edx`. The second argument is the address of the first character in the string 4, which goes in `rsi`. The first argument, the device to write to 5, is stored in `edi` just before the call to `write` 6.
