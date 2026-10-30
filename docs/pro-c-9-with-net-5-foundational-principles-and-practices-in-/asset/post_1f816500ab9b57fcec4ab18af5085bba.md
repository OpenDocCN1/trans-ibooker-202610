
//Calc.cs
using System;

namespace CalculatorExamples
{
    //This class contains the app's entry point.
    class Program
    {
        static void Main(string[] args)
        {
            Calc c = new Calc();
            int ans = c.Add(10, 84);
            Console.WriteLine("10 + 84 is {0}.", ans);
            //Wait for user to press the Enter key
            Console.ReadLine();
        }
    }
    // The C# calculator.
    class Calc
    {
        public int Add(int addend1, int addend2)
        {
            return addend1 + addend2;
        }
    }
}

Compiling this code produces a file *.dll assembly that contains a manifest, CIL instructions, and metadata describing each aspect of the Calc and Program classes.

Note Chapter 2 examines how to use graphical integrated development environments (IDEs), such as Visual Studio Community, to compile your code files.

For example, if you were to output the IL from this assembly using ildasm.exe (examined a little later in this chapter), you would find that the Add() method is represented using CIL such as the following:

```
.method public hidebysig instance int32
    Add(int32 addend1,
        int32 addend2) cil managed
{
    // Code size       9 (0x9)
    .maxstack  2
    .locals init (int32 V_0)
    IL_0000:  nop
    IL_0001:  ldarg.1
    IL_0002:  ldarg.2
    IL_0003:  add
    IL_0004:  stloc.0
    IL_0005:  br.s       IL_0007
```

```
IL_0007: ldloc.0
IL_0008: ret
} // end of method Calc::Add
```

Don’t worry if you are unable to make heads or tails of the resulting CIL for this method because Chapter 19 will describe the basics of the CIL programming language. The point to concentrate on is that the C# compiler emits CIL, not platform-specific instructions.

Now, recall that this is true of all .NET Core compilers. To illustrate, assume you created this same application using Visual Basic, rather than C#.

```
' Calc.vb
Namespace CalculatorExample
    Module Program
        ' This class contains the app's entry point.
        Sub Main(args As String())
            Dim c As New Calc
            Dim ans As Integer = c.Add(10, 84)
            Console.WriteLine("10 + 84 is {0}", ans)
            'Wait for user to press the Enter key before shutting down
            Console.ReadLine()
        End Sub
    End Module
    ' The VB.NET calculator.
    Class Calc
        Public Function Add(ByVal addend1 As Integer, ByVal addend2 As Integer) As Integer
            Return addend1 + addend2
        End Function
    End Class
End Namespace
```

If you examine the CIL for the Add() method, you find similar instructions (slightly tweaked by the Visual Basic compiler).

```
.method public instance int32 Add(int32 addend1,
    int32 addend2) cil managed
{
    // Code size       9 (0x9)
    .maxstack 2
    .locals init (int32 V_0)
    IL_0000: nop
    IL_0001: ldarg.1
    IL_0002: ldarg.2
    IL_0003: add.ovf
    IL_0004: stloc.0
    IL_0005: br.s       IL_0007

    IL_0007: ldloc.0
    IL_0008: ret
} // end of method Calc::Add
```

As a final example, the same simple Calc program developed in F# (another .NET Core language) is shown here:

```
// Learn more about F# at http://fsharp.org

// Calc.fs
open System

module Calc =
    let add addend1 addend2 =
        addend1 + addend2

[<EntryPoint>]
let main argv =
    let ans = Calc.add 10 84
    printfn "10 + 84 is %d" ans
    Console.ReadLine()
    0
```

If you examine the CIL for the Add() method, once again you find similar instructions (slightly tweaked by the F# compiler).

```
.method public static int32 Add(int32 addend1,
                                int32 addend2) cil managed
{
    .custom instance void [FSharp.Core]Microsoft.FSharp.Core.
CompilationArgumentCountsAttribute::.ctor(int32[]) = ( 01 00 02 00 00 00 01 00 00 00 01 00 00 00 00 00 )
    // Code size       4 (0x4)
    .maxstack 8
    IL_0000:  ldarg.0
    IL_0001:  ldarg.1
    IL_0002:  add
    IL_0003:  ret
} // end of method Calc::'add'
```

## Benefits of CIL

At this point, you might be wondering exactly what is gained by compiling source code into CIL rather than directly to a specific instruction set. One benefit is language integration. As you have already seen, each .NET Core compiler produces nearly identical CIL instructions. Therefore, all languages are able to interact within a well-defined binary arena.

Furthermore, given that CIL is platform-agnostic, the .NET Core Framework itself is platform-agnostic, providing the same benefits Java developers have grown accustomed to (e.g., a single code base running on numerous operating systems). In fact, there is an international standard for the C# language. Prior to .NET Core, there were numerous implementations of .NET for non-Windows platforms, such as Mono. These still exist, although the need for them is greatly reduced with the cross-platform capability of .NET Core.

## Compiling CIL to Platform-Specific Instructions

Because assemblies contain CIL instructions rather than platform-specific instructions, CIL code must be compiled on the fly before use. The entity that compiles CIL code into meaningful CPU instructions is a JIT compiler, which sometimes goes by the friendly name of jitter. The .NET Core runtime environment leverages a JIT compiler for each CPU targeting the runtime, each optimized for the underlying platform.

For example, if you are building a .NET Core application to be deployed to a handheld device (such as an iOS or Android phone), the corresponding jitter is well equipped to run within a low-memory environment. On the other hand, if you are deploying your assembly to a back-end company server (where memory is seldom an issue), the jitter will be optimized to function in a high-memory environment. In this way, developers can write a single body of code that can be efficiently JIT compiled and executed on machines with different architectures.

Furthermore, as a given jitter compiles CIL instructions into corresponding machine code, it will cache the results in memory in a manner suited to the target operating system. In this way, if a call is made to a method named PrintDocument(), the CIL instructions are compiled into platform-specific instructions on the first invocation and retained in memory for later use. Therefore, the next time PrintDocument() is called, there is no need to recompile the CIL.

## Precompiling CIL to Platform-Specific Instructions

There is a utility in .NET Core called crossgen.exe, which can be used to pre-JIT your code. Fortunately, in .NET Core 3.0, the ability to produce "ready-to-run" assemblies is built into the framework. More on this later in this book.

## The Role of .NET Core Type Metadata

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
