
# Fixed-Point Arithmetic Examples

## Listing 19-5: `displayMoney` Function

```asm
# displayMoney.s
# Displays money in dollars and cents.
# Calling sequence:
#   `edi` <- money in cents
        .intel_syntax noprefix
# Useful constant
        .equ  1 cent2dollars, 100
# Stack frame
        .equ    money,-16
        .equ    localSize,-16
# Constant data
        .section	.rodata
        .align 8
decimal:
        .string "."
msg:
        .string "Total = $"
zero:
        .string "0"
endl:
        .string "\n"
# Code
        .text
        .globl  displayMoney
        .type   displayMoney, @function
displayMoney:
        push    rbp                     # save frame pointer
        mov     rbp, rsp                # set new frame pointer
        add     rsp, localSize          # for local var.
        
        mov     money[rbp], rdi         # save input money
        lea     rdi, msg[rip]           # nice message
        call    writeStr
        
        mov     edx, 0                  # clear high order
        mov     eax, money[rbp]         # convert money amount
        mov     ecx, cent2dollars       #     to dollars and cents
      2 div     ecx
      3 mov     money[rbp], edx         # save cents
        mov     edi, eax                # dollars
        call    putUInt                 # write to screen
      4 lea     rdi, decimal[rip]
        call    writeStr
        
        cmp     dword ptr money[rbp], 10  # 2 decimal places?
        jae     twoDecimal                # yes
        lea     rdi, zero[rip]            # no, 0 in tenths place
        call    writeStr
twoDecimal:
        mov     edi, money[rbp]           # load cents
        call    putUInt                   # write to screen

        lea     rdi, endl[rip]
        call    writeStr

        mov     rsp, rbp                  # restore stack pointer
        pop     rbp                       # and caller frame pointer
        ret
```

**Listing 19-5:** Function that displays cents as dollars and cents

We used a scaling factor to move the decimal point two places to the right when reading the money values. Now we need to move the decimal point two places to the left to recover the fractional part. So we need to use the same scaling factor, 100.

Then the `div` instruction will leave the integral part in `eax` and the remainder (the fractional part) in `edx`. We’ll temporarily save the fractional part while we print the integral part. As with the ruler measurement program, we print text to indicate the fractional part.

[Listing 19-6](#c19.xhtml#listing19-6) shows the `main` function that adds two money amounts.

## Listing 19-6: `moneyAdd` Program

```asm
# moneyAdd.s
# Adds two money amounts in dollars and cents.
        .intel_syntax noprefix
# Stack frame
        .equ    x,-16
        .equ    y, -12
        .equ    canary,-8
        .equ    localSize,-16
# Constant data
        .section	.rodata
        .align 8
endl:
        .string "\n"
# Code
        .text
        .globl	main
        .type	main, @function
main:
        push    rbp                     # save frame pointer
        mov     rbp, rsp                # set new frame pointer
        add     rsp, localSize          # for local var.
        mov     rax, qword ptr fs:40    # get canary
        mov     qword ptr canary[rbp], rax

        lea     rdi, x[rbp]             # x amount
        call    getMoney
        
        lea     rdi, y[rbp]             # y amount
        call    getMoney

        mov     edi, x[rbp]             # retrieve x amount
      1 add     edi, y[rbp]             # add y amount
        call    displayMoney

        mov     eax, 0                  # return 0;
        mov     rcx, qword ptr canary[rbp]
        xor     rcx, qword ptr fs:40
        je      goodCanary
        call    __stack_chk_fail@plt
goodCanary:
        mov     rsp, rbp                # restore stack pointer
        pop     rbp                     # and caller frame pointer
        ret
```

**Listing 19-6:** Program to add money amounts using fixed-point numbers

The money amounts we’re using in the `main` function have been scaled to be integers, so we can use a simple `add` instruction to add them.

Although fixed-point arithmetic allows us to preserve the full resolution of the numbers, the range of values is limited by the number of bits in the integral data type, 32 in this program. We’ve limited the range of the numbers in our program to be 0 to 42949672.95 but increased resolution to the nearest 0.01.

We have a more convenient notation for representing very large and very small numbers, which we’ll explore next.

# Floating-Point Numbers

Let’s begin with the most important concept in this section: `floating-point numbers` are not `real numbers`. Real numbers include the continuum of all numbers from −∞ to +∞. You already know that computers are finite, so there is certainly a limit on the largest values that can be represented, but the problem is worse than simply a limit on the magnitude.

As you will see in this section, floating-point numbers comprise a small subset of real numbers. There are significant gaps between adjacent floating-point numbers. These gaps can produce several types of errors. To make matters worse, these errors can occur in intermediate results, where they are difficult to debug.

## Floating-Point Representation

`Floating-point representation` is based on `scientific notation`. In floating-point representation we have a sign and two numbers to completely specify a value: a `significand` and an `exponent`. A decimal floating-point number is written as a significand times 10 raised to an exponent. For example, consider these two numbers:

![c19m0005](image_fi/500099c19/c19m0005.png)

`0.0010123 = 1.0123 × 10^(–3)`  
`–456.78 = –4.5678 × 10^(2)`

Notice that in floating-point representation, the number is *normalized* such that only one digit appears to the left of the decimal point. The exponent of 10 is adjusted accordingly. If we agree that each number is normalized and that we are working in base 10, then each floating-point number is completely specified by three items: significand, exponent, and sign. In the previous two examples:

`10123, –3, and +` represent `+1.0123 × 10^(–3)`  
`45678, +2, and –` represent `–4.5678 × 10^(+2)`

The advantage of using floating-point representation is that, for a given number of digits, we can represent a larger range of values.

Let’s look at how floating-point numbers are stored in a computer.

## IEEE 754 Floating-Point Standard

The most commonly used standard for storing floating-point numbers is `IEEE 754` ([https://standards.ieee.org/standard/754-2019.html](https://standards.ieee.org/standard/754-2019.html)). [Figure 19-1](#c19.xhtml#figure19-1) shows the general pattern.

![Figure 19-1: General pattern for storing floating-point numbers](image_fi/500099c19/f19001_replacement.png)

**Figure 19-1:** General pattern for storing floating-point numbers

Here, `S` is the sign of the number.

Like all storage formats, floating-point formats involve trade-offs between resolution, rounding errors, size, and range. The `IEEE 754` standard specifies sizes from 4 to 16 bytes. The most common sizes used in C/C++ are `float` (four bytes) and `double` (eight bytes). The `x86-64` architecture supports both sizes plus a 10-byte extended version that is similar to, but not part of, the `IEEE 754` standard.

[Figure 19-2](#c19.xhtml#figure19-2) shows the number of bits specified for each of these three sizes.

![Figure 19-2: Number of bits for (a) C float, (b) C double, and (c) x86-64 extended version](image_fi/500099c19/f19002.png)

**Figure 19-2:** Number of bits for (a) C `float`, (b) C `double`, and (c) `x86-64` extended version
