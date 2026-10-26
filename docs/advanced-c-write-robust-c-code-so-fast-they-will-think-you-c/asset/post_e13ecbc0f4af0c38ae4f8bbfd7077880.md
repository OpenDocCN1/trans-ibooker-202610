
What we really did not dig into was the concept of name lookup. At some point the compiler will encounter the following call to the func function:

```
func(a, b);
```

When this happens, it must associate its name with the declaration that introduced it. This process is called **name lookup**. This name lookup is true for all the items in the program (variables, namespaces, classes, functions, function templates, and templates). For the program to compile, the name lookup process for variables, namespaces and classes must produce a single declaration. However, for functions and function templates the compiler may associate multiple declarations with the same name – mainly through function overloads which may be extended to consider additional functions due to **Argument-dependent Lookup (ADL)**.

## Identifiers

As defined in the C++ standard, an **identifier** is a sequence of uppercase and lowercase Latin letters, digits, underscores, and most Unicode characters. A valid identifier must begin with a non-digit character and is arbitrarily long and case-sensitive. Every character is significant.

## Names

A **name** is used to refer to an entity or to a label. A name is of one of the following forms:

- An identifier
- An overloaded operator name in function notation (e.g. operator-, operator delete)
- A template name followed by its argument list (vector<int>)
- A user-defined conversion function name (operator float)
- A user-defined literal operator name (operator ""_ms)

Every entity and its name is introduced by a declaration while a name for a label is introduced by either a **goto** statement or by a labeled statement. A name can be used multiple times in one file (or translation unit) to refer to different entities depending on the scope. A name may also be used to refer to the same entity across multiple files (translation units), or different entities, depending upon the linkage. The compiler uses name lookup to associate a declaration that introduces a name with an unknown name in the program through **name lookup**.

## Name lookup

The name lookup process is one of two and is – selected based on the context:

- **Qualified name lookup**: The name appears to the right of the scope resolution operator, `::`, or possibly after `::`, followed by the **template** keyword. A qualified name may refer to a namespace member, class member or enumerator. The name to the left of the `::` operator defines the scope to lookup the name from. If there is no name, then the global namespace is used.
- **Unqualified name lookup**: Everything else. In this case, name lookup examines the current scope and all enclosing scopes.

If the unqualified name is left of the function-call operator, `()` then it uses Argument-Dependent Lookup.

## Argument-Dependent Lookup

The set of rules for looking up unqualified function names is called **Argument-dependent lookup** (known as ADL), or **Koenig lookup** (named after Andrew Koenig, who defined it and is a longtime member of the C++ standards committee). Unqualified function names can appear as either a function-call expression or as part of an implicit function call to an overloaded operator.

ADL basically says that in addition to the scopes and namespaces considered during unqualified name lookup, the "associated namespaces" of all the arguments and template parameters are also considered. Consider the following code:

```
#include <iostream>

#include <string>

int main()
{
    std::string welcome{"Hello there"};
    std::cout << welcome;
    endl(std::cout);
}
```

## Caveat Emptor

We have seen how ADL makes life easier for the programmer by including namespaces related to the function argument types. However, this lookup capability does not come without risk which, for the most part we can minimize. Consider the following sample code:

```
#include <iostream>

namespace mylib
{
void is_substring(std::string superstring, std::string substring)
{
    std::cout << "mylib::is_substring()\n";
}

void contains(std::string superstring, const char* substring) {
    is_substring(superstring, substring);
}
}

int main() {
    mylib::contains("Really long reference", "included");
}
```

When we compile and run the preceding program, we get the expected output:

```
$ ./adl_caveat.exe
mylib::is_substring()
$
```

Figure 3.36: ADL Sample program output

The C++ standards committee then decides that it should introduce an **is_substring()** function that looks like this:

```
namespace std {
    void is_substring(std::string superstring, const char* substring)
    {
        std::cout << "std::is_substring()\n";
    }
}
```

If we add this to the top of our file, compile it and re-run it, we now get the following output:

```
$ ./adl_caveat.exe
std::is_substring()
$
```

Figure 3.37: ADL issue program output

Thanks to ADL, the (next C++ standard) compiler has picked up a different implementation as a better fit for the unqualified function call of **is_substring()**. And because of implicit conversion of arguments it does not clash which would cause an ambiguity and a compiler error. It just silently adopts to the new method which could lead to subtle and hard to find bugs if the argument order is different. The compiler can only detect type and syntactic differences, not semantic ones.

**Note**

For the purposes of demonstrating how ADL works, we have added our function to the std namespace. Namespaces serve a purpose of separating concerns, and to add into someone else's namespace, in particular, the **Standard Library namespace (std)** is not good practice.

So, why caveat emptor (buyer beware)? If you use third-party libraries for your development (and that includes the C++ Standard Library), then when you upgrade the library, you need to ensure that changes to the interface are not going to cause you problems because of ADL.

## Exercise 5: Implementing Templates to Prevent ADL Issues

In this exercise, we will demonstrate a breaking change in C++17 STL that can potentially cause an issue in the wild. C++11 introduced templates for `std::begin(type)` and friends. As a developer, this is an appealing expression of the general interface and you may have written your own versions for size(type) and empty(type) already. Follow these steps to implement this exercise:

- 1. Open the **Lesson3** project in Eclipse. Then in the **Project Explorer**, expand **Lesson3**, then **Exercise05**, and double-click on **Exercise5.cpp** to open the file for this exercise into the editor.
- 2. Click on the **Launch Configuration** drop down menu and select **New Launch Configuration...**. Configure the **L3Exercise5** application from the Search Project menu so that it runs with the name **L3Exercise5**.
- 3. Click on the **Run** button to run Exercise 5. This will produce the following output:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_282_0.png)

Figure 3:38: Successful execution of Exercise 5
- 4. An examination of the code reveals two helper templates:

```
template<class T>
bool empty(const T& x)
{
    return x.empty();
}

template<class T>
int size(const T& x)
{
    return x.size();
}
```

## Implicit Conversion

In determining the Candidate Set of Function in Figure 3.36, Function overload resolution, the compiler has to look at all of the available functions that have been found during the name lookup and determine whether the argument number and types match the call point. In determining whether the type matches, it will also examine all the available conversions to determine whether there is a mechanism to convert from the type T1 type (the type of the argument passed) to the T2 type (the type specified for the function parameter). If it can convert from T1 to T2 for all arguments, then it will add the function to the set of candidates.

This transformation from type T1 to type T2 is known as implicit conversion and occurs when some type, T1, is used in an expression or context that does not accept that type but accepts some other type, T2. This occurs in the following contexts:

- T1 is passed as an argument when calling a function declared with T2 as the parameter.
- T1 is used as an operand to an operator that expects T2.
- T1 is used to initialize a new object of T2 (including return statements).
- T1 is used in a switch statement (in which case, T2 is an int).
- T1 is used in an if statement or do-while or while loop (where T2 is bool).

If an unambiguous conversion sequence from T1 to 2 exists, then the program will compile. The conversions between built-in types are usually determined by the usual arithmetic conversions.

## Explicit – Preventing Implicit Conversion

Implicit conversion is a great feature that, make it possible for a programmer to express their intent, and it just works most of the time. However, the compiler's ability to convert one type into another without the programmer providing a hint, is not always desirable. Consider the following small program:

```
#include <iostream>
```

```
class Real
{
public:
```
