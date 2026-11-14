
using std::endl;

3. Create a constant integer variable, `N`, and set its value to `100000`:

```cpp
const int N = 100000;
```

4. Initialize a random number generator and create a distribution range from `0` to `1000`. Add the following code to achieve this:

```cpp
std::random_device dev;
std::mt19937 rng(dev());
std::uniform_int_distribution<std::mt19937::result_type> dist(0, N);
```

5. Create a method named `insertRandom()` and insert elements from `0` to `N` into a container at random positions. Add the following code to implement this:

```cpp
template<class C> void insertRandom(C &l)
{
    // insert one element to initialize
    l.insert(l.end(), 0);
    for(int i = 0; i < N; ++i)
    {
        int pos = dist(rng) % l.size();
        auto it = l.begin();
        advance(it, pos);
        l.insert(it, i);
    }
}
```

6. Create a method named `insertStart()` and insert elements from `0` to `N` into a container at the start. Add the following code to implement this:

```cpp
template<class C> void insertStart(C &l)
{
    for(int i = 0; i < N; ++i)
    {
        l.insert(l.begin(), i);
    }
}
```

7. Create a method named `insertEnd()` and insert elements from `0` to `N` into a container at the end. Add the following code to implement this:

```cpp
template<class C> void insertEnd(C &l)
{
    for(int i = 0; i < N; ++i)
    {
        l.insert(l.end(), i);
    }
}
```

8. Write the following code in the `main` method:

```cpp
int main()
{
    std::list<int> l;
    std::vector<int> v;
    // list
    {
        Timer t("list random");
        insertRandom(l);
    }
    {
        Timer t("list end");
        insertEnd(l);       
    }
    {
        Timer t("list start");
        insertStart(l);
    }
    // vector
    {
        Timer t("vect random");
        insertRandom(v);
    }
    {
        Timer t("vect end");
        insertEnd(v);       
    }
    {
        Timer t("vect start");
        insertStart(v);
    }
    cerr << endl << l.size() << endl << v.size() << endl;
    Timer::dump();
}
```

9. Save the file and open the terminal. Compile the preceding program, along with the `Timer.cpp` file, by writing the following commands:

```bash
$ g++ -O3 Snippet8.cpp Timer.cpp
$ ./a.out
```

The preceding command generates the following output:

![Figure 8.34: Output of the code in Snippet8.cpp contrasting the timing of std::list and std::vector insertion](image/C14583_08_34.jpg)

###### Figure 8.34: Output of the code in Snippet8.cpp contrasting the timing of std::list and std::vector insertion

As you can see from the preceding output, the code measures the time taken to insert `100000` integers at the start, end, and random locations for `std::vector` and `std::list`. The vector clearly wins by a factor of 100 or more for the random case, and even the worst case for the vector is 10x faster than the random case for the list.

Why does this happen? The answer lies in the way modern computer architecture has evolved. CPU clock speeds have increased from about `1 Mhz` in the early 80s to `5 GHz` as of mid-2019 – a speedup of `5,000x` in clock frequency – and while the earliest CPUs used multiple cycles per instruction, modern ones execute several instructions per cycle on a single core (due to advanced techniques such as pipelining, which we described earlier).

For example, the `IDIV` instruction on the original `Intel 8088` took over 100 clock cycles to complete, whereas on modern processors, it can be completed in less than 5 cycles. On the other hand, RAM bandwidth (the time taken to read or write a byte of memory) has increased very slowly.

Historically, processors have increased in speed by a factor of about `16,000x` between 1980 and 2010. At the same time, the speed increase in RAM has been an order of magnitude smaller – less than 100x. Thus, it is possible that single access to RAM by an instruction causes the CPU to wait for a huge number of clock cycles. This would be an unacceptable degradation of performance, and there have been a lot of technologies to mitigate this issue. Before we explore this, let's measure the impact of memory access in the next exercise.

### Exercise 11: Measuring the Impact of Memory Access

In this exercise, we will examine the performance impact of randomly accessing memory. Perform these steps to complete this exercise:

1. Create a new file named `Snippet9.cpp`.

2. Include the necessary libraries, along with the `Timer.h` header file. Create two constant integer variables, `SIZE` and `N`, and set their values to `100000000`. Also, create a random number generator and a distribution range from `0` to `N-1`. Write the following code to implement this:

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
using std::endl;

const int SIZE = 100'000'000;
const int N = 100'000'000;

std::random_device dev;
std::mt19937 rng(dev());
std::uniform_int_distribution<std::mt19937::result_type> dist(0, SIZE-1);
```

3. Create the `getPRIndex()` function, which returns a pseudo random index between `0` and `SIZE-1`, where `SIZE` is the number of elements in the array. Write the following code to implement this:

#### Note

We will discuss why we use a random number later.

```cpp
uint64_t getPRIndex(uint64_t i)
{
    return (15485863 * i) % SIZE;
}
```

4. Write a function named `sum1()` that accesses a large array of data randomly and sums those elements:

```cpp
uint64_t sum1(vector<int> &v)
{
    `TIME_IT`;
    uint64_t sum = 0;
    for(int i = 0; i < N; ++i)
    {
        sum += v[getPRIndex(i)];
    }
    return sum;
}
```

5. Write a function named `sum2()` that sums random numbers without any memory access:

```cpp
uint64_t sum2()
{
    `TIME_IT`;
    uint64_t sum = 0;
    for(int i = 0; i < N; ++i)
    {
        sum += getPRIndex(i);
    }
    return sum;
}
```

6. In the main function, initialize the vector so that `v[i] == i`, thus, the only difference between `sum1()` and `sum2()` is that `sum1()` accesses memory and `sum2()` only performs computations. As usual, we use `volatile` to prevent the compiler from removing all the code, since it has no side effects. Write the following code in the `main()` function:

```cpp
int main()
{
    // Allocate SIZE integers
    std::vector<int> v(SIZE, 0);
    // Fill 0 to SIZE-1 values into the vector
    for(int i = 0; i < v.size(); ++i)
    {
        v[i] = i;
    }
    volatile uint64_t asum1 = sum1(v);
    volatile uint64_t asum2 = sum2();
    Timer::dump();
}
```

7. Save the program and open the terminal. Compile and run the program by writing the following commands:

```bash
$ g++ -O3 Snippet9.cpp Timer.cpp
$ ./a.out
```

The preceding code generates the following output:

![Figure 8.35: Output of the code in Snippet9.cpp contrasting the timing of computation versus random memory access](image/C14583_08_35.jpg)

###### Figure 8.35: Output of the code in Snippet9.cpp contrasting the timing of computation versus random memory access

From the preceding output, we can clearly see a factor of about `14x` difference in performance.

8. Create a new file named `Snippet10.cpp` and add the same code that was present in `Snippet9.cpp`. Add a new function named `sum3()` that accesses memory linearly instead of randomly. Also, edit the main function. The updated code is as follows:

```cpp
uint64_t sum3(vector<int> &v)
{
    `TIME_IT`;
    uint64_t sum = 0;
    for(int i = 0; i < N; ++i)
    {
        sum += v[i];
    }
}
```
