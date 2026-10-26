```
```cpp
merged.emplace_back(*iter1);
}
else
{
    for (; iter2 != arr2.end(); iter2++)
        merged.emplace_back(*iter2);
}

return merged;
}
```
```

The templatized **merge()** function takes in references to two vectors of type T and returns a new vector containing the elements in input arrays, but sorted in increasing order.

+   3. We can now use the merge operation to write a recursive merge sort implementation, as shown here:

```
```cpp
template <typename T>
std::vector<T> merge_sort(std::vector<T> arr)
{
    if (arr.size() > 1)
    {
        auto mid = size_t(arr.size() / 2);
        auto left_half = merge_sort<T>(std::vector<T>(arr.begin(), arr.begin() + mid));
        auto right_half = merge_sort<T>(std::vector<T>(arr.begin() + mid, arr.end()));

        return merge<T>(left_half, right_half);
    }

    return arr;
}
```
```