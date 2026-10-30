# Compiling CIL to Platform-Specific Instructions

Because assemblies contain CIL instructions rather than platform-specific instructions, CIL code must be compiled on the fly before use. The entity that compiles CIL code into meaningful CPU instructions is a JIT compiler, which sometimes goes by the friendly name of jitter. The .NET Core runtime environment leverages a JIT compiler for each CPU targeting the runtime, each optimized for the underlying platform.

For example, if you are building a .NET Core application to be deployed to a handheld device (such as an iOS or Android phone), the corresponding jitter is well equipped to run within a low-memory environment. On the other hand, if you are deploying your assembly to a back-end company server (where memory is seldom an issue), the jitter will be optimized to function in a high-memory environment. In this way, developers can write a single body of code that can be efficiently JIT compiled and executed on machines with different architectures.

Furthermore, as a given jitter compiles CIL instructions into corresponding machine code, it will cache the results in memory in a manner suited to the target operating system. In this way, if a call is made to a method named PrintDocument(), the CIL instructions are compiled into platform-specific instructions on the first invocation and retained in memory for later use. Therefore, the next time PrintDocument() is called, there is no need to recompile the CIL.

# Precompiling CIL to Platform-Specific Instructions

There is a utility in .NET Core called crossgen.exe, which can be used to pre-JIT your code. Fortunately, in .NET Core 3.0, the ability to produce "ready-to-run" assemblies is built into the framework. More on this later in this book.

# The Role of .NET Core Type Metadata

In addition to CIL instructions, a .NET Core assembly contains full, complete, and accurate metadata, which describes every type (e.g., class, structure, enumeration) defined in the binary, as well as the members of each type (e.g., properties, methods, events). Thankfully, it is always the job of the compiler (not the programmer) to emit the latest and greatest type metadata. Because .NET Core metadata is so wickedly meticulous, assemblies are completely self-describing entities.

To illustrate the format of .NET Core type metadata, let's take a look at the metadata that has been generated for the Add() method of the C# Calc class you examined previously (the metadata generated for the Visual Basic version of the Add() method is similar, so we will examine the C# version only).

```
TypeDef #2 (02000003)
--------------------------------------------------
    TypDefName: CalculatorExamples.Calc  (02000003)
    Flags      : [NotPublic] [AutoLayout] [Class] [AnsiClass] [BeforeFieldInit]  (00100000)
    Extends    : 0100000C [TypeRef] System.Object
    Method #1 (06000003)
--------------------------------------------------
    MethodName: Add (06000003)
    Flags      : [Public] [HideBySig] [ReuseSlot]  (00000086)
    RVA        : 0x00002090
    ImplFlags  : [IL] [Managed]  (00000000)
    CallCnvtn: [DEFAULT]
    hasThis
    ReturnType: I4
    2 Arguments
```