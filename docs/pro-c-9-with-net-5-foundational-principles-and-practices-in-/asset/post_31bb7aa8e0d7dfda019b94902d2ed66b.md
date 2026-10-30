
### Chapter 5: Understanding Encapsulation

This chapter begins your examination of object-oriented programming (OOP) using the C# programming language. After you are introduced to the pillars of OOP (encapsulation, inheritance, and polymorphism), the remainder of this chapter will show you how to build robust class types using constructors, properties, static members, constants, and read-only fields. You will also learn about partial type definitions, object initialization syntax, and automatic properties, and the chapter will wrap up with an examination of record types, new in C# 9.0.

### Chapter 6: Understanding Inheritance and Polymorphism

Here, you will examine the remaining pillars of OOP (inheritance and polymorphism), which allow you to build families of related class types. As you do this, you will examine the role of virtual methods, abstract methods (and abstract base classes), and the nature of the polymorphic interface. Then you will explore pattern matching with the is keyword, and finally, this chapter will explain the role of the ultimate base class of the .NET Core platform, System.Object.

### Chapter 7: Understanding Structured Exception Handling

The point of this chapter is to discuss how to handle runtime anomalies in your code base through the use of structured exception handling. Not only will you learn about the C# keywords that allow you to handle such problems (try, catch, throw, when, and finally), but you will also come to understand the distinction between application-level and system-level exceptions. In addition, this chapter will show you how to set Visual Studio on break on all exceptions to debug the exceptions that escape your notice.

### Chapter 8: Working with Interfaces

The material in this chapter builds upon your understanding of object-based development by covering the topic of interface-based programming. Here, you will learn how to define classes and structures that support multiple behaviors, how to discover these behaviors at runtime, and how to selectively hide particular behaviors using explicit interface implementation. In addition to creating a number of custom interfaces, you will also learn how to implement standard interfaces found within the .NET Core platform. You will use these to build objects that can be sorted, copied, enumerated, and compared.

### Chapter 9: Understanding Object Lifetime

The final chapter of this part examines how the CLR manages memory using the .NET Core garbage collector. Here, you will come to understand the role of application roots, object generations, and the System.GC type. Once you understand the basics, you will examine the topics of disposable objects (using the IDisposable interface) and the finalization process (using the System.Object.Finalize() method). This chapter will also investigate the Lazy<T> class, which allows you to define data that will not be allocated until requested by a caller. As you will see, this feature can be helpful when you want to ensure you do not clutter the heap with objects that are not actually required by your programs.

## Part IV: Advanced C# Programming

This part of the book will deepen your understanding of the C# language by walking you through a number of more advanced (but important) concepts. Here, you will complete your examination of the .NET Core type system by investigating collections and generics. You will also examine a number of more advanced features of C# (e.g., extension methods, operator overloading, anonymous types, and pointer manipulation). You will then examine delegates and lambda expressions, take a first look at Language Integrated Query, and finish the section with two chapters that focus on processes and multithreaded/async programming.

### Chapter 10: Collections and Generics

This chapter explores the topic of *generics*. As you will see, generic programming gives you a way to create types and type members, which contain various *placeholders* that can be specified by the caller. In a nutshell, generics greatly enhance application performance and type safety. Not only will you explore various generic types within the System.Collections.Generic namespace, but you will also learn how to build your own generic methods and types (with and without constraints).

### Chapter 11: Advanced C# Language Features

This chapter deepens your understanding of the C# programming language by introducing you to a number of advanced programming techniques. Here, you will learn how to overload operators and create custom conversion routines (both implicit and explicit) for your types. You will also learn how to build and interact with type indexers, as well as work with extension methods, anonymous types, partial methods, and C# pointers using an unsafe code context.

### Chapter 12: Delegates, Events, and Lambda Expressions

