+   10. Now, change the one occurrence of `IntVector::iterator` to `IntVectorIter`.
+   11. Click on the **Run** button. The output should be the same as before.

In this exercise, there appears to be little difference between typedef and using alias. In either case, the use of a well-named alias makes code easier to read and understand. When more complicated aliases are involved, **using** produces an easier way to write an alias. Introduced in C++11, **using** is now the preferred method for defining an alias. It has other advantages over **typedef**, such as being able to use it inside templates.

# Templates – More than Generic Programming

Templates can also deliver much more than just generic programming (a cookie cutter with types). In the case of generic programming, the template operates as a blueprint that cannot be changed and delivers the compiled version of the template for the specified type or types.

Templates can be written to provide specialization of a function or algorithm based on the types involved. This is known as **template specialization** and is not generic programming in the sense we previously used it. It can only be called generic programming when it makes certain types behave as we would expect them to do in a given context. It cannot be called generic programming when the algorithm that's used for all types is modified.

Examine the following sample of specialization code:

```
```cpp
#include <iostream>

#include <type_traits>

template <typename T, std::enable_if_t<sizeof(T) == 1, int> = 0>
void print(T val)
{
    printf("%c\n", val);
}

template <typename T, std::enable_if_t<sizeof(T) == sizeof(int), int> = 0>
void print(T val)
{
    printf("%d\n", val);
}
```
```