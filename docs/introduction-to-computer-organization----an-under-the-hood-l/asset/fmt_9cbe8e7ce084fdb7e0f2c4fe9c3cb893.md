
I’ve used the `register` storage class modifier 1 to request that the compiler use a CPU register for the `feet` and `inchesRem` variables. The `register` modifier is advisory only. The C language standard doesn’t require the compiler to honor the request. But notice that I didn’t request the compiler use a CPU register for the `inches` variable 2. The `inches` variable must be placed in memory since `scanf` needs a pointer to the location of `inches` 3 to store the value read from the keyboard.

You’ve already seen some `gdb` commands earlier in the book (review Chapter 2 for a refresher). When you hit a breakpoint in a program that has been running, here are some additional commands that you may find useful for moving through the program under your control and viewing information about the program:

1. `n` (next) executes the current source code statement; if it’s a call to a function, the entire function is executed.
2. `s` (step) executes the current source code statement; if it’s a call to a function, step into the function, arriving at the first instruction of the called function.
3. `si` (step instruction) executes the current machine instruction; if it’s a call to a function, step into the function.
4. `i r` (info registers) displays the contents of the registers, except the floating-point and vector registers.

Here’s how I used `gdb` to control the execution of the program and observe the register contents. Note that you’ll probably see different addresses if you replicate this example on your own, which you’re asked to do when it’s Your Turn.

```
1 $ gcc -g -O0 -Wall -masm=intel -o inches2feet inches2feet.c
2 $ gdb ./inches2feet 
GNU gdb (Ubuntu 9.2-0ubuntu1~20.04) 9.2
  --snip--
Reading symbols from ./inches2feet...done.
3 (gdb) l
1       /* inches2feet.c
2       * Converts inches to feet and inches.
3       */
4      
5      #include <stdio.h>
6      #define inchesPerFoot 12
7      
8      int main(void)
9      {
10       register int feet;
(gdb) ENTER
11       register int inchesRem;
12       int inches;
13       int *ptr;
14     
15       ptr = &inches;
16     
17       printf("Enter inches: ");
18       scanf("%i", ptr);
19       
20       feet = inches / inchesPerFoot;
(gdb) ENTER
21       inchesRem = inches % inchesPerFoot;
22       printf("%i\" = %i' %i\"\n", inches, feet, inchesRem);
23     
24       return 0;
25     }
```

We first compile the program 1, load it with `gdb` 2, and then list the source code so we can see where to set breakpoints. Using the enter key 3 (return on some keyboards) repeats the previous command. The debugger starts by printing information about itself, which I have cut out of my display to save space.

We want to follow along as the program processes the data by setting breakpoints at strategic points in the program:

```
(gdb) b 17
Breakpoint 1 at 0x11af: file inches2feet.c, line 17.
(gdb) b 20
Breakpoint 2 at 0x11d8: file inches2feet.c, line 20.
```

We set the first breakpoint where the program is about to prompt the user to enter the input data, line 17, and a second at the statement where the program’s computations begin, line 20.

When we run the program, it breaks at the first breakpoint it encounters:

```
(gdb) r
Starting program: /home/progs/chapter_09/inches2feet/inches2feet  

Breakpoint 1, main () at inches2feet.c:17
17       printf("Enter inches: ");
```

The program stops at line 17 of the source code, and control returns to `gdb`. We can see the contents of the registers with the `i r` command. (Be sure to type a space between `i` and `r`.)

```
(gdb) i r
rax            0x7fffffffdeac      140737488346796
rbx            0x555555555260      93824992236128
rcx            0x555555555260      93824992236128
rdx            0x7fffffffdfd8      140737488347096
rsi            0x7fffffffdfc8      140737488347080
rdi            0x1                 1
rbp            0x7fffffffded0      0x7fffffffded0
rsp            0x7fffffffdea0      0x7fffffffdea0
r8             0x0                 0
r9             0x7ffff7fe0d50      140737354009936
r10            0x7                 7
r11            0x2                 2
r12            0x5555555550a0      93824992235680
r13            0x7fffffffdfc0      140737488347072
r14            0x0                 0
r15            0x0                 0
rip            0x5555555551af      0x5555555551af <main+38>
eflags         0x246 [ PF ZF IF ]
cs             0x33  51
ss             0x2b  43
ds             0x0   0
es             0x0   0
fs             0x0   0
gs             0x0   0
```

This display tells us the contents of the registers before the user enters data (you’ll see different numbers). We might want to know if the compiler honored our request to use registers for the `feet` and `inchesRem` variables. And if it did, which registers did it use?

We’d like to know this information so we can look at the contents of the registers before and after they’re used by the program so we can see if the program is storing the correct values in them. We can answer this question by asking `gdb` to print the addresses of these two variables:

```
(gdb) print &feet
Address requested for identifier "feet" which is in register $r12
(gdb) print &inchesRem
Address requested for identifier "inchesRem" which is in register $rbx
```

When we ask for the address of a variable, `gdb` will give the memory address associated with a programmer-supplied identifier. But in this program we asked the compiler to use registers, and `gdb` tells us which register the compiler chose for each variable.

We didn’t ask the compiler to use registers for the `inches` and `ptr` variables, so `gdb` should tell us where they are located in memory:

```
(gdb) print &inches
$1 = (int *) 0x7fffffffdeac
(gdb) print &ptr
$2 = (int **) 0x7fffffffdeb0
```

Now that we know `r12` is being used for `feet` and `rbx` for `inchesRem`, we can see what’s currently stored in these two registers and continue running the program:

```
1 (gdb) i r rbx r12
rbx            0x555555555260      93824992236128
r12            0x5555555550a0      93824992235680
2 (gdb) c
Continuing.
Enter inches: 123 3
```

Rather than display all the registers, we can specify the two we want to look at 1. Continuing program’s execution 2, the program asks the user to enter the number of inches, and I responded with `123` 3. It then breaks back into `gdb` at the next breakpoint it encounters:

```
Breakpoint 2, main () at inches2feet.c:20
20       feet = inches / inchesPerFoot;
```

Before starting the computations, let’s make sure the user’s input is stored in the right place:

```
(gdb) print inches
$3 = 123
```

The program is about to compute the number of feet, and then it will compute the remainder of inches. So I’ll execute two statements using `n`:

```
(gdb) n
21       inchesRem = inches % inchesPerFoot;
(gdb) ENTER 
22       printf("%i\" = %i' %i\"\n", inches, feet, inchesRem);
```

The program is now ready to print out the results of the computations. I’ll check to make sure all the computations were performed correctly and the results are in the proper variables, which we’ve already determined are in `rbx` and `r12`:

```
(gdb) i r rbx r12
rbx            0x3   3
r12            0xa   10
```

There are other ways to see what’s stored in `feet` and `inchesRem`:

```
(gdb) print $rbx
$4 = 3
(gdb) print $r12
$5 = 10
(gdb) print feet
$6 = 10
(gdb) print inchesRem
$7 = 3
```

When using `gdb`’s `print` command, you can print only one variable at a time, even if a register is being used to store the variable. The `$` prefix on the register name isn’t required for the `i r` command, but it is for the `print` command.

Before completing execution of the program, I’ll take a final look at all the registers:
