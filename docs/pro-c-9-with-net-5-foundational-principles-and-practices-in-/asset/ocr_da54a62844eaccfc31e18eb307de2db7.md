# Chapter 15: Multithreaded, Parallel, and Async Programming

This chapter examines how to build multithreaded applications and illustrates a number of techniques you can use to author thread-safe code. The chapter opens by revisiting the .NET delegate type to ensure explaining a delegate’s intrinsic support for asynchronous method invocations. Next, you will investigate the types within the System.Threading namespace. The next section covers the Task Parallel Library (TPL). Using the TPL, .NET developers can build applications that distribute their workload across all available CPUs in a wickedly simple manner. At this point, you will also learn about the role of Parallel LINQ, which provides a way to create LINQ queries that scale across multiple machine cores. The remainder of the chapter covers creating nonblocking calls using the async/await keywords, introduced in C# 5; local functions and generalized async return types, both new in C# 7; and asynchronous streams, introduced in C# 8.

# Part V: Programming with .NET Core Assemblies

Part V dives into the details of the .NET Core assembly format. Not only will you learn how to deploy and configure .NET Core code libraries, but you will also come to understand the internal composition of a .NET Core binary image. This section explains the role of .NET Core attributes and the role of resolving type information at runtime and the role of the Dynamic Language Runtime (DLR) and the C# dynamic keyword. The final chapter covers the syntax of Common Intermediate Language (CIL) and the role of dynamic assemblies.

# Chapter 16: Building and Configuring Class Libraries

At a high level, assembly is the term used to describe a binary file created with a .NET Core compiler. However, the true story of .NET Core assemblies is far richer than that. Here, you will learn how to build and deploy assemblies and learn the difference between class libraries and console applications and the difference between .NET Core and .NET Standard class libraries. The final section covers the new options available in .NET 5, such as single file executables and ready-to-run publishing.

# Chapter 17: Type Reflection, Late Binding, and Attribute-Based Programming

This chapter continues your examination of .NET Core assemblies by checking out the process of runtime type discovery using the System.Reflection namespace. Using the types of this namespace, you can build applications that can read an assembly’s metadata on the fly. You will also learn how to load and create types at runtime dynamically using late binding. The final topic of this chapter will explore the role of .NET Core attributes (both standard and custom). To illustrate the usefulness of each of these topics, the chapter shows you how to construct an extendable application complete with snap-ins.

# Chapter 18: Dynamic Types and the Dynamic Language Runtime

.NET 4.0 introduced a new aspect of the .NET runtime environment called the Dynamic Language Runtime. Using the DLR and the C# dynamic keyword, you can define data that is not truly resolved until runtime. Using these features simplifies some complex .NET Core programming tasks dramatically. In this chapter, you will learn some practical uses of dynamic data, including how to leverage the .NET Core reflection APIs in a streamlined manner, as well as how to communicate with legacy COM libraries with a minimum of fuss and bother.