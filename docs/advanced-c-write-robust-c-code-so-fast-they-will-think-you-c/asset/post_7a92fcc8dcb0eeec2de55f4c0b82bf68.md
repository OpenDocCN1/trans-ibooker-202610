
By examining the code and comparing it with the output, we will find that the **rvalue** variable allows us to manipulate (read from and write to) the data stored in the **value** variable. We have a reference, **rvalue**, to the **value** variable. We can also see that the values stored in the **a** and **b** variables were exchanged by the **swap()** function.

- 5. In the editor, change the function definition of swap:

```
void swap(const int& lhs, const int& rhs)
```

- 6. Click on the **Run** button. When the Errors in Workspace dialog appears, click **Cancel**. The first error that's reported by the compiler is shown here:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_146_0.png)

Figure 2A.39: Read-only error on assignment

By changing the arguments from **int& lhs** to **const int& lhs**, we've told the compiler that the arguments should not be changed inside this function. And because we assign to lhs in the function, the compiler generates the error about lhs being read-only and terminates.

## Implementing Standard Operators

To use fractions like a built-in class, we need them to work with the standard mathematical operators (+, -, *, /) and their assignment counterparts (+=, -=, *=, /=). If you are unfamiliar with assignment operators, then consider the following two expressions – they produce the same output:

```
a = a + b;
a += b;
```

The syntax for declaring these two operators for Fraction is as follows:

```
// member function declarations
Fraction& operator+=(const Fraction& rhs);
Fraction operator+(const Fraction& rhs) const;
```

```
// normal function declaration of operator+
Fraction operator+(const Fraction& lhs, const Fraction& rhs);
```

Because the operator+= method modifies the contents of the left-hand variable (adds a to b and then stores it again in a), it is recommended that it is implemented as a member function. In this case, as we have not created a new value, we can just return a reference to the existing lhs.

The operator+ method, on the other hand, should not modify lhs nor rhs and return a new object. The implementer is free to implement it as a member function or as a free function. Both are shown in the preceding code, but only one should exist. The interesting thing to note about the member function's implementation is the const keyword at the end of the declaration. This tells the compiler that when this member function is called, it will not modify the internal state of the object. While both approaches are valid, if possible, operator+ should be implemented as a normal function, outside of the class.

The same approach can be used for the other operators - (subtract), * (multiply), and / (divide). The preceding methods implement the semantics for the standard mathematical operators and make our type act like the built-in types.

## Implementing the Output Stream Operator (<<)

C++ abstracts Input/Output (I/O) into the stream class hierarchy in the standard library (which we will talk about in Chapter 2B, No Ducks Allowed - Templates and Deduction). In Exercise 5, Declaring and Initializing Fractions, we saw that we could insert the fraction into an output stream as follows:

```
std::cout << "fraction = " << fraction.getNumerator() << "/" << fraction.getDenominator() << "\n";
```

So far, for our Fraction class, we have written out the numerator and denominator values by accessing the data values from outside using the getNumerator() and getDenominator() methods, but there is a better way to do this. As part of making our classes first-class citizens in C++, where it makes sense, we should overload the I/O operators. In this chapter, we will only look at the output operator, <<, also known as the insertion operator. That way, we can replace the previous code with a much cleaner version:

```
std::cout << "fraction = " << fraction << "\n";
```

We can overload the operator as either a friend function or a normal function (if the class provides getter functions of the data that we need to insert). For our purpose, we define it as a normal function:

```
inline std::ostream& operator<< (std::ostream &out, const Fraction &rhs)
{
    out << rhs.getNumerator() << " / " << rhs.getDenominator();
    return out;
}
```

## Structuring Our Code

Before we delve into the exercise where we implement the operators and turn our Fraction into a full-blown type in the C++ world, we need to have a brief discussion about where we put the bits and pieces of our class – the declaration and the definition. The declaration is the blueprint for our class that indicates what data storage it requires and the methods that it will implement. The definition is the actual implementation details of each of the methods.

In languages such as Java and C#, the declaration and the definition are one and the same and they must exist in one file (Java) or across multiple files (C# partial classes). In C++, depending on the class and how much you wish to expose to other classes, the declaration MUST appear in a header file (which can be #included in other files for use) and the definition can appear in one of three places – inline in the definition, inline in the same file as the definition, or in a separate implementation file.

The header file is usually named with the .hpp extension, while the implementation file is typically one of *.cpp or *.cxx. The implementation file is also known as a translation unit. By defining a function as inline, we allow the compiler to optimize the code in a way that the function may not even exist in the final program – it has substituted the steps we put into the function into the locations that we call the function from.

## Exercise 7: Adding Operators to the Fraction Class

In this exercise, we aim to implement operators in our Fraction class using unit tests to develop the functionality. This makes our Fraction class a real type. Let's get started:

- 1. Open the **Lesson2A** project in Eclipse, and then in the **Project Explorer**, expand **Lesson2A**, then **Exercise07**, and double-click on **Exercise7.cpp** to open the file for this exercise in the editor.
- 2. Click on the **Launch Configuration** drop-down menu and select **New Launch Configuration....** Configure Exercise7 to run with the name Exercise7.
- 3. When that is complete, it will be the currently selected Launch Configuration.
- 4. We also need to configure a unit test. In Eclipse, click on the menu item called **Run | Run Configurations...**, right-click **C/C++ Unit** on the left-hand side, and select **New Configuration**.
- 5. Change the name from **Lesson2A Debug** to **Exercise7 Tests**.
- 6. Under **C/C++ Application**, select the **Search Project** option and choose **tests** in the new dialog.
- 7. Next, go to the **C/C++ Testing** tab and select **Google Test Runner** in the dropdown. Click on **Apply** at the bottom of the dialog and click on the **Run** option for the test, which we have to run for the first time:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_149_0.png)

Figure 2A.40: Failing tests – multiplication

- 8. Open the **Fraction.cpp** file in the editor and locate the **operator*=** function. Update it with the following code:

```
Fraction& Fraction::operator*=(const Fraction& rhs)
{
    Fraction tmp(m_numerator*rhs.m_numerator, m_denominator*rhs.m_denominator);
    *this = tmp;
    return *this;
}
```

- 9. Click on the **Run** button to rerun the tests. This time, all the tests pass:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_150_0.png)

Figure 2A.41: Passing tests

- 10. In your IDE, open the **tests/FractionTests.cpp** file and find the two tests that were failing. One tested **operator*=()**, while the other tested **operator*()**. How did fixing **operator*=()** fix **operator*()**? If you open Fraction.hpp in the editor, you will find that the **operator*()** function was implemented for you by making a call to **operator*=()**, that is, it is marked inline and is a normal function and not a member function. In general, this is the approach to take when overloading these operators – the one that modifies the object that is calling it is a member function, whereas the one that has to generate a new value is a normal function that calls the member function.

- 11. Open **Fraction.hpp** in the editor and change the line near the top of the file so that it reads like so:

```
#define EXERCISE7_STEP 11
```
