
6. Click on the Run button. When the Errors in Workspace dialog appears, click Cancel:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_127_0.png)

Figure 2A.26: Cannot convert error

By changing the argument type, we forced the compiler to enforce the type of the value that was passed to the function.

7. Call the printOrange() function twice by passing the orange enum variable in the initial call and the apple variable in the second call, respectively:

```
printOrange(orange);
printOrange(apple);
```

This shows that the compiler is implicitly converting the orange and apple into an int so that it can call the function. Also, note the warning about comparing Apple and Orange.

8. Restore the printOrange() function by taking an int argument and changing the definition of the orange enum to the following:

```
enum class Orange;
```

9. Click on the Run button. When the Errors in Workspace dialog appears, click Cancel:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_127_1.png)

Figure 2A.27: Multiple errors for scoped enum changes

10. Locate the first error listed for this build:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_128_0.png)

Figure 2A.28: First scoped enum error

11. The first thing to note about scoped enums is that when you reference the enumerators, they must have a scope specifier. Therefore, in the editor, go and change this line to the following:

```
Orange orange{Orange::Hamlin};
```

12. Click on the Run button. When the Errors in Workspace dialog appears, click Cancel. Good news – the error count dropped to 8 from 9. Check the errors in the console and locate the first one:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_128_1.png)

Figure 2A.29: Second scoped enum error

This error reports that it is not able to find an insertion operator (<<) for an ostream that knows how to handle the Orange type. Because this involves a template-based class (which we’ll talk about later), the error messages become very long-winded. Take a minute to look at all the messages that appear from this error to the next (red lines). It shows you what the compiler was trying to do to be able to compile that line.

13. Change the indicated line to read the following:

```
std::cout << "orange = " << static_cast<int>(orange) << "\n";
```

14. Click on the **Run** button. When the Errors in Workspace dialog appears, click **Cancel**. Good news – the error count dropped to 6 from 8. Check the errors in the console and locate the first one:

![](img/6507d435e2edfa3c00c220d0ccfcaf94_129_0.png)

Figure 2A.30: Third scoped enum error

This error reports (finally) that you cannot compare apples and oranges. At this point, we decide that the program is attempting to do something that makes no sense and there is no point in attempting to fix the rest. We could fix this error by casting it to an int again, but we would need to cast for the next error as well. The final error would then be that Valencia is missing the Orange:: scope specifier.

15. It is left as an exercise for you to get the file to compile again with orange as a scope enumeration.

In this exercise, we found that scope enums improve the strong type checking of C++ and if we wish to use them as an integral value, then we need to cast them, unlike non-scope enums, which are implicitly converted.

## Troubleshooting Compiler Errors

As seen from the preceding exercise, the compiler can generate a very large amount of error and warning messages from one error. This is why it is recommended to find the first error and fix it first. Development in IDEs or using build systems that color-code errors can make this easier.

## Structures and Classes

Enumerations are the first of the user-defined types, but they do not really extend the language so that we can express the solutions to our problems at the appropriate level of abstraction. Structs and classes, however, allow us to capture and group the data and then associate methods to manipulate that data in a consistent and meaningful manner.

If we consider the multiplication of two matrices, A (m x n) and B (n x p), which results in matrix C (m x p), then the equation for the ith row and jth column of C is as follows:

cᵢⱼ = Σₖ₌₁ⁿ aᵢₖ bₖⱼ

Figure 2A.31: Equation of ith row and jth column

If we had to write it every time we wished to multiply two matrices, we would end up with many nested for loops. But if we could abstract a matrix into a class, then we could express it as simple as we express the multiplication of two integers or two floating-point numbers:

```
Matrix a;

Matrix b;

// Code to initialize the matrices

auto c = a * b;
```

This is the beauty of object-oriented design – data encapsulation and the abstraction of concepts are explained at such a level that we can easily understand what the program is trying to achieve without getting buried in the details. Once we have established that the matrix multiplication is correctly implemented, then we are free to focus on solving our problem at a higher level.

