So far, the multiple constructors on **Fraction** and the overloaded arithmetic operators are all examples of overloaded functions that the compiler has to refer to when it meets one of these functions. Consider the following code:

```
int main(int argc, char** argv) {
    print(42);
}
```

When the compiler meets the line **print(42)**, it needs to work out which of the previously defined functions to call, so it performs the following process (grossly simplified):

![](bbox=[0.07, 0.36, 0.88, 0.58])

Figure 2A.44: Function overload resolution (simplified)

The C++ standard defines rules for how the compiler determines the best candidate function based on how it has to manipulate (that is, convert) the arguments to get a match. If no conversions are required, then that function is the best match.

# Classes, Structs, and Unions

When you define a class and do not specify an access modifier (public, protected, private), all the members will be private by default:

```
class Fraction
{
    Fraction() {};        // All of these are private
    int m_numerator;
    int m_denominator;
};
```