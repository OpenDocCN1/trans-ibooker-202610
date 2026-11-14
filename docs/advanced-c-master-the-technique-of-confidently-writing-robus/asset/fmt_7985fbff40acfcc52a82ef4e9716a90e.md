
If we had to write it every time we wished to multiply two matrices, we would end up with many nested for loops. But if we could abstract a matrix into a class, then we could express it as simple as we express the multiplication of two integers or two floating-point numbers:

```cpp
Matrix a;

Matrix b;

// Code to initialize the matrices

auto c = a * b;
```

This is the beauty of object-oriented design – data encapsulation and the abstraction of concepts are explained at such a level that we can easily understand what the program is trying to achieve without getting buried in the details. Once we have established that the matrix multiplication is correctly implemented, then we are free to focus on solving our problem at a higher level.

The discussion that follows refers to classes, but it applies equally to structs and, mostly, to unions. We will outline the differences between classes, structs, and unions after we learn how to define and use a class.

## Fraction Class

To show you how to define and use a class, we will work on developing the `Fraction` class to implement rational numbers. When defined, we can use `Fraction` like any other built-in type (add, subtract, multiply, divide) without worrying about the details – this is abstraction. We can now just think and reason about a fraction at a higher level, that is, an abstract level.

The `Fraction` class will do the following:

*   Contain two integer member variables, `m_numerator` and `m_denominator`
*   Provide methods to copy itself, assign to it, multiply, divide, add, and subtract
*   Provide a method to write to an output stream

To attain the preceding objective, we have the following definitions:

![Figure 2A.32: Definitions of operations](image/C14583_02A_32.jpg)

In addition, the operations that we perform will need to normalize the fraction by reducing it to the lowest terms. To do this, the numerator and the denominator are both divided by their greatest common divisor (GCD).

## Constructors, Initialization, and Destructors

A class definition, as expressed in C++ code, is the pattern that's used for creating objects in memory and manipulating objects through their methods. The first thing that we need to do is tell the compiler that we wish to declare a new type – a class. To declare the `Fraction` class, we start with the following:

```cpp
class Fraction
{
};
```

We place this in a header file, `Fraction.h`, as we wish to reuse this class specification in other areas of our code.

The next thing we need to do is introduce the data to be stored in the class, which in this case are `m_numerator` and `m_denominator`. These are both of the `int` type:

```cpp
class Fraction
{
  int m_numerator;
  int m_denominator;
};
```

We have now declared the data to be stored and given them names that anyone familiar with mathematics would understand regarding what each member variable stores:

![](image/C14583_02A_33.jpg)

As this is a class, by default, any item that's declared is assumed to be `private`. This means that no external entity can access these variables. It is this feature of hiding (making data private, and for that matter, some methods) that enables encapsulation in C++. C++ has three class access modifiers:

*   `public`: This means that the member (variable or function) is accessible from anywhere outside of the class.
*   `private`: This means that the member (variable or function) cannot be accessed from outside the class. In fact, it cannot even be viewed. Private variables and functions can only be accessed from within the class or by a friend method or class. The private members (variables and functions) are used by the public functions to achieve the desired functionality.
*   `protected`: This is a cross between private and public. From outside the class, the variable or function is private. However, for any class that is derived from the class that declares the protected members, they are treated as public.

At this point in our definition of the class, this is not very useful. Let's change the declaration to the following:

```cpp
class Fraction
{
public:
  int m_numerator;
  int m_denominator;
};
```

By doing this, we can access the internal variables. The `Fraction number;` variable declaration will cause the compiler to do two things:

*   Allocate enough memory to hold both the data items (depending on the types, this may or may not involve padding, that is, the inclusion or addition of unused memory to align members for the most efficient access). The `sizeof` operator can tell us how much memory is allocated for our class.
*   Initialize the data items by calling the `default constructor`.

These steps are the same that the compiler does for built-in types, that is, step 2 does nothing, leading to uninitialized variables. But what is this default constructor? And what does it do?

Firstly, the default constructor is a special member function. It is one of the many possible constructors, three of which are deemed special member functions. Constructors can be declared with zero, one, or more arguments, just like any other function, but they do NOT specify a return type. Constructors serve the special purpose of initializing all the member variables to place the object into a well-defined state. If the member variables are themselves a class, then it may not be necessary to specify how to initialize the variable. If the member variable is of a built-in type, then we will need to provide the initial values for them.

## Class Special Member Functions

When we define a new type (struct or class), the compiler will create up to six (6) special member functions for us:

*   **Default Constructor** (`Fraction::Fraction()`): This is called when no arguments are provided (such as in the preceding section). This can be achieved by either having no argument list for the constructor or by defining a default value for all the arguments, such as `Fraction(int numerator=0, denominator=1)`. The compiler provides an `implicit` `inline` default constructor that performs the default initialization of member variables – for built-in types, this means do nothing.
*   **Destructor** (`Fraction::~Fraction()`): This is a special member function that is called when the object's lifetime ends. Its purpose is to release any resources that were allocated and kept by the object during its lifetime. The compiler provides a `public` `inline` member function that calls the destructors of the member variables.
*   **Copy Constructor** (`Fraction::Fraction(const Fraction&)`): This is another constructor where the first parameter is a form of `Fraction&` and there are no other arguments, or the remainder of the arguments have default values. The form of the first argument is one of `Fraction&`, `const Fraction&`, `volatile Fraction&`, or `const volatile Fraction&`. We will deal with `const` later but not with `volatile` in this book. The compiler provides a `non-explicit` `public` `inline` member function, usually with the form of `Fraction::Fraction(const Fraction&)`, that copies each of the member variables in the order of initialization.
*   **Copy Assignment** (`Fraction& Fraction::operator=(Fraction&)`): This is a member function with the name `operator=` and the first argument is either a value or any of the reference types of the class, in this case, `Fraction`, `Fraction&`, `const Fraction&`, `volatile Fraction&`, or `const volatile Fraction&`. The compiler provides a `public` `inline` member function, usually with the form of `Fraction::Fraction(const Fraction&)`, that copies each of the member variables in the order of initialization.
