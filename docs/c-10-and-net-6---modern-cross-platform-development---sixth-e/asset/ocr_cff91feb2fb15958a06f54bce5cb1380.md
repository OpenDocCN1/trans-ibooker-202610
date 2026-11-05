| Feature | Chapter | Topic |
|---|---|---|
| Global namespace imports | 2 | Importing namespaces |
| Constant string literals | 2 | Formatting using interpolated strings |
| File-scoped namespaces | 5 | Simplifying namespace declarations |
| Required properties | 5 | Requiring properties to be set during instantiation |
| Record structs | 6 | Working with record struct types |
| Null parameter checks | 6 | Checking for null in method parameters |

# Understanding C# standards

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

# Discovering your C# compiler versions

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