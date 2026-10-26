This example would produce the following output:

```
First element: 1
Elements in second array: 1 2 3 4
```

As we can see, std::array provides operator[], which is same as the C-style array, to avoid the cost of checking whether the index is less than the size of the array. Additionally, it also provides a function called at(index), which throws an exception if the argument is not valid. In this way, we can handle the exception in an appropriate manner. So, if we have a piece of code where we will be accessing an element with a bit of uncertainty, such as an array index being dependent on user input, we can always catch the error using exception handling, as demonstrated in the following example.

```
try
{
    std::cout << arr.at(4);    // No error
    std::cout << arr.at(5);    // Throws exception std::out_of_range
}
catch (const std::out_of_range& ex)
{
    std::cerr << ex.what();
}
```

Apart from that, passing std::array to another function is similar to passing any built-in data type. We can pass it by value or reference, with or without const. Additionally, the syntax doesn't involve any pointer-related operations or referencing and de-referencing operations. Hence, the readability is much better compared to C-style arrays, even for multidimensional arrays. The following example demonstrates how to pass an array by value:

```
void print(std::array<int, 5> arr)
{
    for(auto ele: arr)
    {
        std::cout << ele << ", ";
    }
}
```

```
std::array<int, 5> arr = {1, 2, 3, 4, 5};
print(arr);
```