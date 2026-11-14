
# Figure 14-5: Scope and lifetime of three classes of C variables

[Figure 14-5](#c14.xhtml#figure14-5) shows a run of this program.

![Figure 14-5: Scope and lifetime of three classes of C variables](image_fi/500099c14/f14005.png)

The program shows that the `x` and `y` variables in `main` are different from the `x` and `y` variables in `addConst`. We can also see that the `x` in `addConst` is newly initialized each time the function is called. But the `y` variable in `addConst` is given its initial value only the first time the function is called, and the addition of 1,000 to the variable persists between the two calls to the function. We can also see that both `main` and `addConst` are using the same (global) `z`.

My assembly language solution to the `varLife` program, Listings [14-23](#c14.xhtml#listing14-23) and [14-24](#c14.xhtml#listing14-24), is similar to what the compiler generated, but I use more meaningful labels and names to make the code easier for a human to read.

## Listing 14-23: Assembly language version of the `main` function for the `varLife` program

```
# varLife.s
# Compares scope and lifetime of automatic, static, and global variables.
        .intel_syntax noprefix

# Stack frame
        .equ    x,-8
        .equ    y,-4
        .equ    localSize,-16
# Useful constants
        .equ    INITx,12
        .equ    INITy,34
        .equ    INITz,56
        .section    .rodata
        .align  8
tableHead1:
        .string "           automatic   static   global"
tableHead2:
        .string "                   x        y        z"
format:
        .string "In main:%12i %8i %8i\n"
# Define global variable
        .data
        .align  4
        .globl  z
        .type   z, @object
z:
      1 .int    INITz   # initialize the global
# Code
        .text
        .globl  main
        .type   main, @function
main:
        push    rbp                     # save frame pointer
        mov     rbp, rsp                # set new frame pointer
        add     rsp, localSize          # for local var.

        mov   2 dword ptr x[rbp], INITx # initialize locals
        mov     dword ptr y[rbp], INITy

        lea     rdi, tableHead1[rip]    # print heading
        call    puts@plt
        lea     rdi, tableHead2[rip]
        call    puts@plt
        mov   3 ecx, z[rip]             # print variables
        mov     edx, y[rbp]
        mov     esi, x[rbp]
        lea     rdi, format[rip]
        mov     eax, 0
        call    printf@plt

        call    addConst                # add to variables
        call    addConst

        mov     ecx, z[rip]             # print variables
        mov     edx, y[rbp]
        mov     esi, x[rbp]
        lea     rdi, format[rip]
        mov     eax, 0
        call    printf@plt

        mov     eax, 0                  # return 0;
        mov     rsp, rbp                # restore stack pointer
        pop     rbp                     # and caller frame pointer
        ret
```

The `.int` directive is the same as the `.long` used by the compiler. In our environment, both emit a 32-bit integer in little endian order. I prefer using the `.int` directive rather than `.long` to specify an `int` value 1, but this is just personal style.

Since `x` and `y` in this function are automatic local variables, they are in the stack frame and accessed relative to the frame pointer 2, while `z` is a global so accessed relative to the instruction pointer 3.

## Listing 14-24: Assembly language version of the `addConst` function

When we write the `addConst` function in assembly language, we’ll use a more meaningful identifier to mangle the static local variable, as shown in [Listing 14-24](#c14.xhtml#listing14-24).

```
# addConst.s
# Adds constant to automatic, static, global variables.
        .intel_syntax noprefix

# Stack frame
        .equ  1 x,-4
        .equ    localSize,-16
# Useful constants
        .equ    ADDITION,1000
        .equ    INITx,78
        .equ    INITy,90
# Constant data
        .section  .rodata
        .align  8
format:
        .string "In addConst:%8i %8i %8i\n"
# Define static variable
        .data
        .align  4
        .type 2 y_addConst, @object
y_addConst:
      3 .int    INITy

# Code
        .text
        .globl  addConst
        .type   addConst, @function
addConst:
        push    rbp                     # save frame pointer
        mov     rbp, rsp                # set new frame pointer
        add     rsp, localSize          # for local var.
        mov     dword ptr x[rbp], INITx # initialize

        add     dword ptr x[rbp], ADDITION # add to vars
        add   4 dword ptr y_addConst[rip], ADDITION
        add     dword ptr z[rip], ADDITION

        mov     ecx, z[rip]             # print variables
        mov     edx, y_addConst[rip]
        mov     esi, x[rbp]
        lea     rdi, format[rip]
        mov     eax, 0                  # no floats
        call    printf@plt

        mov     eax, 0                  # return 0;
        mov     rsp, rbp                # restore stack pointer
        pop     rbp                     # and caller frame pointer
        ret
```

Be careful to notice that the automatic local variable, `x`, here is created in the stack frame for `addConst` 1 so is different from the `x` that was created in the stack frame for `main`. I have chosen to mangle the static local variable, `y`, by appending the function name to the name of the variable, thus assuring a unique name to the variable 2. Since `y` is static, it’s placed in the `.data` section 3 and accessed relative to the instruction pointer 4.

## Table 14-1: Memory Characteristics of Some Components of a Program

[Table 14-1](#c14.xhtml#table14-1) summarizes the memory characteristics of some of the most common components for a program.

| Role in the program | Memory segment | Access | Lifetime |
| --- | --- | --- | --- |
| Automatic local variable | Stack | Read and write | Function |
| Constant | Text | Read only | Program |
| Instruction | Text | Read only | Program |
| Static local variable | Data | Read and write | Program |
| Global variable | Data | Read and write | Program |

The memory segment in [Table 14-1](#c14.xhtml#table14-1) refers to the segments created by the operating system. Constants are placed in a text segment because the operating system prohibits a program from writing to a text segment. Variables need to be in a segment that can be both read from and written to.

## Table 14-2: Some Common Assembler Memory Directives

[Table 14-2](#c14.xhtml#table14-2) summarizes some of the more common assembler directives used to control where program components go in memory.

| Directive | Memory segment | Effect |
| --- | --- | --- |
| `.text` | Text | Instructions follow |
| `.rodata` | Text | Constant data follows |
| `.string "string"`, … | Text | Arrays of characters, each terminated by `NUL` |
| `.ascii "string"`, … | Text | Arrays of characters |
| `.asciz "string"`, … | Text | Arrays of characters, each terminated by `NUL` |
| `.data` | Data | Variable data follows |
| `.bss` | Data | Following data memory initialized to zero |
| `.comm label, size` | Data | Allocates `size` bytes of uninitialized data memory |
| `.byte expression`, … | Data | Initialize memory, one byte for each expression |
| `.int expression`, … | Data | Initialize memory, one `int` for each expression |
| `.long expression`, … | Data | Initialize memory, one `int` for each expression |

The `.string`, `.ascii`, and `.asciz` directives can allocate more than one text string, each separated by a comma. The `.string` and `.asciz` directives add a `NUL` character to the end of the text string, while `.ascii` does not.
