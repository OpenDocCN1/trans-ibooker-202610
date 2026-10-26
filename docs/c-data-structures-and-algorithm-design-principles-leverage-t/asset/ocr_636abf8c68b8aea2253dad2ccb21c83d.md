# Exercise 11: Implementing a Graph and Representing it as an Adjacency Matrix

In this exercise, we will implement a graph representing the network of cities shown in the preceding figure, and demonstrate how it can be stored as an adjacency matrix. Let's get started:

+   1. First, let's include the required headers:

```
#include <iostream>
#include <vector>
```

+   2. Now, let's add an `enum` class so that we can store the names of the cities:

```
enum class city: int
{
    LONDON,
    MOSCOW,
    ISTANBUL,
    DUBAI,
    MUMBAI,
    SEATTLE,
    SINGAPORE
};
```

+   3. Let's also add a `<<` operator for the `city` enum:

```
std::ostream operator<<(std::ostream os, const city c)
{
    switch(c)
    {
        case city::LONDON:
            os << "LONDON";
            return os;
        case city::MOSCOW:
            os << "MOSCOW";
            return os;
        case city::ISTANBUL:
            os << "ISTANBUL";
            return os;
        case city::DUBAI:
            os << "DUBAI";
            return os;
        case city::MUMBAI:
            os << "MUMBAI";
```