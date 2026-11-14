
# Converting Decimal Text Strings to Integers

Let's modify the C program in Listings [16-7](#c16.xhtml#listing16-7), [16-8](#c16.xhtml#listing16-8), and [16-9](#c16.xhtml#listing16-9) to convert numeric text strings into decimal integers. When converting from hexadecimal text strings, we shifted the accumulated value four bits to the left, thus multiplying it by 16. We'll use the same algorithm for converting decimal text strings but multiply by 10 instead of 16. Listings [16-13](#c16.xhtml#listing16-13), [16-14](#c16.xhtml#listing16-14), and [16-15](#c16.xhtml#listing16-15) show the C program.

## Listing 16-13: Main Program (`convertDec.c`)

```c
/* convertDec.c
 * Reads decimal number from keyboard and displays how
 * it's stored in hexadecimal.
 */

#include <stdio.h>
#include "writeStr.h"
#include "readLn.h"
#include "decToUInt.h"
#define MAX 20
int main()
{
  char theString[MAX];
  unsigned int theInt;
   
  writeStr("Enter an unsigned integer: ");
  readLn(theString, MAX);

  decToUInt(theString, &theInt);
  printf("\"%s\" is stored as 0x%x\n", theString, theInt);
   
  return 0;
}
```

The `main` function for this decimal conversion program is almost the same as for the hexadecimal conversion program in the previous section. The primary difference is that we display the original text string entered by the user and show how the resulting `unsigned int` is stored in hexadecimal (see callout 1 in listing).

## Listing 16-14: Header File (`decToUInt.h`)

The function to do the conversion, `decToUInt`, takes a pointer to the text string and a pointer to the variable for the primary output, and it returns the number of characters that were converted.

```c
/* decToUInt.h
 * Converts decimal character string to unsigned int.
 * Returns number of characters.
 */
 
#ifndef DECTOUINT_H
#define DECTOUINT_H
int decToUInt(char *stringPtr, unsigned int *intPtr);
#endif
```

## Listing 16-15: Implementation (`decToUInt.c`)

```c
/* decToUInt.c
 * Converts decimal character string to unsigned int.
 * Returns number of characters.
 */

#include <stdio.h>
#include "decToUInt.h"
#define INTMASK 0xf

int decToUInt(char *stringPtr, unsigned int *intPtr)
{
  int radix = 10;
  char current;
  int count = 0;
  
  *intPtr = 0;
  current = *stringPtr;
  while (current != '\0')
  {
    current = current & INTMASK;        // 1
    *intPtr = *intPtr * radix;          // 2
    *intPtr += current;                 // 3
    stringPtr++;
    count++;
    current = *stringPtr;
  }
  return count;
}
```

The first difference between this algorithm and the one for hexadecimal is that we don't need to check for alphabetic characters because the ASCII code for the numeric characters is contiguous from 0 to 9. Since the low-order four bits of the ASCII code for the numerals is the same as the integer value it represents, we can simply mask off the high-order four bits (callout 1).

As we saw, when working with hexadecimal, we can easily make room for the new value by shifting the accumulating result, but this works only when the maximum of the new value is a power of two. When working with decimal, we need to multiply the accumulating result by 10 (callout 2), and then we need to add the new value (callout 3).

## Listing 16-16: Compiler-Generated Assembly (`decToUInt.s`)

The compiler generates the assembly language for the `decToUInt` function:

```asm
        .file   "decToUInt.c"
        .intel_syntax noprefix
        .text
        .globl  decToUInt
        .type   decToUInt, @function
decToUInt:
        push    rbp
        mov     rbp, rsp
        mov     QWORD PTR -24[rbp], rdi     ## save stringPtr
        mov     QWORD PTR -32[rbp], rsi     ## save intPtr
        mov     DWORD PTR -4[rbp], 10       ## radix = 10;
        mov     DWORD PTR -8[rbp], 0        ## count = 0;
        mov     rax, QWORD PTR -32[rbp]
        mov     DWORD PTR [rax], 0          ## *intPtr = 0;
        mov     rax, QWORD PTR -24[rbp]
        movzx   eax, BYTE PTR [rax]
        mov     BYTE PTR -9[rbp], al        ## load character
        jmp     .L2                         ## go to bottom
.L3:
        and     BYTE PTR -9[rbp], 15        ## convert to int
        mov     rax, QWORD PTR -32[rbp]
        mov     edx, DWORD PTR [rax]        ## load current value
        mov     eax, DWORD PTR -4[rbp]      ## load radix
        imul    edx, eax                    ## 1: times 10
        mov     rax, QWORD PTR -32[rbp]
        mov     DWORD PTR [rax], edx        ## store 10 times current
        mov     rax, QWORD PTR -32[rbp]
        mov     edx, DWORD PTR [rax]        ## load 10 times current
        movsx   eax, BYTE PTR -9[rbp]       ## 2: byte to 32 bits
        add     edx, eax                    ## 3: add in latest value
        mov     rax, QWORD PTR -32[rbp]
        mov     DWORD PTR [rax], edx        ## *intPtr += current;
        add     QWORD PTR -24[rbp], 1       ## stringPtr++;
        add     DWORD PTR -8[rbp], 1        ## count++;
        mov     rax, QWORD PTR -24[rbp]
        movzx   eax, BYTE PTR [rax]
        mov     BYTE PTR -9[rbp], al        ## load next character
.L2:
        cmp     BYTE PTR -9[rbp], 0         ## NUL?
        jne     .L3                         ## no, keep going
        mov     eax, DWORD PTR -8[rbp]      ## yes, return count;
        pop     rbp
        ret
        .size   decToUInt, .-decToUInt
        .ident  "GCC: (Ubuntu 9.3.0-17ubuntu1~20.04) 9.3.0"
        .section        .note.GNU-stack,"",@progbits
```

The assembly language for the `decToUInt` function is similar to the assembly language for the `hexToInt` function shown in [Listing 16-12](#c16.xhtml#listing16-12). The primary differences are that the accumulated result is multiplied by the radix we're converting to, 10 for decimal, using the `imul` instruction (callout 1), and that the new value is type cast to be a 32-bit `int` (callout 2) before it's added to the accumulated result (callout 3).

The x86-64 instruction set includes both an unsigned multiply instruction, `mul`, and a signed one, `imul`. It may seem odd that the compiler is using the signed multiply instruction to convert the numeric text string to an `unsigned int`. We'll see the reason after looking at the details of the two instructions.

### Multiply Instructions

#### `imul`—Signed Multiply

The signed multiply instruction can have one, two, or three operands:

1.  Performs a signed multiply.

2.  `imul reg` multiplies the integer in `al`, `ax`, `eax`, or `rax` by the integer in `reg`, leaving the result in `ax`, `dx:ax`, `edx:eax`, or `rdx:rax`, respectively.

3.  `imul mem` multiplies the integer in `al`, `ax`, `eax`, or `rax` by the integer in `mem`, leaving the result in `ax`, `dx:ax`, `edx:eax`, or `rdx:rax`, respectively.

4.  `imul reg1, reg2` multiplies the integer in `reg1` by the integer in `reg2`, leaving the result in `reg1`. `reg1` and `reg2` can be the same register.

5.  `imul reg, mem` multiplies the integer in `reg` by the integer in `mem`, leaving the result in `reg`.

6.  `imul reg1, reg2, imm` multiplies the integer in `reg2` by the integer `imm`, leaving the result in the destination `reg1`. `reg1` and `reg2` can be the same register.

7.  `imul reg, mem, imm` multiplies the integer in `mem` by the integer `imm`, leaving the result in `reg`.

    The width of the integers in registers or in memory must be the same. In the first form, the width of the result will be twice that of the integers being multiplied and will be sign-extended in the high-order portion. In the second and third forms, the *n*-bit destination is multiplied by the *n*-bit source and *n* low-order bits left in the destination. In the last two forms –128 ≤ `imm` ≤ +127, which is sign-extended to the same width as the source and destination registers before multiplying it by the *n*-bit source, leaving the *n* low-order bits in the destination register.
