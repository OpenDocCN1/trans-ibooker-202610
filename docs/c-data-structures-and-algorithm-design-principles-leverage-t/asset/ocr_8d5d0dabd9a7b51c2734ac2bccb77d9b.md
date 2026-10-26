# Exercise 14: Hash Table with Chaining

In this exercise, we shall implement a hash table and use chaining to handle collisions. Let's get started:

1. First, let's include the required headers:

```
#include <iostream>
#include <vector>
#include <list>
#include <algorithm>
```

2. Now, let's add the hash_map class. We'll alias unsigned int to avoid writing a long name:

```
using uint = unsigned int;
class hash_map
{
    std::vector<std::list<int>> data;
```

3. Now, let's add a constructor for hash_map that will take the size of the data or hash map:

```
public:
hash_map(size_t n)
{
    data.resize(n);
}
```

4. Let's add an insert function:

```
void insert(uint value)
{
    int n = data.size();
    data[value % n].push_back(value);
    std::cout << "Inserted " << value << std::endl;
}
```

As we can see, we are always inserting the value in the data. One alternative could be to search for the value and insert it only if the value is not present.