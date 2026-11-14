
## Profiling

**Profiling** is a non-intrusive method of measuring the performance of the functions in a program. Profilers work by sampling a program's current execution address at frequent intervals (hundreds of times in a second) and making a log of which functions happened to be executing at the time. This is a statistical sampling approach that has reasonable accuracy. Sometimes, though, the results can be confusing as a program may spend a lot of time on functions that are a part of the operating system kernel. The most popular runtime profiling tool on Linux is `perf`. In the next section, we'll make use of `perf` to profile our program.

### Exercise 3: Using `perf` to Profile Programs

`perf` can be installed on `Ubuntu` as follows:

```
apt-get install linux-tools-common linux-tools-generic
```

To get familiar with the basics of using `perf`, we'll profile and analyze the program from the previous exercise with the help of the `perf` tool. Perform the following steps to complete this exercise:

1.  Open the `Snippet2.cpp` file we created in the previous exercise, and remove the `TIME_IT` macros from the two functions.

2.  Open the terminal, recompile the code again with the `-O3` flag, and then create a profile data sample with `perf` as follows:

    ```
    g++ -O3 Snippet2.cpp
    perf record ./a.out
    ```

    The output of the previous command is as follows:

    ![](image/C14583_08_21.jpg)

    ###### Figure 8.21: Using the `perf` command to analyze the code in `Snippet2.cpp`

    This creates a file called `perf.data` which can be analyzed or visualized.

3.  Now, use the following command to visualize the recorded data:

    ```
    perf report
    ```

    A console-based GUI will show the following data after executing the previous command:

    ![Figure 8.22: Using the `perf` command to analyze the code in `Snippet2.cpp` ](image/C14583_08_22.jpg)

    ###### Figure 8.22: Using the `perf` command to analyze the code in `Snippet2.cpp`

    You can move the cursor up and down to select a function and press `Enter` to get a list of options.

4.  Highlight `testMul`, press `Enter`, and choose **Annotate testMul** in the resulting list. A list of assembly code is shown, with annotations describing the percentage of execution time for each line of code, as follows:

    ![Figure 8.23: Viewing the timing statistics using the `perf` command for the `Snippet2.cpp` code ](image/C14583_08_23.jpg)

    ###### Figure 8.23: Viewing the timing statistics using the `perf` command for the `Snippet2.cpp` code

    Notice that the **Integer Multiply** (`IMUL`) instruction (integer multiply) took over **99%** of the time to execute. Traditionally, integer multiplications are always expensive on the `x86` architecture and this continues to be true, even in the latest generation of CPUs. This annotation view displays arrows next to each jump or branching instruction which, when highlighted, shows what comparison instruction it is associated with and what address it jumps to with line drawings. You can navigate to the previous view by pressing the left arrow key and exit the program using the `q` key.

Up until now, we've looked at several methods that are used to assess the performance of our programs. This is the most critical stage of optimization since it tells us where we need to direct our efforts. In the upcoming sections, we will explore various techniques that will help us optimize our code.

## Optimization Strategies

Optimization of code can be done in several ways, such as the following:

*   Compiler-based optimization
*   Source code micro-optimization
*   Cache-friendly code
*   Algorithmic optimization

Here, each technique has its pros and cons. We will examine each of these methods in detail in the upcoming sections. Roughly speaking, these are ordered in terms of effort required and also potential gains in performance. We'll look at compiler-based optimization in the next section.

### Compiler-Based Optimization

Passing the correct options to the compiler can net many performance benefits. A real-world example of this is the Clear Linux `distro` (Linux distribution) that was created by Intel. This `distro` has been compiled to extract the maximum performance from all the code and outperforms most other Linux distributions by 30% on most benchmarks, which is a very significant speedup. On the `gcc` and `clang` family of compilers, the most basic option for optimization is `-O<N>`, where `N` is one of the numbers `1`, `2`, or `3`. `-O3` enables almost every optimization in the compiler, but there are several others not enabled by that flag that can make a difference.

### Loop Unrolling

**Loop unrolling** is a technique that can be used by compilers to reduce the number of branches that are executed. Every time a branch is executed, there is a certain performance overhead. This can be reduced by repeating the loop body multiple times, and reducing the number of times the loop is executed. Loop unrolling can be done at the source level by the programmer, but modern compilers do a very good job automatically.

Even though modern processors mitigate the overhead of branching by means of **branch prediction** and **speculative execution** circuits, loop unrolling still produces performance benefits. Loop unrolling optimizations can be enabled on the `gcc` and `clang` family of compilers with the `-funroll-loops` command-line flag. In the next section, we'll test the performance of a program with and without loop unrolling enabled.

### Exercise 4: Using Loop Unrolling Optimizations

In this exercise, we'll write a simple program that uses nested loops and test its performance with and without loop unrolling enabled. We'll understand the way compilers implement the automatic unrolling of loops.

Perform these steps to complete this exercise:

1.  Create a file named `Snippet3.cpp`.

2.  Write a program that takes the first **10,000** numbers and prints out how many of these are factors of each other (the full code can be found in `Snippet3.cpp`):

    ```cpp
    #include <iostream>

    int main()
    {
        int ret = 0;
        for(size_t i = 1; i < 10000; ++i)
        {
            for(size_t j = 1; j < 10000; ++j)
            {
                if(i % j == 0)
                {
                    ++ret;
                }
            }
        }
        std::cout << "Result: " << ret << std::endl;
    }
    ```

3.  Save the program and open the terminal. Compile the program with the `-O3` flag first and time it using the following command:

    ```
    g++ -O3 Snippet3.cpp
    time ./a.out
    ```

    The output of the previous command is as follows:

    ![Figure 8.24: Output of the code in `Snippet3.cpp` ](image/C14583_08_24.jpg)

    ###### Figure 8.24: Output of the code in `Snippet3.cpp`

4.  Now, compile the same code with the loop unrolling enabled and time it again:

    ```
    g++ -O3 -funroll-loops Snippet3.cpp
    time ./a.out
    ```

    The output of the previous command is as follows:

    ![Figure 8.25: Output of the code in `Snippet3.cpp` compiled with the loop unrolling option ](image/C14583_08_25.jpg)

    ###### Figure 8.25: Output of the code in `Snippet3.cpp` compiled with the loop unrolling option

5.  Open the **Godbolt compiler explorer** and paste the preceding complete code into the left-side.

6.  On the right-hand side, select `x86-64 gcc 8.3` from the compiler options and write the `-O3` flag in the options. Assembly code will be generated. For the for loop, you'll see the following output:

    ![Figure 8.26: Assembly code of the for loop ](image/C14583_08_26.jpg)

    ###### Figure 8.26: Assembly code of the for loop
