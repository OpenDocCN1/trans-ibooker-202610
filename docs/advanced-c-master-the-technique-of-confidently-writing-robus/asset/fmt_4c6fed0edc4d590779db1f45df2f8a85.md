
## Move Constructor and Move Assignment

*   **Move Constructor** (`Fraction::Fraction(Fraction&&)`): This is a new type of constructor that was introduced in C++11 where the first parameter is a form of `Fraction&&` and there are no other arguments, or the remainder of the arguments have default values. The form of the first argument is one of `Fraction&&`, `const Fraction&&`, `volatile Fraction&&`, or `const volatile Fraction&&`. The compiler provides a `non-explicit` `public` `inline` member function, usually with the form of `Fraction::Fraction(Fraction&&)`, that moves each of the member variables in the order of initialization.

*   **Move Assignment** (`Fraction& Fraction::operator=(Fraction&&)`): This is a new type of assignment operator that was introduced in C++11 and is a member function with the name `operator=`, and the first argument is any of the forms that are allowed for the move constructor. The compiler provides a `public` `inline` member function, usually with the form of `Fraction::Fraction(Fraction&&)`, that copies each of the member variables in the order of initialization.

Except for the `Default Constructor`, these functions deal with managing the resources owned by this class – that is, how to copy/move them and how to dispose of them. On the other hand, the `Default Constructor` is more like any other constructor that takes values – it only initializes the resources.

We can declare any of these special functions, force them to be defaulted (that is, have the compiler generate the default version), or force them to not be created. There are also rules about when these are automatically generated in the presence of the other special functions. The first four functions are relatively straightforward conceptually, but the two "move" special member functions take some additional explanation. We will deal with what is known as move semantics in detail in "Chapter 3", "The Distance Between Can and Should – Objects, Pointers, and Inheritance", but for now it is essentially what it indicates – it moves something from one object to the other.

## Implicit Versus Explicit Constructors

The preceding description talks about the compiler generating implicit or non-explicit constructors. If a constructor exists that can be called with one argument, for example, a `Copy Constructor`, or a `Move Constructor`, by default, the compiler is allowed to invoke it if necessary so that it can convert it from one type to another type, allowing an expression, function call, or assignment to be encoded. This is not always a desired behavior, and we may wish to prevent the implicit conversion and ensure that if the user of our class really wants that conversion, then they have to write it out in the program. To achieve this, we prefix the declaration of the constructor with the `explicit` keyword, like so:

```
explicit Fraction(int numerator, int denominator = 1);
```

The `explicit` keyword can be applied to other operators as well, where the compiler may use it for type conversion.

## Class Special Member Functions – Compiler Generation Rules

Firstly, the `Default Constructor` will not be generated if we declare any other form of constructor – `default`, `copy`, `move`, or `user-defined`. None of the other special member functions affect its generation.

Secondly, a `Destructor` will not be generated if a destructor is declared. None of the other special member functions affect its generation.

The generation of the other four special functions is dependent on the presence of the declaration of the `destructor` or one of the other special functions, as shown in the following table:

![](image/C14583_02A_34.jpg)

**Figure 2A.34: Special Member Function Generation Rules**

## Defaulting and Deleting Special Member Functions

Prior to C++11, if we wanted to prevent the use of a `copy constructor` or `copy assignment` member function, then we had to declare the function as `private` and not provide a definition of the function:

```cpp
class Fraction
{
public:
  Fraction();
private:
  Fraction(const Fraction&);
  Fraction& operator=(const Fraction&);
};
```

In this manner, we ensured that if anyone tried to access the `copy constructor` or `copy assignment` from outside the class, then the compiler would generate an error stating that the function is not accessible. This still declares the functions, and they are accessible from within the class. It is an effective means, but not perfect, to remove these special member functions from use.

But we can do better since C++11 introduced two new declaration forms that allow us to override the default behavior of the compiler, as defined in the preceding rules.

Firstly, we can force the compiler to not generate the method by declaring it with the `= delete` suffix, like so:

`Fraction(const Fraction&) = delete;`

> **Note**
> We can omit the name of the parameter if it is not used. This is true for any function or member function. In fact, depending on the warning level that's been set for the compiler, it may even generate a warning that the argument is not being used.

Alternatively, we can force the compiler to generate its default implementation of a special member function by using the `= default` suffix, like so:

`Fraction(const Fraction&) = default;`

If this is only the declaration of the function, then we can also omit the name of the argument. Although, good practice dictates that we should name the argument to indicate its use. That way, the users of our classes do not need to look at the implementation of calling the function.

> **Note**
> Declaring a special member function using the `default` suffix is deemed to be a `user-defined` member function for the purpose of the aforementioned rules.

## Rule of Three/Five and Rule of Zero

As we discussed previously, with the exception of the `Default Constructor`, the special member functions deal with the semantics of managing the resources owned by this class – that is, how to copy/move them and how to dispose of them. This leads to two "rules" within the C++ community about dealing with the special functions.

Prior to C++11, there was the `Rule of Three`, which dealt with the `copy constructor`, `copy assignment operator`, and the `destructor`. It basically states that we need to implement one of these methods since the management of the encapsulated resource is non-trivial.

With the introduction of the `move constructor` and `move assignment operator` in C++11, this rule was expanded to the `Rule of Five`. No changes were made to the essence of the rule. Simply, the number of special member functions increased to five. Remembering the rules for compiler generation, there is an additional reason for ensuring all five special methods are implemented (or forced via `= default`), and that is, if the compiler does not have access to a `move-semantic` function, it will attempt to use a `copy-semantic` function, and this may not be what is desired.

> **Note**
> For more details, see the `C.ctor: Constructors, assignments, and destructors` section of the C++ Core Guidelines, which can be found here: [http://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines](http://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines).

## Constructors – Initializing the Object

The main task of a constructor is to put the object into a stable state so that any operations performed on or by the object through its member functions result in a consistent defined behavior. While the previous statement is true of the `copy` and `move constructors`, they achieve this by different semantics (copying or moving from another object).
