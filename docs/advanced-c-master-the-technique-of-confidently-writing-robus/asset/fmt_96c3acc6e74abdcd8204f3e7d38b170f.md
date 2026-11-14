
# Stack Unwinding and Exception Handling Exercise

The process of destroying all local function variables, as guaranteed by the C++ language, is known as **Stack Unwinding**. As the stack unwinds in the presence of an exception, C++ uses its well-defined rules to destroy all the objects in the scope.

When an exception occurs, the function call stack starts searching linearly from the current function back to the function that called it, to the one that called that, and so on, until an exception handler (expressed by a `catch` block) that matches the exception is found.

If an exception handler is found, then the stack unwinding occurs, destroying all the local variables for all the functions in the stack. The objects are destroyed in the reverse order that they were created. If no handler is found to deal with the thrown exception, then the program terminates (usually without warning the user).

## Exercise 1: Implementing Exceptions in `Fraction` and `Stack`

In this exercise, we will return to two classes that we worked on in *Chapter 2A*, *No Ducks Allowed – Types and Deduction* and *Chapter 2B*, *No Ducks Allowed – Templates and Deduction* – `Fraction` and `Stack`, both of which can experience runtime anomalies. We are going to update their code so that they can raise exceptions whenever any problem is detected. Follow these steps to implement this exercise:

1.  Open Eclipse and create a new project named `Lesson3` using the files found in the `Lesson3` examples folder.

2.  As this is a **CMake-based project**, change the current builder to be **CMake Build (portable)**.

3.  Go to the **Project** | **Build All** menu to build all the exercises. By default, the console at the bottom of the screen will display the **CMake Console [Lesson3]**.

4.  Configure a new **Launch Configuration**, `L3Exercise1` to run with the name `Exercise1`.

5.  Also, configure a new C/C++ Unit Run Configuration, `L3Ex1Tests`, to run `L3Ex1tests`. Set the **Google Tests Runner**.

6.  Click on the **Run** option for the existing **18** tests to run and pass.

    ![Figure 3.12: Existing tests all pass (Runs: 18)](image/C14583_03_12.jpg)

    *Figure 3.12: Existing tests all pass (Runs: 18)*

7.  Open `Fraction.hpp` in the editor and change the line at the top of the file to read like so:

    ```
    #define EXERCISE1_STEP  14
    ```

8.  Click on the **Run** button to re-run the tests – we have added one test that will attempt to create a `Fraction` with a zero denominator. The test expects that an exception has been thrown:

    ![Figure 3.13: New failing test ThrowsDomainErrorForZeroDenominator](image/C14583_03_13.jpg)

    *Figure 3.13: New failing test ThrowsDomainErrorForZeroDenominator*

9.  Click on the failing test name – the **Messages** window will now show the expected behavior and the actual behavior. You may need to scroll to the right to read it all. At the very right it indicates "**Expected … throws an exception of type `std::domain_error`**" and the next line states "**Actual: it throws nothing**".

10. Double-click on the message and it will take you to the following test:

     ![Figure 3.14: The failing test](image/C14583_03_14.jpg)

     *Figure 3.14: The failing test*

     The `ASSERT_THROW()` macro requires two arguments. Since the `Fraction` initializer has a comma in it, it is necessary to wrap the whole first argument in an extra set of parentheses. The second argument is expected to get a `std::domain_error` from this constructor. The internal `try/catch` structure is present to confirm that the expected string is captured inside the exception object. If we do not want to check this, then we could simply write the test like so:

     ```
     ASSERT_THROW(({Fraction f1{1,0}; }), std::domain_error);
     ```

11. Open the file `Fraction.cpp` in the editor. Insert the following line near the top of the file:

     ```
     #include <stdexcept>
     ```

12. Modify the constructor to throw an exception if it's been created with a zero denominator:

     ```
     Fraction::Fraction(int numerator, int denominator)
         : m_numerator{numerator}, m_denominator{denominator}
     {
         if(m_denominator == 0)
         {
             throw std::domain_error("Zero Denominator");
         }
     }
     ```

13. Click on the **Run** button to re-run the tests. **19** tests now pass.

14. Open `Fraction.hpp` in the editor and change the line near the top of the file to read like so:

     ```
     #define EXERCISE1_STEP  20
     ```

15. Click on the **Run** button to re-run the tests – the new test `ThrowsRunTimeErrorForZeroDenominator` fails.

16. Click on the failing test name – the **Messages** window will now show the expected behavior and the actual behavior. You may need to scroll to the right to read it all. At the very right it indicates "**Expected… throws an exception of type `std::runtime_error`**" and the next line states "**Actual: it throws a different type**".

17. Double-click on the message again to open the failing test:

     ![Figure 3.15: Another failing test](image/C14583_03_15.jpg)

     *Figure 3.15: Another failing test*

     This test is verifying that the division assignment operator will throw an exception for a divide by zero.

18. Open `Fraction.cpp` and locate the `operator/=()` function. You'll see that, inside this function, it actually uses the constructor for `Fraction`, so it will throw a `std::domain_error`.

19. Now modify `operator/=()` to detect this problem before the constructor is called so that it throws a `std::runtime_error` with the expected message.

20. Modify `Fraction.cpp` by adding a domain error that will detect the division operator:

     ```
     Fraction& Fraction::operator/=(const Fraction& rhs)
     {
         if (rhs.m_numerator == 0)
         {
             throw std::runtime_error("Fraction Divide By Zero");
         }
         Fraction tmp(m_numerator*rhs.m_denominator,
                      m_denominator*rhs.m_numerator);
         *this = tmp;
         return *this;
     }
     ```

21. Click on the **Run** button to re-run the tests. All **20** tests pass.

22. Open `Stack.hpp` in the editor and change the line near the top of the file to read like so:

     ```
     #define EXERCISE1_STEP  27
     ```

23. Click on the **Run** button to re-run the tests – we have added one test that will attempt to pop from an empty stack. In the **C/C++ Unit tab** window, click on the triangle next to `FractionTest` to collapse the lists of tests and show the `StackTest`:

     ![Figure 3.16: pop Stack test fails](image/C14583_03_16.jpg)

     *Figure 3.16: pop Stack test fails*

24. Use the **C/C++ Unit** window to click through and locate the failing test. Determine the expected exception and then open `Stack.hpp`. Add `#include <stdexcept>` to the top of the file and then update the `pop()` function so that it looks like this:

     ```
     void pop()
     {
         if(empty())
             throw std::underflow_error("Pop from empty stack");
         m_stack.pop_back();
     }
     ```

25. Click on the **Run** button to re-run the tests. **21** tests now pass.

26. Open `Stack.hpp` in the editor and change the line at the top of the file to read like so:

     ```
     #define EXERCISE1_STEP  31
     ```

27. Click on the **Run** button to re-run the tests – the newly added test, `TopEmptyStackThrowsUnderFlowException`, fails.

28. Use the **C/C++ Unit** window to click through and locate the failing test. Determine the expected exception and then open `Stack.hpp`. Update the non-const `top()` method so that it looks as follows:

     ```
     reference top()
     {
         if(empty())
             throw std::underflow_error("Top from empty stack");
         return m_stack.back();
     }
     ```

29. Click on the **Run** button to re-run the tests. **22** tests pass.

30. Open `Stack.hpp` in the editor and change the line at the top of the file to read like so:

     ```
     #define EXERCISE1_STEP  35
     ```
