# Implicit Conversion

In determining the Candidate Set of Function in Figure 3.36, Function overload resolution, the compiler has to look at all of the available functions that have been found during the name lookup and determine whether the argument number and types match the call point. In determining whether the type matches, it will also examine all the available conversions to determine whether there is a mechanism to convert from the type T1 type (the type of the argument passed) to the T2 type (the type specified for the function parameter). If it can convert from T1 to T2 for all arguments, then it will add the function to the set of candidates.

This transformation from type T1 to type T2 is known as implicit conversion and occurs when some type, T1, is used in an expression or context that does not accept that type but accepts some other type, T2. This occurs in the following contexts:

+   - T1 is passed as an argument when calling a function declared with T2 as the parameter.
+   - T1 is used as an operand to an operator that expects T2.
+   - T1 is used to initialize a new object of T2 (including return statements).
+   - T1 is used in a switch statement (in which case, T2 is an int).
+   - T1 is used in an if statement or do-while or while loop (where T2 is bool).

If an unambiguous conversion sequence from T1 to 2 exists, then the program will compile. The conversions between built-in types are usually determined by the usual arithmetic conversions.

# Explicit – Preventing Implicit Conversion

Implicit conversion is a great feature that, make it possible for a programmer to express their intent, and it just works most of the time. However, the compiler's ability to convert one type into another without the programmer providing a hint, is not always desirable. Consider the following small program:

```
#include <iostream>
```

```
class Real
{
public:
```