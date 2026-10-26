
This creates a file called **`perf.data`** which can be analyzed or visualized.

- 3. Now, use the following command to visualize the recorded data:

```
`$ perf report`
```

A console-based GUI will show the following data after executing the previous command:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_582_0.png)

Figure 8.22: Using the perf command to analyze the code in Snippet2.cpp

You can move the cursor up and down to select a function and press *Enter* to get a list of options.

- 4. Highlight **testMul**, press *Enter*, and choose **Annotate testMul** in the resulting list. A list of assembly code is shown, with annotations describing the percentage of execution time for each line of code, as follows:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_582_1.png)

Figure 8.23: Viewing the timing statistics using the perf command for the Snippet2.cpp code

Notice that the **Integer Multiply (IMUL)** instruction (integer multiply) took over **99%** of the time to execute. Traditionally, integer multiplications are always expensive on the **x86** architecture and this continues to be true, even in the latest generation of CPUs. This annotation view displays arrows next to each jump or branching instruction which, when highlighted, shows what comparison instruction it is associated with and what address it jumps to with line drawings. You can navigate to the previous view by pressing the left arrow key and exit the program using the *q* key.

Even though modern processors mitigate the overhead of branching by means of **branch prediction** and **speculative execution** circuits, loop unrolling still produces performance benefits. Loop unrolling optimizations can be enabled on the **gcc** and **clang** family of compilers with the **-funroll-loops** command-line flag. In the next section, we'll test the performance of a program with and without loop unrolling enabled.

## Exercise 4: Using Loop Unrolling Optimizations

In this exercise, we'll write a simple program that uses nested loops and test its performance with and without loop unrolling enabled. We'll understand the way compilers implement the automatic unrolling of loops.

Perform these steps to complete this exercise:

- 1. Create a file named **Snippet3.cpp**.
- 2. Write a program that takes the first **10,000** numbers and prints out how many of these are factors of each other (the full code can be found in **Snippet3.cpp**):
- 3. Save the program and open the terminal. Compile the program with the **-O3** flag first and time it using the following command:

```
# include <iostream>
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

```
$ g++ -O3 Snippet3.cpp
$ time ./a.out
```

The output of the previous command is as follows:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_585_0.png)

Figure 8.24: Output of the code in Snippet3.cpp

- 4. Now, compile the same code with the loop unrolling enabled and time it again:

```
$ g++ -O3 -funroll-loops Snippet3.cpp
$ time ./a.out
```

The output of the previous command is as follows:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_585_1.png)

Figure 8.25: Output of the code in Snippet3.cpp compiled with the loop unrolling option

- 5. Open the Godbolt compiler explorer and paste the preceding complete code into the left-side.

- 6. On the right-hand side, select x86-64 gcc 8.3 from the compiler options and write the -O3 flag in the options. Assembly code will be generated. For the for loop, you'll see the following output:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_585_2.png)

Figure 8.26: Assembly code of the for loop

## Exercise 5: Using Profile Guided Optimization

In this exercise, we will use profile guided optimization on the code from the previous exercise. We'll understand how to use profile guided optimization with the gcc compiler.

Perform these steps to complete this exercise:

- 1. Open the terminal and compile the code from the previous exercise with profiling enabled. Include any other optimization flags that we need (in this case, -O3). Write the following code to implement this:
  $ g++ -O3 -fprofile-generate Snippet3.cpp

- 2. Now, run the profiled version of the code by writing the following command:
  $ ./a.out

The program runs normally and prints the result, and no other output is seen - but it generates a file containing data that will help the compiler in the following step. Note that with profiling enabled, the program executes several times slower than it would normally. This is something to keep in mind with large programs. After executing the previous command, a file called Snippet3.gcda will be generated, which contains profile data. When doing this with large, complex applications, it is important to run the program with the datasets and workflows that it will most commonly encounter in the production environment. By choosing the data correctly here, the eventual performance gain will be higher.

- 3. Recompile with the PGO optimization flags, that is, -fprofile-use and -fprofile-correction, as illustrated in the following code:
  $ g++ -O3 -fprofile-use -fprofile-correction Snippet3.cpp

Note that other than the profile-related compiler options, the other options must be exactly the same as the ones in the previous compilation step.

- 4. Now, if we time the executable, we will see a large performance improvement:
  $ time ./a.out

The output of the previous command is as follows:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_587_0.png)

Figure 8.28: Timing results of the code in Snippet3.cpp with PGO optimization

```
using std::endl;
int main()
{
    // Fill 100,000,000 1s in a vector
    std::vector<int> v( 100'000'000, 1);
    // Call accumulate 100 times, accumulating to a total sum
    uint64_t total = 0;
    for(int i = 0; i < 100; ++i)
    {
        total += std::accumulate(v.begin(), v.end(), 0);
    }
    std::cout << "Total: " << total << std::endl;
}
```

3. Save the program and open the terminal. Compile the program normally and time the execution using the following commands:

```
$ g++ -O3 Snippet4.cpp
$ time ./a.out
```

The output of the previous command is as follows:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_589_0.png)

Figure 8.29: Output of the code in Snippet4.cpp

4. Now, compile the code with the parallelization options, that is, -O3 -fopenmp and -D_GLIBCXX_PARALLEL:

```
$ g++ -O3 -fopenmp -D_GLIBCXX_PARALLEL Snippet4.cpp
$ time ./a.out
```

The output is as follows:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_589_1.png)

Figure 8.30: Output of the code in Snippet4.cpp compiled with parallelization options

We can avoid these multiple allocations and copies by using the **reserve()** method. When we know how many elements a vector will contain, calling the **reserve()** method to pre-allocate the storage makes quite a difference. Let's implement an exercise in the next section to optimize vector growth.

## Exercise 7: Optimizing Vector Growth

In this exercise, we will time the effect of the **push_back** method in a loop, with and without calling the reserve method. First, we will extract the **Timer** class we used in the previous sections into a separate header and implementation file – this will allow us to use it as common code for all the succeeding code snippets. Perform these steps to complete this exercise:

- 1. Create a header file named **Timer.h.**

- 2. Include the necessary header files:

```
#include <map>
#include <string>
#include <chrono>
#include <cstdint>
```

- 3. Create a class named **Timer**. Within the **Timer** class, declare four variables, namely **ms_Counts**, **ms_Times**, **m_tmStart**, and **m_sName**. Declare a constructor, destructor, and the **dump()** method. Add the following code to implement this:

```
class Timer
{
    static std::map<std::string, int64_t> ms_Counts;
    static std::map<std::string, int64_t> ms_Times;
    std::string m_sName;
    std::chrono::time_point<std::chrono::high_resolution_clock> m_tmStart;
public:
    // When constructed, save the name and current clock time
    Timer(std::string sName);
    // When destroyed, add the time elapsed and also increment the count under this name
    ~Timer();
    // Print out the stats for each measured block/function
    static void dump();
};
```
