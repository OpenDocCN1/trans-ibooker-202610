The explicit keyword can be applied to other operators as well, where the compiler may use it for type conversion.

# Class Special Member Functions – Compiler Generation Rules

Firstly, the **Default Constructor** will not be generated if we declare any other form of constructor – default, copy, move, or user-defined. None of the other special member functions affect its generation.

Secondly, a **Destructor** will not be generated if a destructor is declared. None of the other special member functions affect its generation.

The generation of the other four special functions is dependent on the presence of the declaration of the destructor or one of the other special functions, as shown in the following table:

| If the user declares | Copy Constructor | Copy assignment | Move Constructor | Move assignment |
|---|---|---|---|---|
| Copy Constructor | No - user defined | Yes | No | No |
| Copy assignment | Yes | No - user defined | No | No |
| Move Constructor | Deleted | Deleted | No - user defined | No |
| Move assignment | Deleted | Deleted | No | No - user defined |
| Destructor | Yes | Yes | No | No |

**Figure 2A.34: Special Member Function Generation Rules**

# Defaulting and Deleting Special Member Functions

Prior to C++11, if we wanted to prevent the use of a copy constructor or copy assignment member function, then we had to declare the function as private and not provide a definition of the function:

```
class Fraction
{
public:
    Fraction();

private:
    Fraction(const Fraction&);
```