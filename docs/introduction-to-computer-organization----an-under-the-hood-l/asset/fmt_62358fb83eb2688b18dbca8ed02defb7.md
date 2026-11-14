
## Stack Discipline and Implementation

Figure 11-2: One dinner plate has been popped from the stack.

If you have guessed that it’s easy to really mess up a stack, you’re right. A stack must be used according to a strict discipline. Within any function:

*   Always push an item onto the stack before popping anything off.
*   Never pop more things off than you have pushed on.
*   Always pop everything off the stack.

If you have no use for the item(s) to be popped off, you may simply adjust the stack pointer. This is equivalent to discarding the items that are popped off. (Our dinner plate analogy breaks down here.)

A good way to maintain this discipline is to think of the use of parentheses in an algebraic expression. A push is analogous to a left parenthesis, and a pop is analogous to a right parenthesis. The pairs of parentheses can be nested, but they have to match. An attempt to push too many items onto a stack is called `stack overflow`. An attempt to pop items off the stack beyond the bottom is called `stack underflow`.

We have looked only at the essential operations on a stack here. It’s common to add other operations in an implementation of a stack. For example, a `peek` operation allows you to look at the item on the top of the stack without removing it. And as you’ll see in subsequent chapters, items not on the top of the stack are often accessed directly without pushing and popping, but in a very well-controlled way.

A stack is implemented by dedicating a contiguous area of main memory to it. Stacks can grow in either direction in memory, into higher addresses or lower. An `ascending stack` grows into higher addresses, and a `descending stack` grows into lower addresses. The stack pointer can point to the top item on the stack, a `full stack`, or to the memory location where the next item will be pushed onto the stack, an `empty stack`. These four possible stack implementations are shown in [Figure 11-3](#c11.xhtml#figure11-3) with the integers 1, 2, and 3 pushed onto the stack in that order. Be sure to notice that memory addresses are *increasing downward* in this figure, which is the way we usually view them in the `gdb` debugger.

![Figure 11-3: Four ways to implement a stack](image_fi/500099c11/f11003.png)

Figure 11-3: Four ways to implement a stack

x86-64 instructions use the stack as a `full-descending stack`. To understand this choice, think about how you might organize things in memory. Recall that the control unit automatically increments the program counter as your program is executed. Programs come in vastly different sizes, so storing the program instructions at low memory addresses allows maximum flexibility with respect to program size.

The stack is a dynamic structure. You do not know ahead of time how much stack space will be required by any given program as it executes. It’s impossible to know how much space to allocate for the stack. You would like to allocate as much space as possible, while preventing it from colliding with program instructions. The solution is to start the stack at the highest address and have it grow toward lower addresses.

This is a highly simplified rationalization for implementing stacks such that they grow “downward” in memory. The organization of various program elements in memory is much more complex than the simple description given here. But this may help you to understand that there are some good reasons for what may seem to be a rather odd implementation.

The important point is that we need to write our assembly language accordingly. We’ll next look at the details of how the stack is used in the function prologue and epilogue and how arguments to another function in registers, by writing our own “Hello, World!” program directly in assembly language.

### Inside the Function Prologue and Epilogue

My assembly language version of the “Hello, World!” program, [Listing 11-3](#c11.xhtml#listing11-3), closely follows the assembly language generated from the C version by the compiler in [Listing 11-2](#c11.xhtml#listing11-2), but I’ve added comments and used a more meaningful label for the string constant. This should make it a little easier to understand how the program uses the stack and passes arguments to the `write()` function.

```assembly
# helloWorld.s
# Hello World program using the write() system call

        .intel_syntax noprefix
# Useful constant
      1 .equ    STDOUT, 1
        
# Constant data
      2 .section  .rodata
message:
        .string "Hello, World!\n"
        .equ   3 msgLength, .-message-1

# Code
        .text
        .globl  main
        .type   main, @function
main:
        push    rbp             # save caller's frame pointer
        mov     rbp, rsp        # our frame pointer

        mov     edx, MsgLength  # message length       
        lea     rsi, message[rip]  # message address
        mov     edi, STDOUT     # the screen
        call    write@plt       # write message

        mov     eax, 0          # return 0

        pop     rbp             # restore caller frame pointer
        ret                     # back to caller
```

Listing 11-3: “Hello, World!” program written in assembly language

Before we get to a discussion of the prologue, notice that I’ve used another assembler directive, `.equ`, in [Listing 11-3](#c11.xhtml#listing11-3) 1. The format is

```
.equ symbol, expression
```

Note that we don’t need to specify the `.text` segment for the `.rodata` section 2. The assembler and linker produce an `.rodata` section, and it’s up to the operating system to determine where to load it.

The `expression` must evaluate to an integer, and the assembler sets `symbol` equal to that value. You can then use the symbol in your code, making it much easier to read, and the assembler will plug in the value of the expression. The expression is often just an integer. In this program, I have equated the symbol `STDOUT` to the integer 1.

The `.` character in an expression means *here* in memory location. Thus, when the assembler gets to the expression 3, it computes the current location in memory, which is the end of the C-style text string; subtracts the beginning location of the string, the location that the programmer labeled `message`, and then subtracts 1 for the terminating `NUL` character. The net result is that `MsgLength` is equated with the number of printable characters in the text string.

You’ve learned in Chapter 10 how the caller’s frame pointer is saved on the call stack and a new frame pointer is established for this function. But now that you know more about how the call stack works, let’s walk through the prologue of this function with `gdb`.

The first thing we need to do is to set a breakpoint at the beginning of the function:

```gdb
(gdb) b main
Breakpoint 1 at 0x1139: file helloWorld.s, line 18.
```

You can use either the label, `main`, or the line number. We saw how to use the `li` command to see the line numbers in Chapter 2. Using the line number may cause `gdb` to execute the prologue and break after it. (I’ve seen different behavior in different versions of `gdb`.)

After setting the breakpoint, when we run the program, it breaks at the first instruction, and we can inspect the contents of the `rbp` and `rsp` registers:

```gdb
(gdb) r
Starting program: /home/bob/progs/chap11/helloWorld_asm/helloWorld 

Breakpoint 1, main () at helloWorld.s:18
18             push    rbp                 # save caller's frame pointer
(gdb) i r rbp rsp
rbp            0x0                 0x0
rsp            0x7fffffffde88      0x7fffffffde88
```
