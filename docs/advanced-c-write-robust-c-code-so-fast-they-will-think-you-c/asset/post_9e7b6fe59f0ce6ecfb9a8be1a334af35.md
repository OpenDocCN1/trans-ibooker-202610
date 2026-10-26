
+   4. Define a helper macro named **TIME_IT** to time functions by writing the following code:

```
```cpp
// Helper macro to time functions
#define TIME_IT Timer t(__func__)
```
```

+   5. Once the header file has been created, create a new file named **Timer.cpp** and include the **Timer.h** file in it. Also, write the actual implementation for the constructors, destructors, and the `dump()` method inside the **Timer.cpp** file. Write the following code to implement this:

```
```cpp
#include <string>
#include <iostream>
#include <cstdint>
#include "Timer.h"

using std::map;
using std::string;
using std::cerr;
using std::endl;

// When constructed, save the name and current clock time
Timer::Timer(string sName): m_sName(sName)
{
    m_tmStart = std::chrono::high_resolution_clock::now();
}
// When destroyed, add the time elapsed and also increment the count under
// this name
Timer::~Timer()
{
    auto tmNow = std::chrono::high_resolution_clock::now();
    auto msElapsed = std::chrono::duration_cast<std::chrono::milliseconds>(tmNow - m_tmStart);
    ms_Counts[m_sName]++;
    ms_Times[m_sName] += msElapsed.count();
}
// Print out the stats for each measured block/function
void Timer::dump()
{
    cerr << "Name\t\t\tCount\t\t\tTime(ms)\t\tAverage(ms)\n";
    cerr << "-------------------------------------------------------------\n";
    for(const auto& it: ms_Times)
```

As we can see, the effect of calling **reserve()** has resulted in an improvement of about 4% in execution time. In a larger program that has run for a long time, the system memory often becomes very fragmented. In such cases, the improvement by pre-allocating memory with **reserve()** could be much better. In general, it is usually faster to reserve memory beforehand, rather than doing it incrementally on the fly. Even the Java Virtual Machine, for performance reasons, uses this technique of allocating a huge chunk of memory upfront when starting up.

## Short-Circuit Logical Operators

The && and || logical operators are **short-circuited**, which means that the following:

- If the left-hand side of the || operator is **true**, the right-hand side is not evaluated.
- If the left-hand side of the && operator is **false**, the right-hand side is not evaluated.

By keeping the more unlikely (or less expensive) expression on the left-hand side, we can reduce the amount of work that needs to be done. In the next section, we'll solve an exercise and learn how to write logical expressions optimally.

## Exercise 8: Optimizing Logical Operators

In this exercise, we will examine the impact of ordering conditional expressions when used with logical operators. Perform these steps to complete this exercise:

+   1. Create a new file named **Snippet6.cpp**.
+   2. Include the necessary libraries and the Timer.h file that we created in the previous exercise by writing the following code:

```
#include <vector>
#include <string>
#include <iostream>
#include <random>

#include "Timer.h"

using std::vector;
using std::cerr;
using std::endl;
```

3. Define a function named sum1() that computes the sum of the integers between 0 and N. Each number is summed only if it meets either or of two specific criteria. The first condition is that the number must be less than N/2. The second condition is that the number, when divided by 3, must return 2 as a remainder. Here, we set N to 100,000,000 so we have some measurable time taken by the code. Write the following code to implement this:

```
const uint64_t N = 100000000;

uint64_t sum1()
{
    TIME_IT;
    uint64_t ret = 0;
    for(uint64_t b=0; b < N; ++b)
    {
        if(b % 3 == 2 || b < N/2)
        {
            ret += b;
        }
    }

    return ret;
}
```

4. Now, define another function named sum2(). It must contain the same logic that we wrote for the previous function, sum1(). The only change here is that we reverse the order of the conditional expression of the if statement. Write the following code to implement this:

```
uint64_t sum2()
{
    TIME_IT;
    uint64_t ret = 0;
    for(uint64_t b=0; b < N; ++b)
    {
        if(b < N/2 || b % 3 == 2)
        {
            ret += b;
        }
    }

    return ret;
}
```

Note that in the sum2 function, the b < N/2 condition will evaluate to true half of the time. Thus, the second condition, that is, b % 3 == 2, is only evaluated for half of the iterations. If we assume for simplicity that both conditionals take 1 unit of time, the total time taken for sum2() would be N/2 + (2 * N/2) = N * 3/2. In the case of the sum1() function, the condition on the left-hand side will evaluate to true only 33% of the time, and the remaining 66% of the time, both conditions will be evaluated. Thus, the estimated time taken would be N/3 + (2 * N * 2/3) = N * 5/3. We expect that the ratio between the times for the sum1 and sum2 function would be 5/3 to 3/2 – that is, sum1 is 11% slower.

5. Add the following code in the main function:

```
int main()
{
    volatile uint64_t dummy = 0;
    for(int i = 0; i < 100; ++i)
    {
        dummy = sum1();
    }
    for(int i = 0; i < 100; ++i)
    {
        dummy = sum2();
    }
    Timer::dump();
}
```

6. Save the file and open the terminal. Compile and time the preceding program, as well as the Timer.cpp file, by writing the following commands:

```
$ g++ -O3 Snippet6.cpp Timer.cpp
$ ./a.out
```

The output is as follows:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_597_0.png)

Figure 8.32: Output of the code in Snippet6.cpp showing the effect of optimizing boolean conditions

As you can see from the preceding output, we ended up with about 38% improvement in speed, which is much more than expected. Why would this happen? The answer is that the % operator performs an integer division, which is much more expensive than a comparison, but the compiler will not generate a division instruction for the N/2 expression because it is a constant value.

For the first function, the outer loop fails branch prediction only once when it exits, but the inner loop fails branch prediction **100,000,000** times – each time it exits. For the second one, once again, the outer loop fails branch prediction only once when it exits, but the inner loop fails branch prediction only 100 times – each time it exits. The factor of **1,000,000** between these branch prediction failure counts will result in the first function being slower than the second. Perform these steps to complete this exercise:

+   1. Create a file named **Snippet7.cpp** and include the necessary libraries:

```
```cpp
#include <vector>
#include <string>
#include <iostream>
#include <random>
#include "Timer.h"

using std::vector;
using std::cerr;
using std::endl;
```
```

+   2. Define a function named **sum1()** with a nested loop. The outer for loop should cycle N times, whereas the inner for loop should iterate 100 times. Set the value of **N to 100000000**. Write the following code to implement this:

```
```cpp
const uint64_t N = 100000000;
uint64_t sum1()
{
    TIME_IT;
    uint64_t ret = 0;
    for(int i = 0; i < N; ++i)
    {
        for(int j = 0; j < 100; ++j)
        {
            ret += i ^ j;
        }
    }
    return ret;
}
```
```

If we assume that the processor predicts branches in loops (statistically, the branch instruction at the end of the loop is more likely to jump to the start of the loop than not), then it will end up mispredicting every time j reaches 100 – in other words, N times.

3. Define a new function, sum2(), with a nested loop. The only change here is that we must set the inner loop count to N and the outer loop count to 100. Add the following code to implement this:

```
uint64_t sum2()
{
    TIME_IT;
    uint64_t ret = 0;
    for(int i = 0; i < 100; ++i)
    {
        for(int j = 0; j < N; ++j)
        {
            ret += i ^ j;
        }
    }
    return ret;
}
```

Now, our reasoning is that the branch misprediction happens only 100 times.

4. Add the following code in the main function:

```
int main()
{
    volatile uint64_t dummy;
    dummy = sum1();
    dummy = sum2();
    Timer::dump();
}
```

5. Save the file and open the terminal. Compile the preceding program, along with the Timer.cpp file, and time them using the following commands. Remember that you need to have the Timer.cpp and Timer.h files you created earlier in the same directory:

```
$ g++ -O3 Snippet7.cpp Timer.cpp
$ ./a.out
```

The output of executing the previous command is as follows:

| Name | Count | Time(ms) | Average(ms) |
|------|-------|----------|-------------|
| sum1 | 1 | 2442 | 2442 |
| sum2 | 1 | 2377 | 2377 |

Figure 8.33: Output of the code in Snippet7.cpp showing the effect of branch prediction optimization
