
# Records in C and Assembly Language

Let's start by looking at a program that defines a record, stores data in the fields of each record, and then displays the values, as shown in Listing 17-11.

```c
/* recordField.c
 * Allocates a record and assigns a value to each field.
 */

#include <stdio.h>

int main(void)
{
    struct
    {
        char aChar;
        int anInt;
        char anotherChar;
    } x;

    x.aChar = 'a';
    x.anInt = 123;
    x.anotherChar = 'b';

    printf("x: %c, %i, %c\n",
           x.aChar, x.anInt, x.anotherChar);
    return 0;
}
```

**Listing 17-11:** A single record using a `struct` variable in C

We use the `struct` keyword to declare a record in C. We define the fields of the record by using the usual C syntax: a data type followed by the field name. We can define a record by following its declaration with a name for the record. We access the individual fields of a record by using the dot operator.

We can learn how the record is stored in memory by looking at the compiler-generated assembly language for this function in Listing 17-12.

```asm
        .file   "recordField.c"
        .intel_syntax noprefix
        .text
        .section        .rodata
.LC0:
        .string "x: %c, %i, %c\n"
        .text
        .globl  main
        .type   main, @function
main:
        push    rbp
        mov     rbp, rsp
        sub     rsp, 16                 ## memory for record
        mov     BYTE PTR 1-12[rbp], 97  ## x.aChar = 'a';
        mov     DWORD PTR -8[rbp], 123  ## x.anInt = 123;
        mov     BYTE PTR -4[rbp], 98    ## x.anotherChar = 'b';
        movzx   eax, BYTE PTR -4[rbp]   ## load x.anotherChar
        movsx   ecx, al                 ## to 32 bits
        mov     edx, DWORD PTR -8[rbp]  ## load x.anInt
        movzx   eax, BYTE PTR -12[rbp]  ## load x.aChar
        movsx   eax, al                 ## to 32 bits
        mov     esi, eax
        lea     rdi, .LC0[rip]
        mov     eax, 0
        call    printf@PLT
        mov     eax, 0
        leave
        ret
        .size   main, .-main
        .ident  "GCC: (Ubuntu 9.3.0-17ubuntu1~20.04) 9.3.0"
        .section        .note.GNU-stack,"",@progbits
```

**Listing 17-12:** Accessing the fields of a C `struct` variable in memory

Like other local variables, the record is allocated in the function's stack frame, so its fields are accessed relative to the stack frame pointer, `rbp`. The compiler computes the offset from the address in `rbp` to each field in the record.

We show the layout in memory for the record in Figure 17-1, which shows the record after the values have been assigned to the three fields.

![Figure 17-1: Stack frame showing one record implemented as a C struct](image_fi/500099c17/f17001.png)

**Figure 17-1:** Stack frame showing one record implemented as a C `struct`

An `int` in our environment takes four bytes and must be aligned on a four-byte memory address. Although both the two `char` fields in this record take only one byte, the alignment requirement for the `int` field leads to the six "wasted" bytes, as shown in Figure 17-1. There are also four extra bytes in the stack frame to create proper stack pointer alignment, as described in "Variables on the Stack" in Chapter 11.

## Records in Assembly Language

Rather than compute the offset from `rbp`, we'll use a different technique in our assembly language version of accessing the fields of a record, as shown in Listing 17-13.

```asm
# recordField.s
# Allocates a record and assigns a value to each field.
        .intel_syntax noprefix
# Stack frame
        .equ    x,-12
        .equ    localSize,-16
# record offsets
        .equ    aChar,0
        .equ    anInt,4
        .equ    anotherChar,8
        .equ    recordSize,12
# Constant data
        .section  .rodata
        .align  8
message:
        .string "x: %c, %i, %c\n"
# Code
        .text
        .globl	main
        .type	  main, @function
main:
        push    rbp                 # save frame pointer
        mov     rbp, rsp            # set new frame pointer
        add     rsp, localSize      # for local var.

        lea     rax, x[rbp]         # fill record
        mov     byte ptr aChar[rax], 'a'
        mov     dword ptr anInt[rax], 123
        mov     byte ptr anotherChar[rax], 'c'
        
        lea     rax, x[rbp]         # print record
        movzx   ecx, byte ptr anotherChar[rax]
        mov     edx, dword ptr anInt[rax]
        movzx   esi, byte ptr aChar[rax]
        lea     rdi, message[rip]
        mov     eax, 0
        call    printf@plt

        mov     eax, 0              # return 0;
        mov     rsp, rbp            # restore stack pointer
        pop     rbp                 # and caller frame pointer
        ret
```

**Listing 17-13:** Accessing record fields in assembly language.

In our assembly language version, we first equate the field names with their respective offsets from the beginning of the record. The diagram in Figure 17-1 is useful in coming up with these numbers. Then we can load the address of the beginning of the record and directly access the fields using their names.

Passing a record to another function raises additional issues. As you have seen, we need to specify the type of data that we are passing, but a record can have many fields, each of which can have a different data type. In the next section, we'll see how C solves this problem.

## Passing Records to Other Functions in C

Defining the fields every time we define another instance of a record is cumbersome. C allows us to define our own `struct` types using a *structure tag* (or simply *tag*), which serves as a synonym for the field definitions. Not only is this useful for defining multiple records of the same field composition, but it's necessary for passing records to other functions.

For example, we defined the `struct` variable `x` in Listing 17-11:

```c
struct
{
    char aChar;
    int anInt;
    char anotherChar;
} x;
```

Instead, if we create a tag for the fields in the `struct` like this,

```c
struct aTag
{
    char aChar;
    int anInt;
    char anotherChar;
};
```

then we have created a new programmer-defined data type, `struct aTag`. We can then define variables of our new data type in the usual way:

```c
struct aTag x;
```

We'll start by declaring our new `struct` data type in a separate header file, as shown in Listing 17-14, so that it can be included in each file where it is used.

```c
/* aRecord.h
 * Declaration of a record.
 */

#ifndef ARECORD_H
#define ARECORD_H
struct aTag
{
    char aChar;
    int anInt;
    char anotherChar;
};
#endif
```

**Listing 17-14:** Declaration of a programmer-defined record data type in C

The tag for a record in C is a programmer-defined identifier placed immediately after the `struct` keyword in the record's declaration. We can then use `aTag` to represent the record fields declared in Listing 17-14. Listing 17-15 shows how we can use the tag to define two records in a program.

```c
/* records.c
 * Allocates two records, assigns a value to each field
 * in each record, and displays the contents.
 */

#include "aRecord.h"
#include "loadRecord.h"
#include "displayRecord.h"

int main(void)
{
    struct aTag x;
    struct aTag y;

    loadRecord(&x, 'a', 123, 'b');
    loadRecord(&y, '1', 456, '2');
  
    displayRecord(x);
    displayRecord(y);
  
    return 0;
}
```

**Listing 17-15:** Program to load data into two records and then display the contents
