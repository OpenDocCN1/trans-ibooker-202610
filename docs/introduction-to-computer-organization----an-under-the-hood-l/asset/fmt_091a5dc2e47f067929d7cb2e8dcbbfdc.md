
## The algorithm for determining overflow in C

The algorithm for determining overflow in C (Listing 15-8) is somewhat complicated, so we'll take advantage of the fact that the CPU makes that determination during addition and sets the status flags in the `rflags` register accordingly. We can use the results in the `rflags` register by writing `addTwo` directly in assembly language, as shown in Listing 15-10.

```
# addTwo.s
# Adds two integers and returns OF
# Calling sequence:
#       edi <- x, 32-bit int
#       esi <- y, 32-bit int
#       rdx <- &z, place to store sum
#       returns value of OF
        .intel_syntax noprefix
# Stack frame
        .equ    temp,-4
        .equ    overflow,-8
        .equ    localSize,-16
# Code
        .text
        .globl  addTwo
        .type   addTwo, @function
addTwo:
        push    rbp         # save frame pointer
        mov     rbp, rsp    # set new frame pointer
        add     rsp, localSize  # for local var.

        add     edi, esi    # x + y
      1 seto    al          # OF T or F
        movzx   eax, al     # convert to int for return
        mov     [rdx] , edi # *c = sum
        
        mov     rsp, rbp    # restore stack pointer
        pop     rbp         # and caller frame pointer
        ret
```

Listing 15-10: Assembly language version of `addTwo` returns the value of the `OF` flag

We're using another instruction in Listing 15-10, `seto` 1. The `setcc` instructions are used to tell when various conditions, as shown in the condition codes, are true or false:

### `setcc`—Byte on Condition

1. Sets an 8-bit register to 0 or 1 depending on the condition `cc`.

2. `setcc reg` stores 1 in 8-bit `reg` when `cc` is true, 0 when `cc` is false.

The `setcc` instructions use the same `cc` codes as those in the `jcc` group, as shown in Table 13-1 in Chapter 13. `reg` is an 8-bit register, except `ah`, `bh`, `ch`, and `dh` cannot be used in 64-bit mode. C treats 1 as true and 0 as false.

Another difference in our assembly language version of `addTwo` is that we won't use the red zone for local variables. It's a matter of personal programming style, but I prefer to create a full stack frame so I don't have to worry about using the stack if I change the function. I also have not saved the input arguments. The values in the registers used for passing arguments do not need to be preserved for the calling function (see Table 11-2 in Chapter 11).

If we use the C version of the `main` function (Listing 15-6) to call our assembly language version of `addTwo`, we still need to `#include` the C header file (Listing 15-7) to tell the compiler how to call `addTwo`.

Comparing our assembly language solution (Listing 15-10) with what the compiler did (Listing 15-9) shows that we used about half as many instructions.

Our assembly language version of the `main` function for this program, as shown in Listing 15-11, is similar to what the compiler generates from the C version, but our use of symbolic names for offsets in the stack frame and labels makes it easier to read.

```
# sumInts.s
# Adds two integers using local variables
# Checks for overflow
        .intel_syntax noprefix
        
# Stack frame
        .equ    x,-24
        .equ    y,-20
        .equ    z,-16
        .equ    overflow,-12
        .equ    canary,-8
        .equ    localSize,-32
# Read only data
        .section  .rodata
askMsg:
        .string "Enter an integer: "
readFormat:
        .string "%i"
resultFormat:
        .string "%i + %i = %i\n"
overMsg:
        .string "    *** Overflow occurred ***\n"
# Code
        .text
        .globl  main
        .type   main, @function
main:
        push    rbp                 # save frame pointer
        mov     rbp, rsp            # set new frame pointer
        add     rsp, localSize      # for local var.
        mov     rax, fs:40          # get stack canary
        mov     canary[rbp], rax    # and save it
        
        mov     dword ptr x[rbp], 0 # x = 0
        mov     dword ptr y[rbp], 0 # y = 0

        lea     rdi, askMsg[rip]    # ask for integer
        mov     eax, 0
        call    printf@plt
        lea     rsi, x[rbp]         # place to store x
        lea     rdi, readFormat[rip]
        mov     eax, 0
        call    __isoc99_scanf@plt
        
        lea     rdi, askMsg[rip]    # ask for integer
        mov     eax, 0
        call    printf@plt
        lea     rsi, y[rbp]         # place to store y
        lea     rdi, readFormat[rip]
        mov     eax, 0
        call    __isoc99_scanf@plt
        
        lea     rdx, z[rbp]         # place to store sum
        mov     esi, x[rbp]         # load x
        mov     edi, y[rbp]         # load y
        call    addTwo
        mov     overflow[rbp], eax  # save overflow
        mov     ecx, z[rbp]         # load z
        mov     edx, y[rbp]         # load y
        mov     esi, x[rbp]         # load x
        lea     rdi, resultFormat[rip]
        mov     eax, 0              # no floating point
        call    printf@plt
        
        cmp     dword ptr overflow[rbp], 0 # overflow?
        je      noOverflow
        lea     rdi, overMsg[rip]   # yes, print message
        mov     eax, 0
        call    printf@plt
noOverflow:
        mov     eax, 0              # return 0

        mov     rcx, canary[rbp]    # retrieve saved canary
        xor     rcx, fs:40          # and check it
        je      goodCanary
        call    __stack_chk_fail@plt  # bad canary
goodCanary:
        mov     rsp, rbp            # restore stack pointer
        pop     rbp                 # and caller frame pointer
        ret
```

Listing 15-11: Assembly language version of the `main` function for the `sumIntegers` program

This example shows one of the reasons for writing a subfunction in assembly language—we were able to access a feature of the CPU, the `OF` in the `rflags` register, which is not accessible in the higher-level language we're using, C. And we needed to check for overflow immediately after the operation that we are checking is performed (addition, in this example).

This example also illustrates a common use of the return value. Inputs and outputs are often passed in the argument list, with supplemental information about the computation carried in the return value.

That said, calling a function to simply add two numbers is inefficient. In the next section, we'll look at a common extension to C that allows us to insert assembly language directly in our C code.

### Inline Assembly Language

Like many C compilers, `gcc` includes an extension to the standard C language that allows us to embed assembly language in our C code, *inline assembly*. Doing so can be complex. We'll look at a simple case here. You can read the details at `https://gcc.gnu.org/onlinedocs/gcc/Using-Assembly-Language-with-C.html`, or you can use the `info gcc` shell command and select **C Extensions**▶**Using Assembly Language with C**▶**Extended Asm**.

The general form for embedding assembly language in C is as follows:

```
asm asm-qualifiers (assembly language statements
                  : output operands
                  : input operands
                  : clobbers);
```

The `asm-qualifiers` are used to help the compiler optimize the C code, a topic that is beyond the scope of this book. We're not asking the compiler to optimize our C code, so we won't use `asm-qualifiers`.
