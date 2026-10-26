+   8. In the editor, move the `std::logic_error` handler to just after the `std::domain_error` handler. Click on the **Run** button. This time, it will report that the `std::domain_error` handler was executed which is actually what we would expect.
+   9. Now change the `throw` line to be a `std::logic_error` exception. Click on the **Run** button. This time, it will report that `std::logic_error` handler was executed as expected.
+   10. Now change the `throw` line to be a `std::underflow_error` exception. Click on the **Run** button and this time it will report that the exception was caught by the `std::exception` handler as expected. `std::exception` is the base class for all Standard Library exceptions.

In this exercise, we implemented a series of exception handlers and observed how the order of exception handlers affects the way an exception is caught and how the exception hierarchy can be used.

# CMake Generator Expressions

When working with **CMake**, it is sometimes necessary to adjust the value of a variable. **CMake** is a build generator system that can generate build files for many build tools and compiler tool chains. Because of this flexibility, if you want to turn on some feature in the compiler, you only need to apply to it to one particular type. This is because the command-line options are different between vendors. For example, the command line option for the g++ compiler to enable C++17 support is **-std=c++17**, but for **msvc** it is **/std:c++17**. If you open the **CMakeLists.txt** file and locate **L3Exercise2 add_executable**, then the following line will be after it:

```
target_compile_options(L3Exercise2 PRIVATE $<$<CXX_COMPILER_ID:GNU>:-fpermissive>)
```

This uses the **$<CXX_COMPILER_ID:GNU>** variable query to check whether it is a GCC compiler. If yes, then it generates a 1 (true), otherwise 0 (false). It also uses the **$<condition:true_string>** condition expression to add **-fpermissive** to the compiler options for the **L3Exercise2** target, but only for the gcc compiler. These can be added for each compiler type as separate calls to **target_compile_options** or via one call.

> **Note**
> 
> For more information on generator expressions, please take a look at the following link: https://cmake.org/cmake/help/v3.15/manual/cmake-generator-expressions.7.html.