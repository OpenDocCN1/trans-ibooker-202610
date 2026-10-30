
- Updates to logic flow using type matching in addition to value checking (pattern matching)
- Returning a reference to a value, instead of just the value itself (ref locals and returns)
- The introduction of lightweight throwaway variables (called *discards*)
- Throw expressions, allowing the throw to be executed in more places, such as conditional expressions, lambdas, and others

C# 7 has had two minor releases, adding the following features:

- The ability to have a program's main method be async.
- A new literal, default, that allows for initialization of any type.
- Correction of an issue with pattern matching that prevented using generics with the new pattern matching feature.
- Like anonymous methods, tuple names can be inferred from the projection that creates them.
- Techniques for writing safe, efficient code, a combination of syntax improvements that enable working with value types using reference semantics.
- Named arguments can be followed by positional arguments.
- Numeric literals can now have leading underscores before any printed digits.
- The private protected access modifier enables access for derived classes in the same assembly.
- The result of a conditional expression (?:) can now be a reference.

That is also the edition where I added "(New 7.x)" and "(Updated 7.x)" to section titles to make it easier to find the changes in the language from the previous version. The "x" indicates the minor version of C# 7, such as 7.1.

C# 8, released with .NET Core 3.0 on September 23, 2019, introduced additional features for streamlining your code base, and it added some more significant features (such as tuples and ref locals and returns) that developers had been asking to have included in the language specification for quite some time.

C# 8 has had two minor releases, adding the following features:

- Read-only members for structs
- Default interface members
- Pattern matching enhancements
- Using declarations
- Static local functions
- Disposable ref structs
- Nullable reference types
- Asynchronous streams
- Indices and ranges
- Null-coalescing assignment
- Unmanaged constructed types
- stackalloc in nested expressions
- Enhancement of interpolated verbatim strings

New features in C# 8 are indicated as “(New 8)” in their section headings, and updated features are indicated as “(Updated 8).”

## New Features in C# 9

C# 9, released on November 10, 2020, with .NET 5, adds the following features:

- Records
- Init-only setters
- Top-level statements
- Pattern matching enhancements
- Performance improvements for interop
- “Fit and finish” features
- Support for code generators

New features in C# 9 are indicated as “(New 9)” in their section headings, and updated features are indicated as “(Updated 9).”

## Managed vs. Unmanaged Code

It is important to note that the C# language can be used only to build software that is hosted under the .NET Core runtime (you could never use C# to build a native COM server or an unmanaged C/C++-style application). Officially speaking, the term used to describe the code targeting the .NET Core runtime is managed code. The binary unit that contains the managed code is termed an assembly (more details on assemblies in just a bit). Conversely, code that cannot be directly hosted by the .NET Core runtime is termed unmanaged code.

As mentioned previously, the .NET Core platform can run on a variety of operating systems. Thus, it is quite possible to build a C# application on a Windows machine and run the program on an iOS machine using the .NET Core runtime. As well, you can build a C# application on Linux using Visual Studio Code and run the program on Windows. With Visual Studio for Mac, you can also build .NET Core applications on a Mac to be run on Windows, macOS, or Linux.

Unmanaged code can still be accessed from a C# program, but it then locks you into a specific development and deployment target.

## Using Additional .NET Core–Aware Programming Languages

Understand that C# is not the only language that can be used to build .NET Core applications. .NET Core applications can generally be built with C#, Visual Basic, and F#, which are the three languages supported directly by Microsoft.

## Getting an Overview of .NET Assemblies

Regardless of which .NET Core language you choose to program with, understand that despite .NET Core binaries taking the same file extension as unmanaged Windows binaries (*.dll), they have absolutely no internal similarities. Specifically, .NET Core binaries do not contain platform-specific instructions but rather platform-agnostic Intermediate Language (IL) and type metadata.

> Note IL is also known as Microsoft Intermediate Language (MSIL) or alternatively as the Common Intermediate Language (CIL). Thus, as you read the .NET/.NET Core literature, understand that IL, MSIL, and CIL are all describing essentially the same concept. In this book, I will use the abbreviation CIL to refer to this low-level instruction set.

When a *.dll has been created using a .NET Core compiler, the binary blob is termed an assembly. You will examine numerous details of .NET Core assemblies in Chapter 16. However, to facilitate the current discussion, you do need to understand four basic properties of this new file format.

First, unlike .NET Framework assemblies that can be either a *.dll or *.exe, .NET Core projects are always compiled to a file with a .dll extension, even if the project is an executable. Executable .NET Core assemblies are executed with the command dotnet <assembly name>.dll. New in .NET Core 3.0 (and later), the dotnet.exe command is copied to the build directory and renamed to <assembly name>.exe. Running this command automatically calls the dotnet <assembly name>.dll file, executing the equivalent of dotnet <assembly name>.dll. The *.exe with your project name isn’t actually your project’s code; it is a convenient shortcut to running your application.

New in .NET 5, your application can be reduced to a single file that is executed directly. Even though this single file looks and acts like a C++-style native executable, the single file is a packaging convenience. It contains all the files needed to run your application, potentially even the .NET 5 runtime itself! But know that your code is still running in a managed container just as if it were published as multiple files.

Second, an assembly contains CIL code, which is conceptually similar to Java bytecode, in that it is not compiled to platform-specific instructions until absolutely necessary. Typically, “absolutely necessary” is the point at which a block of CIL instructions (such as a method implementation) is referenced for use by the .NET Core runtime.

Third, assemblies also contain metadata that describes in vivid detail the characteristics of every “type” within the binary. For example, if you have a class named SportsCar, the type metadata describes details such as SportsCar’s base class, specifies which interfaces are implemented by SportsCar (if any), and gives full descriptions of each member supported by the SportsCar type. .NET Core metadata is always present within an assembly and is automatically generated by the language compiler.

Finally, in addition to CIL and type metadata, assemblies themselves are also described using metadata, which is officially termed a manifest. The manifest contains information about the current version of the assembly, culture information (used for localizing string and image resources), and a list of all externally referenced assemblies that are required for proper execution. You’ll examine various tools that can be used to examine an assembly’s types, metadata, and manifest information over the course of the next few chapters.

## The Role of the Common Intermediate Language

Let’s examine CIL code, type metadata, and the assembly manifest in a bit more detail. CIL is a language that sits above any particular platform-specific instruction set. For example, the following C# code models a trivial calculator. Don’t concern yourself with the exact syntax for now, but do notice the format of the Add() method in the Calc class.
