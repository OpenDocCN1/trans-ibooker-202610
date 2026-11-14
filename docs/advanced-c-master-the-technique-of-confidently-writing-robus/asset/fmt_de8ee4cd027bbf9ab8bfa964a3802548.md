
# Branch Prediction Exercise

In this exercise, we will explore and demonstrate the effect of CPU branch prediction on performance. To explore this, we'll write two functions in a program – both perform the same computation using two nested loops which iterate **100** and **100,000,000** times, respectively. The difference between the two functions is that, in the first function, the outer loop is the bigger one, whereas in the second function, the outer loop is the smaller one.

For the first function, the outer loop fails branch prediction only once when it exits, but the inner loop fails branch prediction **100,000,000** times – each time it exits. For the second one, once again, the outer loop fails branch prediction only once when it exits, but the inner loop fails branch prediction only 100 times – each time it exits. The factor of **1,000,000** between these branch prediction failure counts will result in the first function being slower than the second. Perform these steps to complete this exercise:

1. Create a file named `Snippet7.cpp` and include the necessary libraries:

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

2. Define a function named `sum1()` with a nested loop. The outer `for` loop should cycle `N` times, whereas the inner for loop should iterate **100** times. Set the value of `N` to **100000000**. Write the following code to implement this:

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

If we assume that the processor predicts branches in loops (statistically, the branch instruction at the end of the loop is more likely to jump to the start of the loop than not), then it will end up mispredicting every time `j` reaches **100** – in other words, `N` times.

3. Define a new function, `sum2()`, with a nested loop. The only change here is that we must set the inner loop count to `N` and the outer loop count to **100**. Add the following code to implement this:

```cpp
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

Now, our reasoning is that the branch misprediction happens only **100** times.

4. Add the following code in the `main` function:

```cpp
int main()
{
    volatile uint64_t dummy;
    dummy = sum1();
    dummy = sum2();
    Timer::dump();
}
```

5. Save the file and open the terminal. Compile the preceding program, along with the `Timer.cpp` file, and time them using the following commands. Remember that you need to have the `Timer.cpp` and `Timer.h` files you created earlier in the same directory:

```bash
$ g++ -O3 Snippet7.cpp Timer.cpp
$ ./a.out
```

The output of executing the previous command is as follows:

![Figure 8.33: Output of the code in Snippet7.cpp showing the effect of branch prediction optimization](image/C14583_08_33.jpg)

*Figure 8.33: Output of the code in Snippet7.cpp showing the effect of branch prediction optimization*

As you can see from the preceding output, there is a small but certainly significant speedup of about **2%** that can be attributed to the processor being able to predict branches better for the `sum2` function. In the next section, we'll explore more optimization techniques.

## Further Optimizations

Several other techniques exist that can be implemented as you code; some of them are not guaranteed to produce better code, but it takes very little effort to change your coding habits to do these reflexively. They cost nothing but may result in gains. A few of these techniques are as follows:

* Pass parameters that are not primitive types by `const` reference when possible. Even though `move constructors` can make copying inexpensive, they still involve more overhead than using a `const` reference.
* Use pre-increment (`++i`) or pre-decrement (`--i`) operators rather than the postfix versions. This usually has no utility for simple types such as integers but may do so for complex types with a custom increment operator. Getting into a habit of writing `++i` rather than `i++` is good practice unless post-increment is actually the desired behavior. Apart from performance benefits, such code declares the intent more clearly by using the right operator.
* Declare variables as late as possible – it is common in C to declare every variable at the top of a function, but in C++, since variables can have non-trivial constructors, it makes sense to only declare them in the actual block where they are used.
* In terms of `loop hoisting`, if there is any code or calculation in a loop that does not change with the loop iteration, it makes sense to move it outside the loop. This includes creating objects in a loop body. Often, it is more efficient to declare them once, outside the loop. Modern compilers do this automatically, but it doesn't take extra effort to do this yourself.
* Use `const` wherever possible. It does not change the meaning of the code, but it lets the compiler make stronger assumptions about your code that may lead to better optimization. Apart from this, using `const` makes code more readable and reasonable.
* Integer division, modulus, and multiplication (especially by numbers that are not powers of 2) are some of the slowest operations possible on `X86` hardware. If you need to perform such operations in a loop, perhaps you can do some algebraic manipulation to get rid of them.

As we mentioned, several such optimizations may be done by the compiler itself, but doing them as a habit makes the code fast even in debug mode, which is a big advantage when debugging. We have examined a few techniques for micro-optimizing code already – the level of code change required to do these is relatively minor, and some of these can produce major improvements in efficiency. If you want to write faster code in general, you should aim to integrate these techniques as a default coding style over time. In the next section, we'll learn about cache-friendly code.

## Cache Friendly Code

Computer science was developed in the mid-20th century, when computers hardly existed, but nevertheless, by the 1980s, most of the useful data structures and algorithms had been discovered and refined. Algorithmic complexity analysis is a topic that anyone who learns computer science encounters – and there are well-accepted textbook definitions of the complexity of data structure operations. However, after 50 years since these things were analyzed, computers have evolved in a way that is quite different from what could have been envisaged. For example, a common "fact" is that the list data structures are faster for insertion operations than arrays. This seems like common sense because inserting an element into an array involves moving all the items after that point to new locations, whereas inserting into a list is merely a few pointer manipulations. We will test this hypothesis in the following exercise.

### Exercise 10: Exploring the Effect of Caches on Data Structures

In this exercise, we will examine the impact of the cache on arrays and lists in the C++ standard library. Perform these steps to complete this exercise:

1. Create a file named `Snippet8.cpp`.

2. Include the necessary libraries, along with the `Timer.h` header file. Write the following code to implement this:

```cpp
#include <vector>
#include <list>
#include <algorithm>
#include <string>
#include <iostream>
#include <random>
#include "Timer.h"

using std::vector;
using std::list;
using std::cerr;
```
