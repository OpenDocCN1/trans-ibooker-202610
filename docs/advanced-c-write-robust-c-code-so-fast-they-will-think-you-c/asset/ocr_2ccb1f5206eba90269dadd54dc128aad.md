We receive the following output:

![](bbox=[0.06, 0.15, 0.88, 0.25])

Figure 8.39: Output of the perf command's timing report for the code in Snippet11.cpp

In the preceding output, notice that about 33% of the execution time was taken by the std::string constructor, strlen(), and memmove(). All of these are associated with the std::string that was used in fun1(). The heap allocation in particular is the slowest operation.

# Struct of Arrays Pattern

In many programs, we often use an array of objects of the same type – these could represent records from a database, entities in a game, and so on. A common pattern is to iterate through a large array of structures and perform an operation on some fields. Even though the structs are sequential in memory, if we access only a few of fields, a larger size structure will make caching less effective.

The processor may prefetch several structs into cache, but the program only accesses a fraction of that cached data. Since it is not using every field of each struct, most of the cached data is discarded. To avoid this, another kind of data layout can be used – instead of using an array of structs (AoS) pattern, we use a struct of arrays (SoA) pattern. In the next section, we'll solve an exercise wherein we'll examine the performance benefit of using the SoA pattern versus the AoS pattern.

# Exercise 13: Using the Struct of Arrays Pattern

In this exercise, we will examine the performance benefits of using the SoA versus AoS pattern. Perform these steps to complete this exercise:

+   1. Create a file named Snippet12.cpp.

+   2. Include the necessary libraries, along with the Timer.h header file. Initialize a random number generator and also create a distribution range from 1 to N-1. Create a constant integer variable, N, and initialize it with a value of 100,000,000. Add the following code to implement this:

```
#include <vector>
#include <list>
#include <algorithm>
#include <string>
```