
## Listing 16-2: Header File for the `toUpper` Function

```c
/* toUpper.c
 * Converts alphabetic letters in a C string to uppercase.
 */

#include "toUpper.h"
#define UPMASK 0xdf
#define NUL '\0'

int toUpper(char *srcPtr, char *destPtr)
{
    int count = 0;
    while (*srcPtr != NUL)
    {
        *destPtr = *srcPtr & UPMASK;
        srcPtr++;
        destPtr++;
        count++;
    }
    *destPtr = *srcPtr;

    return count;
}
```

## Listing 16-3: The `toUpper` Function

To make sure that bit 5 is `0`, we use a mask that has `0` in bit position 5 and `1` elsewhere. While it’s not the `NUL` character, we perform a bitwise AND with the character, which masks out bit 5 and allows all the other bits to remain the same in the result. Don’t forget to include the `NUL` character from the input text string! Forgetting to do so is a common programming error that often does not show up during testing, because the byte in memory following where the output is being stored just happens to be `0x00` (the `NUL` character). Then if you change the length of the input text string, the next byte in memory may not be `0x00`. The error might show up in a seemingly random way.

Although this function returns a count of the number of characters processed, our `main` function does nothing with the value. A calling function doesn’t need to use a returned value. I usually include a counting algorithm in functions like this for debugging purposes if needed.

## Listing 16-4: Compiler-Generated Assembly Language for the `toUpper` Function

```asm
        .file   "toUpper.c"
        .intel_syntax noprefix
        .text
        .globl  toUpper
        .type   toUpper, @function
toUpper:
        push    rbp
        mov     rbp, rsp
        mov     QWORD PTR -24[rbp], rdi ## save srcPtr
        mov     QWORD PTR -32[rbp], rsi ## save destPtr
        mov     DWORD PTR -4[rbp], 0    ## count = 0;
        jmp     .L2
.L3:
      1 mov     rax, QWORD PTR -24[rbp] ## load srcPtr
      2 movzx   eax, BYTE PTR [rax]     ## and char there
      3 and     eax, -33                ## and with 0xdf
        mov     edx, eax
        mov     rax, QWORD PTR -32[rbp] ## load destPtr
        mov     BYTE PTR [rax], dl      ## store new char
        add     QWORD PTR -24[rbp], 1   ## srcPtr++;
        add     QWORD PTR -32[rbp], 1   ## destPtr++;
        add     DWORD PTR -4[rbp], 1    ## count++;
.L2:
      4 mov     rax, QWORD PTR -24[rbp] ## load srcPtr
      5 movzx   eax, BYTE PTR [rax]     ## and char there
      6 test    al, al                  ## NUL char?
        jne     .L3                     ## no, loop back
        mov     rax, QWORD PTR -24[rbp] ## yes, load srcPtr
        movzx   edx, BYTE PTR [rax]     ## and char there
        mov     rax, QWORD PTR -32[rbp] ## load destPtr
        mov     BYTE PTR [rax], dl      ## store NUL
        mov     eax, DWORD PTR -4[rbp]  ## return count;
        pop     rbp
        ret
        .size   toUpper, .-toUpper
        .ident  "GCC: (Ubuntu 9.3.0-17ubuntu1~20.04) 9.3.0 "
        .section        .note.GNU-stack,"",@progbits
```

Before entering the `while` loop, `toUpper` loads the address of the source text string into `rax`. The `movzx` instruction overwrites the address in `rax` with the byte stored at that address (`BYTE PTR`) and zeros the upper 56 bits of the register. The `and` instruction uses the immediate data `-33` = `0xdf`, which is the mask to turn bit 5 to `0`, thus making sure that the character is uppercase. This code sequence is repeated within the `while` loop for each character in the input text string that is not the `NUL` character.

As explained, treating the characters as bit patterns rather than as numerical values works for both converting lowercase to uppercase and letting uppercase remain unchanged.

Listing 16-4 introduces another logic instruction, `and`. We already saw the `xor` instruction in Chapter 11. Let’s look at two more logical instructions, `and` and `or`.

### Logic Instructions

*Logic instructions* work bitwise. That is, they operate on the individual bits in the corresponding bit positions of the two operands. Two of the most common logic instructions are AND and OR.

#### `and` — Logical AND

1. Performs a bitwise AND between two values.
2. `and reg1, reg2` performs bitwise AND between values in registers `reg1` and `reg2`, which can be the same or different registers. The result is left in `reg1`.
3. `and reg, mem` performs bitwise AND between a value in a register and a value in memory, leaving the result in the register.
4. `and mem, reg` performs bitwise AND between a value in memory and a value in a register, leaving the result in memory.
5. `and reg, imm` performs bitwise AND between a value in a register and the constant `imm`, leaving the result in the register.
6. `and mem, imm` performs bitwise AND between a value in memory and the constant `imm`, leaving the result in memory.

The `and` instruction performs a bitwise AND between the source and destination values, leaving the result in the destination. The `SF`, `ZF`, and `PF` flags in the `rflags` register are set according to the result, the `OF` and `CF` flags are set to `0`, and the `AF` flag is undefined.

#### `or` — Logical Inclusive OR

1. Performs a bitwise inclusive OR between two values.
2. `or reg1, reg2` performs bitwise inclusive OR between values in registers `reg1` and `reg2`, which can be the same or different registers. The result is left in `reg1`.
3. `or reg, mem` performs bitwise inclusive OR between a value in a register and a value in memory, leaving the result in the register.
4. `or mem, reg` performs bitwise inclusive OR between a value in memory and a value in a register, leaving the result in memory.
5. `or reg, imm` performs bitwise inclusive OR between a value in a register and the constant `imm`, leaving the result in the register.
6. `or mem, imm` performs bitwise inclusive OR between a value in memory and the constant `imm`, leaving the result in memory.

The `or` instruction performs a bitwise inclusive OR between the source and destination values, leaving the result in the destination. The `SF`, `ZF`, and `PF` flags in the `rflags` register are set according to the result, the `OF` and `CF` flags are set to `0`, and the `AF` flag is undefined.

### Bit Masking in Assembly Language

We’ll use the same masking algorithm in our assembly language version, but we’ll use identifiers that make it easier to see what is taking place. Listing 16-5 shows the `main` function written in assembly language.
