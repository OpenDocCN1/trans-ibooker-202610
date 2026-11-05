
```
<name>CodeAnalyzing</name>
</assembly>
<members>
    <member name="T:CodeAnalyzing.Program">
        <summary>
        The main class for this console app.
        </summary>
    </member>
    <member name="M:CodeAnalyzing.Program.Main(System.String[])">
        <summary>
        The main entry point for this console app.
        </summary>
        <param name="args">A string array of arguments passed to the console app.</param>
    </member>
</members>
</doc>
```

## Understanding common StyleCop recommendations

Inside a code file, you should order the contents, as shown in the following list:

- 1. External alias directives
- 2. Using directives
- 3. Namespaces
- 4. Delegates
- 5. Enums
- 6. Interfaces
- 7. Structs
- 8. Classes

Within a class, record, struct, or interface, you should order the contents, as shown in the following list:

- 1. Fields
- 2. Constructors
- 3. Destructors (finalizers)
- 4. Delegates
- 5. Events
- 6. Enums
- 7. Interfaces
- 8. Properties
- 9. Indexers
- 10. Methods
- 11. Structs
- 12. Nested classes and records

**Good Practice:** You can learn about all the StyleCop rules at the following link: https://github.com/DotNetAnalyzers/StyleCopAnalyzers/blob/master/DOCUMENTATION.md.

## Practicing and exploring

Test your knowledge and understanding by answering some questions. Get some hands-on practice and explore this chapter's topics with more in-depth research.

## Exercise 6.1 – Test your knowledge

Answer the following questions:

- 1. What is a delegate?
- 2. What is an event?
- 3. How are a base class and a derived class related, and how can the derived class access the base class?
- 4. What is the difference between is and as operators?
- 5. Which keyword is used to prevent a class from being derived from or a method from being further overridden?
- 6. Which keyword is used to prevent a class from being instantiated with the new keyword?
- 7. Which keyword is used to allow a member to be overridden?
- 8. What's the difference between a destructor and a deconstruct method?
- 9. What are the signatures of the constructors that all exceptions should have?
- 10. What is an extension method, and how do you define one?

## Exercise 6.2 – Practice creating an inheritance hierarchy

Explore inheritance hierarchies by following these steps:

- 1. Add a new console application named Exercise02 to your Chapter06 solution/workspace.
- 2. Create a class named Shape with properties named Height, Width, and Area.
- 3. Add three classes that derive from it—Rectangle, Square, and Circle—with any additional members you feel are appropriate and that override and implement the Area property correctly.
- 4. In Main, add statements to create one instance of each shape, as shown in the following code:

```
Rectangle r = new(height: 3, width: 4.5);
WriteLine($"Rectangle H: {r.Height}, W: {r.Width}, Area: {r.Area}");

Square s = new(5);
WriteLine($"Square H: {s.Height}, W: {s.Width}, Area: {s.Area}");

Circle c = new(radius: 2.5);
WriteLine($"Circle H: {c.Height}, W: {c.Width}, Area: {c.Area}");
```

- 5. Run the console application and ensure that the result looks like the following output:

```
Rectangle H: 3, W: 4.5, Area: 13.5
Square H: 5, W: 5, Area: 25
Circle H: 5, W: 5, Area: 19.6349540849362
```

## Exercise 6.3 – Explore topics

Use the links on the following page to learn more about the topics covered in this chapter:

https://github.com/markjprice/cs10dotnet6/blob/main/book-links.md#chapter-6---implementing-interfaces-and-inheriting-classes

## Summary

In this chapter, you learned about local functions and operators, delegates and events, implementing interfaces, generics, and deriving types using inheritance and OOP. You also learned about base and derived classes, and how to override a type member, use polymorphism, and cast between types.

In the next chapter, you will learn how .NET is packaged and deployed, and, in subsequent chapters, the types that it provides you with to implement common functionality such as file handling, database access, encryption, and multitasking.

## 07

## Packaging and Distributing .NET Types

This chapter is about how C# keywords are related to .NET types, and about the relationship between namespaces and assemblies. You'll also become familiar with how to package and publish your .NET apps and libraries for cross-platform use, how to use legacy .NET Framework libraries in .NET libraries, and the possibility of porting legacy .NET Framework code bases to modern .NET.

This chapter covers the following topics:

- The road to .NET 6
- Understanding .NET components
- Publishing your applications for deployment
- Decompiling .NET assemblies
- Packaging your libraries for NuGet distribution
- Porting from .NET Framework to modern .NET
- Working with preview features

## The road to .NET 6

