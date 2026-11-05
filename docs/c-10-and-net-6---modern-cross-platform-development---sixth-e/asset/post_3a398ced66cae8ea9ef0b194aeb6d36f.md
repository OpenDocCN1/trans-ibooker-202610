
5. Add statements to define an interface with a static abstract method, a class that implements it, and then call the method in the top-level program, as shown in the following code:

```
using static System.Console;

Doer.DoSomething();

public interface IWithStaticAbstract
{
    static abstract void DoSomething();
}

public class Doer : IWithStaticAbstract
{
    public static void DoSomething()
    {
        WriteLine("I am an implementation of a static abstract method.");
    }
}
```

6. Run the console app and note that it outputs correctly.

## Generic mathematics

Why has Microsoft added the ability to define static abstract methods? What are they useful for?

For a long time, developers have asked Microsoft for the ability to use operators like * on generic types. This would enable a developer to define mathematical methods to perform operations like adding, averaging, and so on to any generic type rather than having to create dozens of overloaded methods for all the numeric types they want to support. Support for static abstract methods in interfaces is a foundational feature that would enable generic mathematics.

If you are interested, you can read more about this at the following link:

https://devblogs.microsoft.com/dotnet/preview-features-in-net-6-generic-math/

## Practicing and exploring

Test your knowledge and understanding by answering some questions, getting some hands-on practice, and exploring with deeper research into topics of this chapter.

## Exercise 7.1 – Test your knowledge

Answer the following questions:

-   1. What is the difference between a namespace and an assembly?
-   2. How do you reference another project in a .csproj file?
-   3. What is the benefit of a tool like ILSpy?
-   4. Which .NET type does the C# float alias represent?
-   5. When porting an application from .NET Framework to .NET 6, what tool should you run before porting, and what tool could you run to perform much of the porting work?
-   6. What is the difference between framework-dependent and self-contained deployments of .NET applications?
-   7. What is a RID?
-   8. What is the difference between the dotnet pack and dotnet publish commands?
-   9. What types of applications written for the .NET Framework can be ported to modern .NET?
-   10. Can you use packages written for .NET Framework with modern .NET?

## Exercise 7.2 – Explore topics

Use the links on the following page to learn more detail about the topics covered in this chapter:

```
https://github.com/markjprice/cs10dotnet6/blob/main/book-links.md#chapter-7---understanding-and-packaging-net-types
```

## Exercise 7.3 – Explore PowerShell

PowerShell is Microsoft's scripting language for automating tasks on every operating system. Microsoft recommends Visual Studio Code with the PowerShell extension for writing PowerShell scripts.

Since PowerShell is its own extensive language there is not space in this book to cover it. Instead, I have created some supplementary pages on the books GitHub repository to introduce you to some key concepts and show some examples:

```
https://github.com/markjprice/cs10dotnet6/tree/main/docs/powershell
```

## Summary

In this chapter, we reviewed the journey to .NET 6, we explored the relationship between assemblies and namespaces, we saw options for publishing an app for distribution to multiple operating systems, packaged and distributed a class library, and we discussed options for porting existing .NET Framework code bases.

In the next chapter, you will learn about some common Base Class Library types that are included with modern .NET.

## 08

## Working with Common .NET Types

This chapter is about some common types that are included with .NET. These include types for manipulating numbers, text, collections, network access, reflection, and attributes; improving working with spans, indexes, and ranges; manipulating images; and internationalization.

This chapter covers the following topics:

-   - Working with numbers
-   - Working with text
-   - Working with dates and times
-   - Pattern matching with regular expressions
-   - Storing multiple objects in collections
-   - Working with spans, indexes, and ranges
-   - Working with network resources
-   - Working with reflection and attributes
-   - Working with images
-   - Internationalizing your code

## Working with numbers

One of the most common types of data is numbers. The most common types in .NET for working with numbers are shown in the following table:

| Namespace | Example type(s) | Description |
|---|---|---|
| System | SByte, Int16, Int32, Int64 | Integers; that is, zero and positive and negative whole numbers |
| System | Byte, UInt16, UInt32, UInt64 | Cardinals; that is, zero and positive whole numbers |
| System | Half, Single, Double | Reals; that is, floating-point numbers |
| System | Decimal | Accurate reals; that is, for use in science, engineering, or financial scenarios |
| System.Numerics | BigInteger, Complex, Quaternion | Arbitrarily large integers, complex numbers, and quaternion numbers |

.NET has had the 32-bit float and 64-bit double types since .NET Framework 1.0. The IEEE 754 specification also defines a 16-bit floating point standard. Machine learning and other algorithms would benefit from this smaller, lower-precision number type so Microsoft introduced the System.Half type with .NET 5 and later.

Currently, the C# language does not define a half alias so you must use the .NET type System.Half. This might change in the future.

## Working with big integers

The largest whole number that can be stored in .NET types that have a C# alias is about eighteen and a half quintillion, stored in an unsigned long integer. But what if you need to store numbers larger than that?

Let's explore numerics:

-   1. Use your preferred code editor to create a new solution/workspace named `Chapter08`.
-   2. Add a console app project, as defined in the following list:
-      1. Project template: **Console Application** / `console`
-      2. Workspace/solution file and folder: `Chapter08`
-      3. Project file and folder: `WorkingWithNumbers`
-   3. In `Program.cs`, delete the existing statements and add a statement to import `System.Numerics`, as shown in the following code:
-   4. Add statements to output the maximum value of the `ulong` type, and a number with 30 digits using `BigInteger`, as shown in the following code:

```
using System.Numerics;
```

```
WriteLine("Working with large integers:");
WriteLine("---------------------------------");

ulong big = ulong.MaxValue;
WriteLine($"{big,40:N0}");

BigInteger bigger =
    BigInteger.Parse("123456789012345678901234567890");

WriteLine($"{bigger,40:N0}");
```

The 40 in the format code means right-align 40 characters, so both numbers are lined up to the right-hand edge. The N0 means use thousand separators and zero decimal places.

-   5. Run the code and view the result, as shown in the following output:

```
Working with large integers:
---------------------------------
                18,446,744,073,709,551,615
123,456,789,012,345,678,901,234,567,890
```

## Working with complex numbers

A complex number can be expressed as a + bi, where a and b are real numbers, and i is an imaginary unit, where i² = -1. If the real part a is zero, it is a pure imaginary number. If the imaginary part b is zero, it is a real number.

Complex numbers have practical applications in many STEM (science, technology, engineering, and mathematics) fields of study. Additionally, they are added by separately adding the real and imaginary parts of the summands; consider this:

```
(a + bi) + (c + di) = (a + c) + (b + d)i
```

Let's explore complex numbers:

-   1. In Program.cs, add statements to add two complex numbers, as shown in the following code:

```
WriteLine("Working with complex numbers:");
Complex c1 = new(real: 4, imaginary: 2);
Complex c2 = new(real: 3, imaginary: 7);
Complex c3 = c1 + c2;

// output using default ToString implementation
WriteLine($"{c1} added to {c2} is {c3}");

// output using custom format
```
