
# While Loop Implementation in Assembly

The `while` statement first checks to see if the pointer variable, `stringPtr`, is pointing to the `NUL` character. If it is not the `NUL` character, program flow enters the `while` loop body and writes the character pointed to by `stringPtr` on the screen. The pointer variable is then incremented to point to the next character in the text string. Program flow returns to the top of the loop where this next character is checked to see if it is the `NUL` character. This loop terminates when the character pointer, `stringPtr`, is pointing to the `NUL` character. By testing for this condition first, we would not even enter the body of the `while` loop if `stringPtr` were to point to an empty string, because there would be nothing to do.

The compiler generated the assembly language shown in [Listing 13-2](#c13.xhtml#listing13-2) for this program.

## Compiler-Generated Assembly (Listing 13-2)

```
.file   "helloWorld.c"
.intel_syntax noprefix
.text
.section        .rodata
.LC0:
        .string "Hello, World!\n"
.text
.globl  main
.type   main, @function
main:
        push    rbp
        mov     rbp, rsp
        sub     rsp, 16
        lea     rax, .LC0[rip]          ## address of text string
        mov     QWORD PTR -8[rbp], rax
      1 jmp     .L2                     ## jump to bottom
2 .L3:
        mov     rax, QWORD PTR -8[rbp]  ## address of current character
        mov     edx, 1                  ## one character
        mov     rsi, rax                ## pass address
        mov     edi, 1                  ## STDOUT_FILENO
        call    write@PLT
        add     QWORD PTR -8[rbp], 1    ## increment pointer
.L2:
        mov     rax, QWORD PTR -8[rbp]  ## address of current character
        movzx   eax, BYTE PTR [rax]     ## load character
      3 test    al, al                  ## NUL character?
        jne     .L3                     ## no, continue looping
        mov     eax, 0                  ## yes, all done
        leave
        ret
.size   main, .-main
.ident  "GCC: (Ubuntu 9.3.0-17ubuntu1~20.04) 9.3.0"
.section        .note.GNU-stack,"",@progbits
```

Listing 13-2: Assembly language for the C program in [Listing 13-1](#c13.xhtml#listing13-1)

Although this assembly language seems to be testing for the termination condition at the end of the loop, it follows the logical flow shown in [Figure 13-1](#c13.xhtml#figure13-1). It jumps down to `.L2` and tests for the terminating condition before jumping up to `.L3` to start execution of the body of the `while` loop.

This code introduces two more instructions:

## The `movzx` Instruction

`movzx`—Move with zero-extend

Copies (moves) a source value from memory or a register to a wider register and zero-extends the value in the destination register.

`movzx reg, reg` moves from a register to a register.

`movzx reg, mem` moves from a memory location to a register.

An 8-bit source value can be extended to 16, 32, or 64 bits, and a 16-bit value can be extended to 32 or 64 bits. The additional high-order bits in the destination are all `0`. If the extension is to 32 bits, the high-order 32-bit portion of the destination register is also all `0`. Recall (from Chapter 9) that a `mov` of 32 bits into a register will zero the high-order 32 bits of the register, thus zero-extending 32 bits to 64 bits. The `movzx` instruction does not affect the status flags in the `rflags` register.

## The `test` Instruction

`test`—Test bits

Performs a bitwise AND between the source and destination operands, without changing either, and sets status flags accordingly.

`test reg, reg` tests between two registers.

`test mem, reg` tests between a register and a memory location.

`test reg, imm` tests between an explicit number and a register.

`test mem, imm` tests between an explicit number and a memory location.

The `test` instruction sets the status flags in the `rflags` register to show the result of AND-ing the source and destination operands. Neither operand is changed.

## Hand-Written Assembly (Listing 13-3)

In my assembly language version of this program, [Listing 13-3](#c13.xhtml#listing13-3), I have organized the code so that the conditional test is at the top of the `while` loop.

```
# helloWorld.s
# Hello World program using the write() system call
# one character at a time.

.intel_syntax noprefix
# Useful constants
.equ    STDOUT,1
# Stack frame
.equ    aString,-8
.equ    localSize,-16
# Read only data
.section  .rodata
theString:
        .string "Hello, World!\n"
# Code
.text
.globl  main
.type   main, @function
main:
        push    rbp                 # save frame pointer
        mov     rbp, rsp            # set new frame pointer
        add     rsp, localSize      # for local var.

        lea     rax, theString[rip]
        mov     aString[rbp], rax   # *aString = "Hello World.\n";
whileLoop:
        mov     rsi, aString[rbp]   # current char in string
      1 cmp     byte ptr [rsi], 0   # null character?
      2 je      allDone             # yes, all done

        mov     edx, 1              # one character
        mov     edi, STDOUT         # to standard out
        call    write@plt
        
      3 inc     qword ptr aString[rbp]  # aString++;
      4 jmp     whileLoop           # back to top
allDone:
        mov     eax, 0              # return 0;
        mov     rsp, rbp            # restore stack pointer
        pop     rbp                 # and caller frame pointer
        ret
```

Listing 13-3: Writing one character at a time to the screen, in assembly language

I have also used two more new instructions, `cmp` and `inc`, which are detailed next.

## The `cmp` Instruction

`cmp`—Compare

Compares first operand with second operand and, without changing either, sets the status flags accordingly.

`cmp reg, reg` compares the value in a first register with the value in a second register.

`cmp mem, reg` compares the value in memory with the value in a register.

`cmp reg, imm` compares the value in a register with the explicit number.

`cmp mem, imm` compares the value in memory with the explicit number.

The `cmp` instruction sets the status flags in the `rflags` register to show the result of subtracting the first operand from the second operand. Neither operand is changed.

## The `inc` Instruction

`inc`—Increment

Adds 1 to a variable.

`inc reg` adds 1 to the value in a register.

`inc mem` adds 1 to the value in memory.

The `inc` instruction does set the status flags in the `rflags` register according to the result.

It might seem that my assembly language solution is less efficient than what the compiler generated ([Listing 13-2](#c13.xhtml#listing13-2)) because the `jmp` instruction is executed in addition to the conditional `je` instruction with each iteration of the loop. Most modern CPUs use a technique known as *branch prediction*, in which they assume that a conditional jump will always go one way. We will not go into the details in this book, but the technique greatly speeds up the execution of the conditional jump instruction when the jump is not taken.

A `while` loop works well when a *sentinel value*, a unique value that marks the end of a data sequence, is used as the termination condition. For example, the `while` loop in [Listing 13-1](#c13.xhtml#listing13-1) works for any length of text string. The loop continues writing one character at a time on the screen until it reaches the sentinel value, a `NUL` character. C has another looping construct, the `for` loop, that many programmers find to be more natural in some algorithms.

## for Loop

Although their C syntax differs, the two looping constructs, `while` and `for`, are semantically equivalent. The syntactical difference is that the `for` loop allows you to group all three control elements—loop control variable initialization, checking, and changing—within the parentheses. The general form of a `for` loop is as follows:
