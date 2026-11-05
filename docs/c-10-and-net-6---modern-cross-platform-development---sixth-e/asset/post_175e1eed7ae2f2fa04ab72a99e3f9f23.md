
## C# 7.2

C# 7.2 was released in November 2017 and focused on minor refinements to the language, including the topics listed in the following table:

| Feature | Chapter | Topic |
|---|---|---|
| Leading underscores in numeric literals | 2 | Storing whole numbers |
| Non-trailing named arguments | 5 | Optional parameters and named arguments |
| private protected access modifier | 5 | Understanding access modifiers |
| You can test == and != with tuple types | 5 | Comparing tuples |

## C# 7.3

C# 7.3 was released in May 2018 and focused on performance-oriented safe code that improves ref variables, pointers, and stackalloc. These are advanced and rarely needed for most developers, so they are not covered in this book.

## C# 8

C# 8 was released in September 2019 and focused on a major change to the language related to null handling, including the topics listed in the following table:

| Feature | Chapter | Topic |
|---|---|---|
| Nullable reference types | 6 | Making a reference type nullable |
| Switch expressions | 3 | Simplifying switch statements with switch expressions |
| Default interface methods | 6 | Understanding default interface methods |

## C# 9

C# 9 was released in November 2020 and focused on record types, refinements to pattern matching, and minimal-code console apps, including the topics listed in the following table:

| Feature | Chapter | Topic |
|---|---|---|
| Minimal-code console apps | 1 | Top-level programs |
| Target-typed new | 2 | Using target-typed new to instantiate objects |
| Enhanced pattern matching | 5 | Pattern matching with objects |
| Records | 5 | Working with records |

## C# 10

C# 10 was released in November 2021 and focused on features that minimize the amount of code needed in common scenarios, including the topics listed in the following table:

| Feature | Chapter | Topic |
|---|---|---|
| Global namespace imports | 2 | Importing namespaces |
| Constant string literals | 2 | Formatting using interpolated strings |
| File-scoped namespaces | 5 | Simplifying namespace declarations |
| Required properties | 5 | Requiring properties to be set during instantiation |
| Record structs | 6 | Working with record struct types |
| Null parameter checks | 6 | Checking for null in method parameters |

## Understanding C# standards

Over the years, Microsoft has submitted a few versions of C# to standards bodies, as shown in the following table:

| C# version | ECMA standard | ISO/IEC standard |
|---|---|---|
| 1.0 | ECMA-334:2003 | ISO/IEC 23270:2003 |
| 2.0 | ECMA-334:2006 | ISO/IEC 23270:2006 |
| 5.0 | ECMA-334:2017 | ISO/IEC 23270:2018 |

The standard for C# 6 is still a draft and work on adding C# 7 features is progressing. Microsoft made C# open source in 2014.

There are currently three public GitHub repositories for making the work on C# and related technologies as open as possible, as shown in the following table:

| Description | Link |
|---|---|
| C# language design | https://github.com/dotnet/csharplang |
| Compiler implementation | https://github.com/dotnet/roslyn |
| Standard to describe the language | https://github.com/dotnet/csharpstandard |

## Discovering your C# compiler versions

.NET language compilers for C# and Visual Basic, also known as Roslyn, along with a separate compiler for F#, are distributed as part of the .NET SDK. To use a specific version of C#, you must have at least that version of the .NET SDK installed, as shown in the following table:

| .NET SDK | Roslyn compiler | Default C# language |
|---|---|---|
| 1.0.4 | 2.0 - 2.2 | 7.0 |
| 1.1.4 | 2.3 - 2.4 | 7.1 |
| 2.1.2 | 2.6 - 2.7 | 7.2 |
| 2.1.200 | 2.8 - 2.10 | 7.3 |
| 3.0 | 3.0 - 3.4 | 8.0 |
| 5.0 | 3.8 | 9.0 |
| 6.0 | 3.9 - 3.10 | 10.0 |

When you create class libraries then you can choose to target .NET Standard as well as versions of modern .NET. They have default C# language versions, as shown in the following table:

| .NET Standard | C# |
|---|---|
| 2.0 | 7.3 |
| 2.1 | 8.0 |

## How to output the SDK version

Let's see what .NET SDK and C# language compiler versions you have available:

- 1. On macOS, start **Terminal**. On Windows, start **Command Prompt**.
- 2. To determine which version of the .NET SDK you have available, enter the following command:
- 3. Note the version at the time of writing is 6.0.100, indicating that it is the initial version of the SDK without any bug fixes or new features yet, as shown in the following output:

```
dotnet --version
```

```
6.0.100
```

## Enabling a specific language version compiler

Developer tools like Visual Studio and the dotnet command-line interface assume that you want to use the latest major version of a C# language compiler by default. Before C# 8.0 was released, C# 7.0 was the latest major version and was used by default. To use the improvements in a C# point release like 7.1, 7.2, or 7.3, you had to add a <LangVersion> configuration element to the project file, as shown in the following markup:

```xml
<LangVersion>7.3</LangVersion>
```

After the release of C# 10.0 with .NET 6.0, if Microsoft releases a C# 10.1 compiler and you want to use its new language features then you will have to add a configuration element to your project file, as shown in the following markup:

```xml
<LangVersion>10.1</LangVersion>
```

Potential values for the <LangVersion> are shown in the following table:

| LangVersion | Description |
|---|---|
| 7, 7.1, 7.2, 7.3<br>8, 9, 10 | Entering a specific version number will use that compiler if it has been installed. |
| latestmajor | Uses the highest major number, for example, 7.0 in August 2019, 8.0 in October 2019, 9.0 in November 2020, 10.0 in November 2021. |
| latest | Uses the highest major and highest minor number, for example, 7.2 in 2017, 7.3 in 2018, 8 in 2019, perhaps 10.1 in early 2022. |
| preview | Uses the highest available preview version, for example, 10.0 in July 2021 with .NET 6.0 Preview 6 installed. |

After creating a new project, you can edit the .csproj file and add the <LangVersion> element, as shown highlighted in the following markup:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net6.0</TargetFramework>
    <LangVersion>preview</LangVersion>
  </PropertyGroup>

</Project>
```

Your projects must target net6.0 to use the full features of C# 10.

![](img/c63b00dca7192cc58cfb9e1f1e64f483_83_0.png)

> **Good Practice:** If you are using Visual Studio Code and you have not done so already, install the Visual Studio Code extension named **MSBuild project tools**. This will give you IntelliSense while editing .csproj files, including making it easy to add the <LangVersion> element with appropriate values.

## Understanding C# grammar and vocabulary

To learn simple C# language features, you can use .NET Interactive Notebooks, which remove the need to create an application of any kind.

To learn some other C# language features, you will need to create an application. The simplest type of application is a console application.

Let's start by looking at the basics of the grammar and vocabulary of C#. Throughout this chapter, you will create multiple console applications, with each one showing related features of the C# language.

## Showing the compiler version

We will start by writing code that shows the compiler version:

- 1. If you've completed Chapter 1, Hello, C#! Welcome, .NET!, then you will already have a Code folder. If not, then you'll need to create it.
- 2. Use your preferred code editor to create a new console app, as defined in the following list:
  - 1. Project template: **Console Application [C#]** / `console`
  - 2. Workspace/solution file and folder: Chapter02
  - 3. Project file and folder: Vocabulary

> **Good Practice:** If you have forgotten how, or did not complete the previous chapter, then step-by-step instructions for creating a workspace/solution with multiple projects are given in *Chapter 1, Hello, C#! Welcome, .NET!*.

- 3. Open the Program.cs file, and at the top of the file, under the comment, add a statement to show the C# version as an error, as shown in the following code:

```
#error version
```

- 4. Run the console application:
  - 1. In Visual Studio Code, in a terminal, enter the command `dotnet run`.
  - 2. In Visual Studio, navigate to **Debug | Start Without Debugging**. When prompted to continue and run the last successful build, click No.

- 5. Note the compiler version and language version appear as a compiler error message number CS8304, as shown in Figure 2.1:

![](img/c63b00dca7192cc58cfb9e1f1e64f483_84_0.png)

Figure 2.1: A compiler error that shows the C# language version

- 6. The error message in the Visual Studio Code PROBLEMS window or Visual Studio Error List window says Compiler version: '4.0.0...' with language version 10.0.

- 7. Comment out the statement that causes the error, as shown in the following code:

```
// #error version
```

- 8. Note that the compiler error messages disappear.
