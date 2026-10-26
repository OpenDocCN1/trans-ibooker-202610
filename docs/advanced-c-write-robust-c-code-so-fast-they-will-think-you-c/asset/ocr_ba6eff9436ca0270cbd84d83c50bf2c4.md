```
```cpp
{
    std::cout << "function1 called\n";
}
int main()
{
    std::cout << "\n\n----- Function Pointers -----\n";
    FnPtr fn{function1};

    fn();

    std::cout << "Complete.\n";
    return 0;
}
```
```

This program when compiled and executed, produces the following output:

![](bbox=[0.357, 0.48, 0.677, 0.539])

Figure 4.7: Function Pointer Program output

Strictly speaking the code should be modified as follows:

```
```cpp
FnPtr fn{&function1};

if(fn != nullptr)
    fn();
```
```

First, thing to note is that the address of (&) operator should be used to initialize the pointer. Secondly, we should check that the pointer is valid before calling it.

```
```cpp
#include <iostream>

using FnPtr = void (*)(void);

struct foo
{
```
```