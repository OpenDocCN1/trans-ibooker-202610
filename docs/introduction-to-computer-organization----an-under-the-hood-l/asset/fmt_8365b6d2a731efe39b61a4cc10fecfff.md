
In this case, the three numbers are stored exactly:

`x = 1.10011001100110011001100 × 2^(20)`

`y = 1.10011001100110011001010 × 2^(20)`

`z = 1.00000000000000000000000 × 2^(–2)`

The subtraction has still caused the high-order 20 bits of `x` and `y` to cancel and left only three bits of significance for `z`, but `z` is correct.

Catastrophic cancellation occurs when at least one of the floating-point numbers has a rounding error that causes an error in the difference. If both numbers are stored exactly, we get benign cancellation. Both types of cancellation cause a loss of significance in the result.

## Associativity

Probably the most insidious effects of floating-point errors are those that occur in intermediate results. They can show up in some sets of data but not in others. Errors in intermediate results even cause floating-point addition not to be associative: there are some values of the `float` `x`, `y`, and `z` for which `(x + y) + z` is not equal to `x + (y + z)`.

Let's write a simple C program to test for associativity, as shown in [Listing 19-8](#c19.xhtml#listing19-8).

```c
/* threeFloats.c
 * Associativity of floats.
 */

#include <stdio.h>

int main()
{
  float x, y, z, sum1, sum2;

  printf("Enter a number: ");
  scanf("%f", &x);
  printf("Enter a number: ");
  scanf("%f", &y);
  printf("Enter a number: ");
  scanf("%f", &z);
  
  sum1 = x + y;
  sum1 += z;      /* sum1 = (x + y) + z */
  sum2 = y + z;
  sum2 += x;      /* sum2 = x + (y + z) */

  if (sum1 == sum2)
    printf("%f is the same as %f\n", sum1, sum2);
  else
    printf("%f is not the same as %f\n", sum1, sum2);

  return 0;
}
```

Listing 19-8: Program to show that floating-point arithmetic is not associative

Most programmers would do the addition in one statement, `sum1 = (x + y) + z`, but doing it in separate stages will allow us to look at the intermediate results in the debugger. We'll start with some simple numbers:

```bash
$ ./threeFloats 
Enter a number: 1.0
Enter a number: 2.0
Enter a number: 3.0
6.000000 is the same as 6.000000
```

The result seems reasonable. Let's try some slightly more interesting numbers:

```bash
$ ./threeFloats 
Enter a number: 1.1
Enter a number: 1.2
Enter a number: 1.3
3.600000 is not the same as 3.600000
```

We'll use `gdb` to see if we can figure out what's going on here:

```bash
$ gdb ./threeFloats 
--snip--

(gdb) b 18
Breakpoint 1 at 0x121f: file threeFloats.c, line 18.
(gdb) r
Starting program: /home/bob/progs/chapter_19/threeFloats_C/threeFloats
Enter a number: 1.1
Enter a number: 1.2
Enter a number: 1.3

Breakpoint 1, main () at threeFloats.c:18
18	  sum1 = x + y;
(gdb) p x
$1 = 1.10000002
(gdb) p y
$2 = 1.20000005
(gdb) p z
$3 = 1.29999995
```

We can see that there is a rounding error in each number as it's stored. Let's step through each statement one at a time and look at how the sums build up:

```bash
(gdb) n
19	  sum1 += z;
(gdb) p sum1
$4 = 2.30000019
```

Both `x` and `y` have rounding errors, and adding them introduces even more rounding error in their sum:

```bash
(gdb) n
20	  sum2 = y + z;
(gdb) p sum1
$5 = 3.60000014
```

Next, we'll follow the buildup of `sum2`:

```bash
(gdb) n
21	  sum2 += x;
(gdb) p sum2
$6 = 2.5
(gdb) n
22	  if (sum1 == sum2)
(gdb) p sum2
$7 = 3.5999999
(gdb) cont
Continuing.
3.600000 is not the same as 3.600000
[Inferior 1 (process 2406) exited normally]
(gdb)
```

Using the debugger to look at the storage of each number and watching the sums build up allows us to see the effects of rounding errors in the `float` storage format. The `%f` format tells `printf` to display six decimal places, rounded as needed. So our program correctly tells us that 3.60000014 ≠ 3.5999999, but `printf` rounds both numbers to 3.600000.

## Comments About Numerical Accuracy

Beginning programmers often see floating-point numbers as real numbers and thus think they are more accurate than integers. It's true that using integers carries its own set of problems: even adding two large integers can cause overflow. Multiplying integers is even more likely to produce a result that will overflow. And we need to take into account that integer division results in two values, the quotient and the remainder, instead of the one value that floating-point division gives us.

But floating-point numbers are not real numbers. As you've seen in this chapter, floating-point representations extend the range of numerical values but have their own set of potential inaccuracies. Arithmetically accurate results require a thorough analysis of your algorithm. These are some points to consider:

1. Try to scale the data such that integer arithmetic can be used.
2. Using `double`s instead of `float`s improves accuracy and may actually increase the speed of execution. Most C and C++ library routines take `double`s as arguments, so the compiler converts `float`s to `double`s when passing them as arguments, as we saw in the call to `printf` in [Listing 19-7](#c19.xhtml#listing19-7).
3. Try to arrange the order of computations so that similarly sized numbers are added or subtracted.
4. Avoid complex arithmetic statements, which may obscure incorrect intermediate results.
5. Choose test data that stresses your algorithm. If your program processes fractional values, include data that has no exact binary equivalent.

The good news is that with today's prevalence of 64-bit computers, the range of integers is –9,223,372,036,854,775,808 ≤ *N* ≤ +9,223,372,036,854,775,807. And there are libraries available in many programming languages that allow us to use arbitrary-precision arithmetic in our programs. A good resource for finding one to use is https://en.wikipedia.org/wiki/List_of_arbitrary-precision_arithmetic_software.

We've looked at the primary causes of numerical errors when using floating-point numbers. For a more rigorous mathematical treatment of the topic, good starting points would be David Goldberg's paper, "What Every Computer Scientist Should Know About Floating-Point Arithmetic," ACM Computing Surveys, Vol 23, No 1, March 1991, and https://en.wikipedia.org/wiki/Floating-point_arithmetic. For an example of a programming technique to reduce rounding errors, you can read about the Kahan summation algorithm at https://en.wikipedia.org/wiki/Kahan_summation_algorithm.

## What You've Learned

1. Binary representation of fractional values: Fractional values in binary are equal to sums of inverse powers of two.
2. Fixed point in binary: The binary point is assumed to be between two specific bits.
3. Floating-point numbers are not real numbers: The gap between adjacent floating-point numbers varies according to the exponent.
4. Floating-point is usually less accurate than fixed-point: Rounding errors are commonly obscured by floating-point format normalization and can accumulate through multiple computations.
5. IEEE 754: The most common standard for representing floating-point values in a computer program. The integer part is always 1. The exponent specifies the number of bits included in, or excluded from, the integer part.
6. SSE floating-point hardware: A separate set of hardware in the CPU, with its own registers and instruction set, for working with floating-point numbers.

So far in this book, we have discussed programs that follow a step-by-step order of execution of instructions. But in some instances, an instruction cannot do anything meaningful with its operands—for example, when we divide by 0. As you saw earlier in this chapter, that can trigger an exception to the intended order of program execution. And we may want to allow outside events, like typing a key on the keyboard, to interrupt the ongoing program execution. After discussing input/output in Chapter 20, we'll look at interrupts and exceptions in Chapter 21.
