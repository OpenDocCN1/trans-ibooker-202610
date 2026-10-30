
```
<AssemblyVersion>1.0.0.1</AssemblyVersion>
<FileVersion>1.0.0.2</FileVersion>
<Version>1.0.0.3</Version>
</PropertyGroup>
```

> **Note** The rest of the entries from Figure 16-4 (and the project file listing) are used when generating NuGet packages from your assembly. This is covered later in the chapter.

## Exploring the CIL

Recall that an assembly does not contain platform-specific instructions; rather, it contains platform-agnostic Common Intermediate Language (CIL) instructions. When the .NET Core runtime loads an assembly into memory, the underlying CIL is compiled (using the JIT compiler) into instructions that can be understood by the target platform. For example, the TurboBoost() method of the SportsCar class is represented by the following CIL:

```
.method public hidebysig virtual
    instance void TurboBoost() cil managed
{
    .maxstack 8
    IL_0000: nop
    IL_0001: ldstr "Ramming speed! Faster is better..."
    IL_0006: call void [System.Console]System.Console::WriteLine(string)
    IL_000b: nop
    IL_000c: ret
}
// end of method SportsCar::TurboBoost
```

As with the other CIL examples in this book, most .NET Core developers don’t need to be deeply concerned with the details. Chapter 19 provides more details on its syntax and semantics, which can be helpful when you are building more complex applications that require advanced services, such as runtime construction of assemblies.

## Exploring the Type Metadata

Before you build some applications that use your custom .NET library, examine the metadata for the types within the CarLibrary.dll assembly. For an example, here is the TypeDef for the EngineStateEnum:

```
TypeDef #1 (02000002)
--------------------------------------------------
TypeDefName: CarLibrary.EngineStateEnum
Flags      : [Public] [AutoLayout] [Class] [Sealed] [AnsiClass]
Extends    : [TypeRef] System.Enum
Field #1
--------------------------------------------------
Field Name: value__
Flags      : [Public] [SpecialName] [RTSpecialName]
CallCnvntn: [FIELD]
Field type: I4

Field #2
--------------------------------------------------
Field Name: EngineAlive
Flags      : [Public] [Static] [Literal] [HasDefault]
DefltValue: (I4) 0
CallCnvntn: [FIELD]
Field type: ValueClass CarLibrary.EngineStateEnum

Field #3
--------------------------------------------------
Field Name: EngineDead
Flags      : [Public] [Static] [Literal] [HasDefault]
DefltValue: (I4) 1
CallCnvntn: [FIELD]
Field type: ValueClass CarLibrary.EngineStateEnum
```

As explained in the next chapter, an assembly’s metadata is an important element of the .NET Core platform and serves as the backbone for numerous technologies (object serialization, late binding, extendable applications, etc.). In any case, now that you have looked inside the CarLibrary.dll assembly, you can build some client applications that use your types.

## Building a C# Client Application

Because each of the CarLibrary types has been declared using the public keyword, other .NET Core applications are able to use them as well. Recall that you may also define types using the C# internal keyword (in fact, this is the default C# access mode for classes). Internal types can be used only by the assembly in which they are defined. External clients can neither see nor create types marked with the internal keyword.

> **Note** The exception to this rule is when an assembly explicitly allows access to another assembly using the InternalsVisibleTo attribute, covered shortly.

To use your library’s functionality, create a new C# Console Application project named CSharpCarClient in the same solution as CarLibrary. You can do this using Visual Studio (right-click the solution and select Add ▶ New Project) or using the command line (three lines, each executed separately).

```
dotnet new console -lang c# -n CSharpCarClient -o .\CSharpCarClient -f net5.0
dotnet add CSharpCarClient reference CarLibrary
dotnet sln .\Chapter16_AllProjects.sln add .\CSharpCarClient
```

The previous commands created the console application, added a project reference to the CarLibrary project for the new project, and added it to your solution.

> **Note** The add reference command creates a project reference. This is convenient for development, as CSharpCarClient will always be using the latest version of CarLibrary. You can also reference an assembly directly. Direct references are created by referencing the compiled class library.

If you still have the solution open in Visual Studio, you’ll notice that the new project shows up in Solution Explorer without any intervention on your part.

