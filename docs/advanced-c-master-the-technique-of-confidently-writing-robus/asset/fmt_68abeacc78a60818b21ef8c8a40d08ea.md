
## Template Type Deduction: Case 3 — Rvalue Reference (`T&&`)

*   **`const int& rx`**: `T` is `int` (reference is ignored, `const` is in the pattern, leaving `int`), while the parameter's type is **`const int&`**.

If we attempt to compile the following, what do we expect? Normally, an array decays to a pointer:

```cpp
int ary[15];
function(ary);
```

The compiler error is as follows:

![Figure 2B.28: Compiler error showing the deduced type for the array argument when passed by reference](image/C14583_02B_28.jpg)

This time, the array is captured as a reference and the size is also included. So, if `ary` was declared as **`ary[10]`**, then a completely different function would result. Let's revert the template to the following:

```cpp
template<typename T>
void function(T parameter)
{
    TypeDisplay<T> type;
}
```

If we attempt to compile the array call, then the error reports the following:

![Figure 2B.29: Compiler error showing the deduced type for the array argument when passed by value](image/C14583_02B_29.jpg)

We can see that, in this case, the array has decayed as the usual behavior when passing an array to a function. We saw this behavior when talking about *Non-Type Template Arguments*.

### Value Categories

`T&&` is called an rvalue reference while `T&` is called an lvalue reference. C++ not only characterizes an expression by type but also by a property called the **value category**. These categories control expression evaluation in the compiler, including the rules for creating, copying, and moving temporary objects. There are five expression value categories defined in the C++17 standard that have the following relationships:

![Figure 2B.30: C++ value categories](image/C14583_02B_30.jpg)

The definitions of each are as follows:

*   An expression that determines the identity of an object is a **glvalue**.
*   An expression whose evaluation initializes an object or the operand of an operator is a **prvalue**. Examples include a literal (except string literal) such as `3.1415`, `true` or `nullptr`, the `this` pointer, post-increment, and post-decrement expressions.
*   A glvalue object that has resources and can be reused (because its life is ending) is an **xvalue**. Examples include a function call whose return type is an rvalue reference to an object, such as **`std::move()`**.
*   A glvalue that is not an xvalue is an **lvalue**. Examples include the name of a variable, a function or a data member, or a string literal.
*   A prvalue or xvalue is an **rvalue**.

It does not matter if you do not fully understand these for the explanation that follows – just know that an expression that is considered to be an lvalue can have its address taken (using the address-of operator, that is, `&`). The type deduction rules for the following require that you know what an lvalue is, as well as what it isn't:

```cpp
template<typename T>
void function(T&& parameter)
{
    TypeDisplay<T> type;
}
```

The type deduction rules for this `ParamType` form are as follows:

*   If the expression is an lvalue reference, then both `T` and `ParamType` are deduced to be an lvalue reference. This is the only scenario where the type is deduced to be a reference.
*   If the expression is an rvalue reference, then the rules for Case 2 apply.

## SFINAE Expression and Trailing Return Types

C++11 introduced a feature called **trailing return types** to provide a mechanism for templates so that they can generalize the return type. A simple example is as follows:

```cpp
template<class T>
auto mul(T a, T b) -> decltype(a * b)
{
    return a * b;
}
```

Here, **`auto`** is used to indicate that a trailing return type is defined. The trailing return type starts with the **`->`** pointer and in this case, the return type is the type that's returned by multiplying **`a`** and **`b`**. The compiler will process the content of the `decltype` and, if it is malformed, it will remove the definition from the function name's lookup, as per usual. This capability opens up many possibilities as the comma operator, **`,`**, can be used inside `decltype` to check for certain attributes.

If we want to test that a class implements a method or contains a type then we can place this inside the `decltype` by converting it into a `void` (in case the comma operator has been overloaded) and then define an object of the real return type at the end of the comma operator. An example of this is shown in the following program:

```cpp
#include <iostream>
#include <algorithm>
#include <utility>
#include <vector>
#include <set>

template<class C, class T>
auto contains(const C& c, const T& x)
    -> decltype((void)(std::declval<C>().find(std::declval<T>())), true)
{
    return end(c) != c.find(x);
}

int main(int argc, char** argv)
{
    std::cout << "\n\n------ SFINAE Exercise ------\n";
    std::set<int> mySet {1,2,3,4,5};
    std::cout << std::boolalpha;
    std::cout << "Set contains 5: " << contains(mySet, 5) << "\n";
    std::cout << "Set contains 15: " << contains(mySet, 15) << "\n";
    std::cout << "Complete.\n";
    return 0;
}
```

When this program is compiled and executed, we obtain the following output:

![Figure 2B.31: Output from the SFINAE expression](image/C14583_02B_31.jpg)

The return type is given by the following code:

```cpp
decltype( (void)(std::declval<C>().find(std::declval<T>())), true)
```

Let's break this down:

*   The operand of **`decltype`** is a comma-separated list of expressions. This means that the compiler will construct but not evaluate the expressions and use the type of the right-most value to determine the return type for the function.
*   **`std::declval<T>()`** allows us to convert the `T` type into a reference type that we can then use to access member functions without having to actually construct an object.
*   As with all SFINAE-based operations, if any expression in the comma-separated list is invalid, then the function is discarded. If they are all valid, then it is added to the list of functions for lookup.
*   The cast to `void` is to prevent any problems that may arise if the user overloads the comma operator.
*   Basically, this is testing whether the **`C`** class has a member function called **`find()`** that takes **`class T`**, **`class T&`**, or **`const class T&`** as an argument.

This method will work for **`std::set`**, which has a **`find()`** method that takes one argument but will fail for other containers because they do not have a **`find()`** member method.

This approach works well if we are only dealing with one type. But if we have a function that needs to produce different implementations based on type, as we have seen before, the **`if constexpr`** approach is much cleaner and generally easier to understand. To use the **`if constexpr`** approach, we need to produce templates that will evaluate to **`true`** or **`false`** at compile time. The standard library provides helper classes for this: **`std::true_type`** and **`std::false_type`**. These two structures have a static constant member named `value` set to **`true`** and **`false`**, respectively. Using SFINAE and template overloads, we can create new detection classes that derive from either of these classes to give us the desired outcome:

```cpp
template <class T, class A0>
auto test_find(long) -> std::false_type;

template <class T, class A0>
auto test_find(int)
    -> decltype(void(std::declval<T>().find(std::declval<A0>())), std::true_type{});

template <class T, class A0>
struct has_find : decltype(test_find<T, A0>(0)) {};
```

The first template for **`test_find`** creates the default behavior that will set the return type to **`std::false_type`**. Note that this has an argument type of **`long`**.