The purpose of this chapter is to demystify the delegate type. Simply put, a .NET Core delegate is an object that points to other methods in your application. Using this type, you can build systems that allow multiple objects to engage in a two-way conversation. After you have examined the use of .NET Core delegates, you will then be introduced to the C# event keyword, which you can use to simplify the manipulation of raw delegate programming. You will wrap up this chapter by investigating the role of the C# lambda operator (=>) and exploring the connection between delegates, anonymous methods, and lambda expressions.

### Chapter 13: LINQ to Objects

This chapter begins your examination of Language Integrated Query (LINQ). LINQ allows you to build strongly typed query expressions that can be applied to a number of LINQ targets to manipulate data in the broadest sense of the word. Here, you will learn about LINQ to Objects, which allows you to apply LINQ expressions to containers of data (e.g., arrays, collections, and custom types). This information will serve you well as you encounter a number of additional LINQ APIs throughout the remainder of this book.

### Chapter 14: Processes, AppDomains, and Load Contexts

Now that you have a solid understanding of assemblies, this chapter dives deeper into the composition of a loaded .NET Core executable. The goal of this chapter is to illustrate the relationship between processes, application domains, and contextual boundaries. These topics provide the proper foundation for Chapter 15, where you will examine the construction of multithreaded applications.

### Chapter 15: Multithreaded, Parallel, and Async Programming

This chapter examines how to build multithreaded applications and illustrates a number of techniques you can use to author thread-safe code. The chapter opens by revisiting the .NET delegate type to ensure explaining a delegate’s intrinsic support for asynchronous method invocations. Next, you will investigate the types within the System.Threading namespace. The next section covers the Task Parallel Library (TPL). Using the TPL, .NET developers can build applications that distribute their workload across all available CPUs in a wickedly simple manner. At this point, you will also learn about the role of Parallel LINQ, which provides a way to create LINQ queries that scale across multiple machine cores. The remainder of the chapter covers creating nonblocking calls using the async/await keywords, introduced in C# 5; local functions and generalized async return types, both new in C# 7; and asynchronous streams, introduced in C# 8.

## Part V: Programming with .NET Core Assemblies

Part V dives into the details of the .NET Core assembly format. Not only will you learn how to deploy and configure .NET Core code libraries, but you will also come to understand the internal composition of a .NET Core binary image. This section explains the role of .NET Core attributes and the role of resolving type information at runtime and the role of the Dynamic Language Runtime (DLR) and the C# dynamic keyword. The final chapter covers the syntax of Common Intermediate Language (CIL) and the role of dynamic assemblies.

### Chapter 16: Building and Configuring Class Libraries

At a high level, assembly is the term used to describe a binary file created with a .NET Core compiler. However, the true story of .NET Core assemblies is far richer than that. Here, you will learn how to build and deploy assemblies and learn the difference between class libraries and console applications and the difference between .NET Core and .NET Standard class libraries. The final section covers the new options available in .NET 5, such as single file executables and ready-to-run publishing.

### Chapter 17: Type Reflection, Late Binding, and Attribute-Based Programming

This chapter continues your examination of .NET Core assemblies by checking out the process of runtime type discovery using the System.Reflection namespace. Using the types of this namespace, you can build applications that can read an assembly’s metadata on the fly. You will also learn how to load and create types at runtime dynamically using late binding. The final topic of this chapter will explore the role of .NET Core attributes (both standard and custom). To illustrate the usefulness of each of these topics, the chapter shows you how to construct an extendable application complete with snap-ins.

### Chapter 18: Dynamic Types and the Dynamic Language Runtime

.NET 4.0 introduced a new aspect of the .NET runtime environment called the Dynamic Language Runtime. Using the DLR and the C# dynamic keyword, you can define data that is not truly resolved until runtime. Using these features simplifies some complex .NET Core programming tasks dramatically. In this chapter, you will learn some practical uses of dynamic data, including how to leverage the .NET Core reflection APIs in a streamlined manner, as well as how to communicate with legacy COM libraries with a minimum of fuss and bother.