The final change to make is to right-click CSharpCarClient in Solution Explorer and select “Set as Startup Project.” If you are not using Visual Studio, you can run the new project by executing dotnet run in the project directory.

> **Note** You can set the project reference in Visual Studio as well by right-clicking the CSharpCarClient project in Solution Explorer, selecting Add ▶ Reference, and selecting the CarLibrary project from the project’s node.

At this point, you can build your client application to use the external types. Update your initial C# file as follows:

```
using System;
// Don't forget to import the CarLibrary namespace!
using CarLibrary;

Console.WriteLine("***** C# CarLibrary Client App *****");
// Make a sports car.
SportsCar viper = new SportsCar("Viper", 240, 40);
viper.TurboBoost();

// Make a minivan.
MiniVan mv = new MiniVan();
mv.TurboBoost();

Console.WriteLine("Done. Press any key to terminate");
Console.ReadLine();
```

This code looks just like the code of the other applications developed thus far in the book. The only point of interest is that the C# client application is now using types defined within a separate custom library. Run your program and verify that you see the display of messages.

You might be wondering exactly what happened when you referenced the CarLibrary project. When a project reference is made, the solution build order is adjusted so that dependent projects (CarLibrary in this example) build first, and then the output from that build is copied into the output directory of the parent project (CSharpCarClient). The compiled client library references the compiled class library. When the client project is rebuilt, so is the dependent library, and the new version is once again copied to the target folder.

> **Note** If you are using Visual Studio, you can click the Show All Files button in Solution Explorer, and you can see all of the output files and verify that the compiled CarLibrary is there. If you are using Visual Studio Code, navigate to the bin/debug/net5.0 directory in the Explorer tab.

When a direct reference is made, the compiled library is also copied to the output directory of the client library, but at the time the reference is made. Without the project reference in place, the projects can be built independently of each other, and the files could become out of sync. In short, if you are developing dependent libraries (as is usually the case with real software projects), it is best to reference the project and not the project output.

## Building a Visual Basic Client Application

Recall that the .NET Core platform allows developers to share compiled code across programming languages. To illustrate the language-agnostic attitude of the .NET Core platform, let’s create another Console Application project (VisualBasicCarClient), this time using Visual Basic (note that each of commands is a one-line command).

```
dotnet new console -lang vb -n VisualBasicCarClient -o .\VisualBasicCarClient -f net5.0
dotnet add VisualBasicCarClient reference CarLibrary
dotnet sln .\Chapter16_AllProjects.sln add VisualBasicCarClient
```

Like C#, Visual Basic allows you to list each namespace used within the current file. However, Visual Basic offers the Imports keyword rather than the C# using keyword, so add the following Imports statement within the Program.vb code file:

```
Imports CarLibrary
Module Program
    Sub Main()
    End Sub
End Module
```

Notice that the Main() method is defined within a Visual Basic module type. In a nutshell, modules are a Visual Basic notation for defining a class that can contain only static methods (much like a C# static class). In any case, to exercise the MiniVan and SportsCar types using the syntax of Visual Basic, update your Main() method as follows:

```
Sub Main()
    Console.WriteLine("***** VB CarLibrary Client App *****")
    ' Local variables are declared using the Dim keyword.
    Dim myMiniVan As New MiniVan()
    myMiniVan.TurboBoost()

    Dim mySportsCar As New SportsCar()
    mySportsCar.TurboBoost()
    Console.ReadLine()
End Sub
```

When you compile and run your application (make sure to set VisualBasicCarClient as the startup project in Visual Studio), you will once again find a series of messages displayed. Furthermore, this new client application has its own local copy of CarLibrary.dll located under the bin\Debug\net5.0 folder.

## Cross-Language Inheritance in Action

An enticing aspect of .NET Core development is the notion of cross-language inheritance. To illustrate, let’s create a new Visual Basic class that derives from SportsCar (which was authored using C#). First, add a new class file named PerformanceCar.vb to your current Visual Basic application. Update the initial class definition by deriving from the SportsCar type using the Inherits keyword. Then, override the abstract TurboBoost() method using the Overrides keyword, like so:

```
Imports CarLibrary
' This VB class is deriving from the C# SportsCar.
Public Class PerformanceCar
```
