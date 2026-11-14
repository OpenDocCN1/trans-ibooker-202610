
# Stack Frame Examination with GDB

The `i r` command gives us the current location of the stack pointer, `rsp`. The instruction about to be executed will push the eight bytes in the `rbp` register onto the call stack. To see the effects in memory, we'll examine the current contents of the stack. Since the call stack is full descending, we'll subtract 8 from the current address in the stack pointer for our display so we can get a view of the area of memory that this instruction will change before it's changed:

```
(gdb) x/2xg 0x7fffffffde80
0x7fffffffde80: 0x0000555555555160      0x00007ffff7de70b3
```

The stack pointer is currently pointing to the value `0x00007ffff7de70b3`, which is the return address that the `call` instruction in the calling function (in the C runtime environment, since this is the `main` function) pushed onto the stack. The `rbp` register contains `0x0000000000000000`. This value is about to be pushed onto the stack at location `0x7fffffffde80`, which currently contains `0x0000555555555160`.

Next, we execute the two instructions in the function prologue, which will take us to the first instruction after the prologue:

```
(gdb) si
19             mov     rbp, rsp            # our frame pointer
(gdb) si
21             mov     edx, MsgLength      # message length
```

We'll inspect the values in the `rsp` and `rbp` registers:

```
(gdb) i r rbp rsp
rbp            0x7fffffffde80      0x7fffffffde80
rsp            0x7fffffffde80      0x7fffffffde80
```

We can see that the stack pointer has been decremented by 8, and the frame pointer has been set to the top of the stack. Let's look at how the stack has changed by examining the same memory area that we examined earlier:

```
(gdb) x/2xg 0x7fffffffde80
0x7fffffffde80: 0x0000000000000000      0x00007ffff7de70b3
```

We see that the value in the `rbp` register, `0x0000000000000000`, has been nicely saved at the top of the call stack. Next, we'll set a breakpoint at the `call write@PLT` instruction so we can make sure that the registers have been set up correctly for `write`:

```
(gdb) b 24
Breakpoint 2 at 0x55555555514e: file helloWorld.s, line 24.
(gdb) c
Continuing.

Breakpoint 2, main () at helloWorld.s:24
24             call    write@plt           # write message
(gdb) i r rdx rsi rdi
rdx            0xe                 14
rsi            0x555555556004      93824992239620
rdi            0x1                 1
```

The `rdx` register contains the number of characters to be written on the screen, and the `rsi` register contains the address of the first character. Recall that a C-style text string is terminated with a `NUL` character, so we'll examine 15 characters at this address:

```
(gdb) x/15c 0x555555556004
0x555555556004: 72 'H'  101 'e' 108 'l' 108 'l' 111 'o' 44 ','  32 ' '  87 'W'
0x55555555600c: 111 'o' 114 'r' 108 'l' 100 'd' 33 '!'  10 '\n' 0 '\000'
```

Next, we'll set a breakpoint at the `ret` instruction to make sure that the stack pointer and frame pointer have been restored to the caller's values:

```
(gdb) b 29
Breakpoint 3 at 0x555555555159: file helloWorld.s, line 29.
(gdb) c
Continuing.
Hello, World!

Breakpoint 3, main () at helloWorld.s:29
29             ret                         # back to caller
(gdb) i r rbp rsp rip
rbp            0x0                 0x0
rsp            0x7fffffffde88      0x7fffffffde88
rip            0x555555555159      0x555555555159 <main+32>
```

I've included the `rip` register in this display to show the effects of the `ret` instruction. Executing the `ret` instruction shows that it pops the value from the top of the stack into the `rip` register, thus returning to the C runtime environment:

```
(gdb) si
__libc_start_main (main=0x555555555139 <main>, argc=1, argv=0x7fffffffdf78,
    init=<optimized out>, fini=<optimized out>, rtld_fini=<optimized out>,
    stack_end=0x7fffffffdf68) at ../csu/libc-start.c:342
342     ../csu/libc-start.c: No such file or directory.
(gdb) i r rbp rsp rip
rbp            0x0                 0x0
rsp            0x7fffffffde90      0x7fffffffde90
rip            0x7ffff7de70b3      0x7ffff7de70b3 <__libc_start_main+243>
```

Looking back at the displays of the stack shown, we can see that the address that was pushed onto the stack by the function in the C runtime environment that called our `main` function has been popped back into the `rip` register.

The protocol that specifies the interaction between functions needs to be followed very precisely, or the program will usually crash.

In the next section, we'll look at how we can create local variables on the stack. You'll see the importance of the frame pointer.

# Local Variables in a Function

Variables that are defined in a C function can be used in the function only where they're defined, making them *local variables*. They are created when the function is called and deleted when the function returns to the calling function, so they are also called *automatic variables*.

You learned in Chapter 9 that CPU registers can be used as variables, but if we were to use CPU registers to hold all of our variables, we'd soon run out of registers in even a small program, so we need to allocate space in memory for variables.

We also saw earlier that a function needs to preserve the contents of some registers (the Save? column in [Table 11-2](#c11.xhtml#table11-2)) for the calling function. If we want to use such a register in our function, we need to save its content in memory and restore it before returning to the calling function.

We'll next look at how to use the call stack for these two purposes: creating and removing automatic variables and saving and restoring register content.

## Variables on the Stack

From the description of the call stack shown previously, you might guess that it's a good place for saving a register's content—simply push it onto the stack before using the register for something else and then pop the content off into the register before returning to the calling function.

Creating variables on the call stack is more complicated. If we restrict our usage of the stack to pushing and popping, keeping track of where each variable is located on the stack would quickly become messy, if not impossible.

There is, however, an easy way to use the stack for variables. As part of the function prologue, we'll allocate enough memory for the variables on the stack by moving the stack pointer, thus increasing the size of the stack frame for the function. We can use the same addressing technique to access our variables in the stack frame that was used to access the message address in [Listing 11-3](#c11.xhtml#listing11-3), except we'll use the frame pointer, `rbp`, for the address base. We need to be careful not to change `rbp` so we can use it as a reference point in the stack frame, leaving the stack pointer free to push and pop items as needed.

To illustrate how to use the stack frame for automatic local variables, we'll start with the C program in [Listing 11-4](#c11.xhtml#listing11-4), which reads one character from the keyboard and echoes it on the screen.

```c
/* echoChar.c
 * Echoes a character entered by the user.
 */

#include <unistd.h>

int main(void)
{
  char aLetter;

  write(STDOUT_FILENO, "Enter one character: ", 21); /* prompt user   */
  read(STDIN_FILENO, &aLetter, 1);                   /* one character */
  write(STDOUT_FILENO, "You entered: ", 13);         /* message       */
  write(STDOUT_FILENO, &aLetter, 1);

  return 0;
}
```

**Listing 11-4:** Program to echo a single character entered by a user

[Listing 11-5](#c11.xhtml#listing11-5) shows the way our compiler does this, which is the assembly language that `gcc` generates for the C program in [Listing 11-4](#c11.xhtml#listing11-4).
