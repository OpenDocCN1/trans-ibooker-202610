# Exercise 25: Fractional Knapsack Problem

In this exercise, we will consider 10 items and try to maximize the value in our knapsack, which can hold a maximum weight of 25 units. Let's get started:

+   1. First, we will begin by adding the required headers and defining an Object struct that will represent one object in our solution:

```
#include <iostream>
#include <algorithm>
#include <vector>
#include <random>
#include <numeric>

template <typename weight_type,
    typename value_type,
    typename fractional_type>
struct Object
{
    using Wtype = weight_type;
    using Vtype = value_type;
    using Ftype = fractional_type;

    Wtype weight;
    Vtype value;
    Ftype value_per_unit_weight;

    // NOTE: The following overloads are to be used for std::sort() and I/O
    inline bool operator< (const Object<Wtype,Vtype,Ftype>& obj) const
    {
        // An object is better or worse than another object only on the
        // basis of its value per unit weight
        return this->value_per_unit_weight < obj.value_per_unit_weight;
    }

    inline bool operator== (const Object<Wtype, Vtype, Ftype>& obj) const
    {
        // An object is equivalent to another object only if
        // its value per unit weight is equal
        return this->value_per_unit_weight == obj.value_per_unit_weight;
    }
```