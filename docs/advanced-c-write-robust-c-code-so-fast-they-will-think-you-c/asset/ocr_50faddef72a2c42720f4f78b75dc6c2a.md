![](bbox=[0.13, 0.07, 0.935, 0.185])

# Figure 2B.32: Initial unit test for stacks

+   5. Open **Stack.hpp** in the editor. You will find the following code:

```
#pragma once
#include <vector>
#include <cstddef>

#define EXERCISE4_STEP    1

namespace acpp
{

template <typename T>
class Stack
{
public:

private:
    std::vector<T> m_stack;
};

} // namespace acpp
```

The first thing to note with a template definition is that it must be placed in a header file that can be included where we need to reuse it. Secondly, we have used a pragma directive (**#pragma once**) which tells the compiler that if it encounters this file again to be #included, it does not need to be. While not strictly part of the standard, nearly all modern C++ compilers support it. Finally, note that, for the purpose of this exercise, we have chosen to store the items in the STL vector.

+   6. In the editor, add the following declaration in the **public** section of the **Stack** class:

```
bool empty() const
{
    return m_stack.empty();
}
```