The discussion that follows refers to classes, but it applies equally to structs and, mostly, to unions. We will outline the differences between classes, structs, and unions after we learn how to define and use a class.

## Fraction Class

To show you how to define and use a class, we will work on developing the Fraction class to implement rational numbers. When defined, we can use Fraction like any other built-in type (add, subtract, multiply, divide) without worrying about the details – this is abstraction. We can now just think and reason about a fraction at a higher level, that is, an abstract level.

The Fraction class will do the following:

- Contain two integer member variables, m_numerator and m_denominator
- Provide methods to copy itself, assign to it, multiply, divide, add, and subtract
- Provide a method to write to an output stream

To attain the preceding objective, we have the following definitions:

Addition  a/b + c/d = (a.d + b.c)/(b.d)  Multiplication  a/b × c/d = (a.c)/(b.d)
Subtraction  a/b - c/d = (a.d - b.c)/(b.d)  Division  a/b ÷ c/d = a/b × d/c = (a.d)/(b.c)

Figure 2A.32: Definitions of operations

In addition, the operations that we perform will need to normalize the fraction by reducing it to the lowest terms. To do this, the numerator and the denominator are both divided by their greatest common divisor (GCD).

## Constructors, Initialization, and Destructors

A class definition, as expressed in C++ code, is the pattern that's used for creating objects in memory and manipulating objects through their methods. The first thing that we need to do is tell the compiler that we wish to declare a new type – a class. To declare the Fraction class, we start with the following:

```
class Fraction
{
};
```

We place this in a header file, Fraction.h, as we wish to reuse this class specification in other areas of our code.

The next thing we need to do is introduce the data to be stored in the class, which in this case are m_numerator and m_denominator. These are both of the int type:

```
class Fraction
{
    int m_numerator;
    int m_denominator;
};
```

We have now declared the data to be stored and given them names that anyone familiar with mathematics would understand regarding what each member variable stores:

fraction = numerator / denominator

Figure 2A.33: Formula of fraction

Firstly, the default constructor is a special member function. It is one of the many possible constructors, three of which are deemed special member functions. Constructors can be declared with zero, one, or more arguments, just like any other function, but they do NOT specify a return type. Constructors serve the special purpose of initializing all the member variables to place the object into a well-defined state. If the member variables are themselves a class, then it may not be necessary to specify how to initialize the variable. It the member variable is of a built-in type, then we will need to provide the initial values for them.

## Class Special Member Functions

When we define a new type (struct or class), the compiler will create up to six (6) special member functions for us:

- **Default Constructor** (`Fraction::Fraction()`): This is called when no arguments are provided (such as in the preceding section). This can be achieved by either having no argument list for the constructor or by defining a default value for all the arguments, such as `Fraction(int numerator=0, denominator=1)`. The compiler provides an **implicit inline** default constructor that performs the default initialization of member variables – for built-in types, this means do nothing.
- **Destructor** (`Fraction::~Fraction()`): This is a special member function that is called when the object's lifetime ends. Its purpose is to release any resources that were allocated and kept by the object during its lifetime. The compiler provides a **public inline** member function that calls the destructors of the member variables.
- **Copy Constructor** (`Fraction::Fraction(const Fraction&)`): This is another constructor where the first parameter is a form of `Fraction&` and there are no other arguments, or the remainder of the arguments have default values. The form of the first argument is one of `Fraction&, const Fraction&, volatile Fraction&,` or `const volatile Fraction&`. We will deal with `const` later but not with `volatile` in this book. The compiler provides a **non-explicit public inline** member function, usually with the form of `Fraction::Fraction(const Fraction&)`, that copies each of the member variables in the order of initialization.
- **Copy Assignment** (`Fraction& Fraction::operator=(Fraction&)`): This is a member function with the name `operator=` and the first argument is either a value or any of the reference types of the class, in this case, `Fraction, Fraction&, const Fraction&, volatile Fraction&,` or `const volatile Fraction&`. The compiler provides a **public inline** member function, usually with the form of `Fraction::Fraction(const Fraction&)`, that copies each of the member variables in the order of initialization.
