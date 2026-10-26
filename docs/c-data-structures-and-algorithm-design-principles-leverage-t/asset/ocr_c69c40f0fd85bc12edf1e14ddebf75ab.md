```
```cpp
// Overloads the << operator so an object can be written directly to a
// stream
// e.g. Can be used as std::cout << obj << std::endl;

template <typename Wtype,
          typename Vtype,
          typename Ftype>
friend std::ostream& operator<<(std::ostream& os,
                              const Object<Wtype,Vtype,Ftype>& obj);
};

template <typename Wtype,
          typename Vtype,
          typename Ftype>
std::ostream& operator<<(std::ostream& os, const
Object<Wtype,Vtype,Ftype>& obj)
{
    os << "Value: " << obj.value
       << "\tWeight: " << obj.weight
       << "\tValue/Unit Weight: " << obj.value_per_unit_weight;
    return os;
}
```
```

Note that we have overloaded the < and == operators since we will use **std::sort()** over a vector of objects.

2. The code for the fractional knapsack solver is as follows:

```
```cpp
template<typename weight_type,
         typename value_type,
         typename fractional_type>
auto fill_knapsack(std::vector<Object<weight_type, value_type, fractional_type>>& objects,
                  weight_type knapsack_capacity)
{
    std::vector<Object<weight_type, value_type, fractional_type>>
    knapsack_contents;
    knapsack_contents.reserve(objects.size());

    // Sort objects in the decreasing order
    std::sort(objects.begin(), objects.end());
```
```