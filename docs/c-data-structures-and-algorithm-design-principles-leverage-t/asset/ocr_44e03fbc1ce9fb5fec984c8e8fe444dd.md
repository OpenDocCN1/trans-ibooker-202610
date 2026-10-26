# Exercise 29: Implementing DFS

In this exercise, we shall implement the DFS algorithm in C++ and test it on the graph shown in *figure 6.2*. The steps are as follows:

+   1. Include the required header files, as follows:

```
```cpp
#include <string>
#include <vector>
#include <iostream>
#include <set>
#include <map>
#include <stack>

template<typename T> class Graph;
```
```

+   2. Write the following struct in order to implement an edge in our graph:

```
```cpp
template<typename T>
struct Edge
{
    size_t src;
    size_t dest;
    T weight;

    // To compare edges, only compare their weights,
    // and not the source/destination vertices
    inline bool operator< (const Edge<T>& e) const
    {
        return this->weight < e.weight;
    }

    inline bool operator> (const Edge<T>& e) const
    {
        return this->weight > e.weight;
    }
};
```
```

Again, since our implementation uses a templatized version of the struct, it allows us to assign edge weights of any data type required. However, for the purposes of DFS, we shall use null values as placeholders for the edge weights.