This part of the book is about the functionality in the **Base Class Library (BCL)** APIs provided by .NET and how to reuse functionality across all the different .NET platforms using .NET Standard.

First, we will review the route to this point and why it is important to understand the past.

.NET Core 2.0 and later's support for a minimum of .NET Standard 2.0 is important because it provides many of the APIs that were missing from the first version of .NET Core. The 15 years' worth of libraries and applications that .NET Framework developers had available to them that are relevant for modern development have now been migrated to .NET and can run cross-platform on macOS and Linux variants, as well as on Windows.

.NET Standard 2.1 added about 3,000 new APIs. Some of those APIs need runtime changes that would break backward compatibility, so .NET Framework 4.8 only implements .NET Standard 2.0. .NET Core 3.0, Xamarin, Mono, and Unity implement .NET Standard 2.1.

.NET 6 removes the need for .NET Standard if all your projects can use .NET 6. Since you might still need to create class libraries for legacy .NET Framework projects or legacy Xamarin mobile apps, there is still a need to create .NET Standard 2.0 and 2.1 class libraries. In March 2021, I surveyed professional developers, and half still needed to create .NET Standard 2.0 compliant class libraries.

Now that .NET 6 has been released with preview support for mobile and desktop apps built using .NET MAUI, the need for .NET Standard has been further reduced.

To summarize the progress that .NET has made over the past five years, I have compared the major .NET Core and modern .NET versions with the equivalent .NET Framework versions in the following list:

- .NET Core 1.x: much smaller API compared to .NET Framework 4.6.1, which was the current version in March 2016.
- .NET Core 2.x: reached API parity with .NET Framework 4.7.1 for modern APIs because they both implement .NET Standard 2.0.
- .NET Core 3.x: larger API compared to .NET Framework for modern APIs because .NET Framework 4.8 does not implement .NET Standard 2.1.
- .NET 5: even larger API compared to .NET Framework 4.8 for modern APIs, with much-improved performance.
- .NET 6: final unification with the support for mobile apps in .NET MAUI, expected by May 2022.

## .NET Core 1.0

.NET Core 1.0 was released in June 2016 and focused on implementing an API suitable for building modern cross-platform apps, including web and cloud applications and services for Linux using ASP.NET Core.

## .NET Core 1.1

.NET Core 1.1 was released in November 2016 and focused on fixing bugs, increasing the number of Linux distributions supported, supporting .NET Standard 1.6, and improving performance, especially with ASP.NET Core for web apps and services.

## .NET Core 2.0

.NET Core 2.0 was released in August 2017 and focused on implementing .NET Standard 2.0, the ability to reference .NET Framework libraries, and more performance improvements.

The third edition of this book was published in November 2017, so it covered up to .NET Core 2.0 and .NET Core for Universal Windows Platform (UWP) apps.

## .NET Core 2.1

.NET Core 2.1 was released in May 2018 and focused on an extendable tooling system, adding new types like Span<T>, new APIs for cryptography and compression, a Windows Compatibility Pack with an additional 20,000 APIs to help port old Windows applications, Entity Framework Core value conversions, LINQ GroupBy conversions, data seeding, query types, and even more performance improvements, including the topics listed in the following table:

| Feature | Chapter | Topic |
|---|---|---|
| Spans | 8 | Working with spans, indexes, and ranges |
| Brotli compression | 9 | Compressing with the Brotli algorithm |
| Cryptography | 20 | What's new in cryptography? |
| EF Core Lazy loading | 10 | Enabling lazy loading |
| EF Core Data seeding | 10 | Understanding data seeding |

## .NET Core 2.2

.NET Core 2.2 was released in December 2018 and focused on diagnostic improvements for the runtime, optional tiered compilation, and adding new features to ASP.NET Core and Entity Framework Core like spatial data support using types from the NetTopologySuite (NTS) library, query tags, and collections of owned entities.

## .NET Core 3.0

.NET Core 3.0 was released in September 2019 and focused on adding support for building Windows desktop applications using Windows Forms (2001), Windows Presentation Foundation (WPF; 2006), and Entity Framework 6.3, side-by-side and app-local deployments, a fast JSON reader, serial port access and other pinout access for Internet of Things (IoT) solutions, and tiered compilation by default, including the topics listed in the following table:

| Feature | Chapter | Topic |
|---|---|---|
| Embedding .NET in-app | 7 | Publishing your applications for deployment |
| Index and Range | 8 | Working with spans, indexes, and ranges |
| System.Text.Json | 9 | High-performance JSON processing |
| Async streams | 12 | Working with async streams |
