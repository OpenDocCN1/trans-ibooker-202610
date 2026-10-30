
You are presented with the internal CLR header information for a given .NET Core assembly, as shown here:

```
Dump of file CarLibrary.dll
File Type: DLL

    clr Header:

        48 cb
        2.05 runtime version
        2158 [ B7C] RVA [size] of MetaData Directory
            1 flags
                IL Only
        0 entry point token
        0 [    0] RVA [size] of Resources Directory
        0 [    0] RVA [size] of StrongNameSignature Directory
        0 [    0] RVA [size] of CodeManagerTable Directory
        0 [    0] RVA [size] of VTableFixups Directory
        0 [    0] RVA [size] of ExportAddressTableJumps Directory
        0 [    0] RVA [size] of ManagedNativeHeader Directory

Summary

        2000 .reloc
        2000 .rsrc
        2000 .text
```

Again, as a .NET Core developer, you will not need to concern yourself with the gory details of an assembly’s CLR header information. Just understand that every .NET Core assembly contains this data, which is used behind the scenes by the .NET Core runtime as the image data loads into memory. Now turn your attention to some information that is much more useful in your day-to-day programming tasks.

## CIL Code, Type Metadata, and the Assembly Manifest

At its core, an assembly contains CIL code, which, as you recall, is a platform- and CPU-agnostic intermediate language. At runtime, the internal CIL is compiled on the fly using a just-in-time (JIT) compiler, according to platform- and CPU-specific instructions. Given this design, .NET Core assemblies can indeed execute on a variety of architectures, devices, and operating systems. (Although you can live a happy and productive life without understanding the details of the CIL programming language, Chapter 19 offers an introduction to the syntax and semantics of CIL.)

An assembly also contains metadata that completely describes the format of the contained types, as well as the format of external types referenced by this assembly. The .NET Core runtime uses this metadata to resolve the location of types (and their members) within the binary, lay out types in memory, and facilitate remote method invocations. You’ll check out the details of the .NET metadata format in Chapter 17 during your examination of reflection services.

An assembly must also contain an associated manifest (also referred to as assembly metadata). The manifest documents each module within the assembly, establishes the version of the assembly, and documents any external assemblies referenced by the current assembly. As you will see over the course of this chapter, the CLR makes extensive use of an assembly’s manifest during the process of locating external assembly references.

## Optional Assembly Resources

Finally, a .NET Core assembly may contain any number of embedded resources, such as application icons, image files, sound clips, or string tables. In fact, the .NET Core platform supports satellite assemblies that contain nothing but localized resources. This can be useful if you want to partition your resources based on a specific culture (English, German, etc.) for the purposes of building international software. The topic of building satellite assemblies is outside the scope of this text. Consult the .NET Core documentation for information on satellite assemblies and localization if you are interested.

## Class Libraries vs. Console Applications

So far in this book, the examples were almost exclusively .NET Core Console applications. If you are reading this book as a current .NET developer, these are like .NET console applications, with the main difference being the configuration process (to be covered later) and, of course, that they run on .NET Core. Console applications have a single-entry point (either a specified Main() method or top-level statements), can interact with the console, and can be launched directly from the operating system. Another difference between .NET Core and .NET console applications is that console applications in .NET Core are launched using the .NET Core Application Host (dotnet.exe).

Class libraries, on the other hand, don’t have an entry point and therefore cannot be launched directly. They are used to encapsulate logic, custom types, and so on, and they are referenced by other class libraries and/or console applications. In other words, class libraries are used to contain the things talked about in “The Role of .NET Core Assemblies” section.

## .NET Standard vs. .NET Core Class Libraries

.NET Core class libraries run on .NET Core, and .NET class libraries run on .NET. Pretty straightforward. However, there is a problem with this. Assume you have a large .NET code base in your organization, with (potentially) years of development behind you and your team. There is probably a significant amount of shared code leveraged by the applications you and your team have built over the years. Perhaps it’s centralized logging, reporting, or domain-specific functionality.

Now you (and your organization) want to move to .NET Core for all new development. What about all that shared code? The effort to rewrite all of your legacy code into .NET Core assemblies could be significant, and until all of your applications were moved to .NET Core, you would have to support two versions (one in .NET and one in .NET Core). That would bring productivity to a screeching halt.

Fortunately, the builders of .NET Core thought through this scenario. .NET Standard is a new type of class library project that was introduced with .NET Core and can be referenced by .NET as well as .NET Core applications. Before you get your hopes up, though, there is a catch with .NET (Core) 5. More on that shortly.

Each .NET Standard version defines a common set of APIs that must be supported by all .NET versions (.NET, .NET Core, Xamarin, etc.) to conform to the standard. For example, if you were to build a class library as a .NET Standard 2.0 project, it can be referenced by .NET 4.6.1+ and .NET Core 2.0+ (plus various versions of Xamarin, Mono, Universal Windows Platform, and Unity).

This means you could move the code from your .NET class libraries into .NET Standard 2.0 class libraries, and they can be shared by .NET Core and .NET applications. That’s much better than supporting duplicate copies of the same code, one for each framework.

Now for the catch. Each .NET Standard version represents the lowest common denominator for the frameworks that it supports. That means the lower the version, the less that you can do in your class library. While .NET (Core) 5 and .NET Core 3.1 can both reference a .NET Standard 2.0 library, you cannot use a significant number of C# 8.0 (or any C# 9.0) features in a .NET Standard 2.0 library. You must use .NET Standard 2.1 for full C# 8.0 and C# 9.0 support. And .NET 4.8 (the latest/last version of the original .NET Framework) only goes up to .NET Standard 2.0. It’s still a good mechanism for leveraging existing code in newer applications, but not a silver bullet.

## Configuring Applications

While it is possible to keep all the information needed by your .NET Core application in the source code, being able to change certain values at runtime is vitally important in most applications of significance. This is typically done through a configuration file that is shipped with your application.

> Note The previous .NET Framework relied mostly on XML files named app.config (or web.config for ASP.NET applications). While XML-based configuration files can still be used, the main method for configuring .NET Core applications is with JavaScript Object Notation (JSON) files as shown in this section. Configuration will be addressed in depth in the chapters in WPF and ASP.NET Core.

To illustrate the process, create a new .NET Core Console application named FunWithConfiguration and add the following package reference to your project:

```
dotnet new console -lang c# -n FunWithConfiguration -o .\FunWithConfiguration -f net5.0
dotnet add FunWithConfiguration package Microsoft.Extensions.Configuration.Json
```

This adds a reference for the JSON file-based .NET Core configuration subsystem (and its dependencies) into your project. To leverage this, first add a new JSON file into your project named appsettings.json. Update the project file to make sure the file is always copied to the output directory when the project is built.

```
<ItemGroup>
    <None Update="appsettings.json">
        <CopyToOutputDirectory>Always</CopyToOutputDirectory>
    </None>
</ItemGroup>
```

Finally, update the file to match the following:

```
{
  "CarName": "Suzy"
}
```

> Note If you are not familiar with JSON, it is a name-value pair format with each object enclosed in curly braces. The entire file can be read as a single object, and subobjects are also marked with curly braces. Later in this book, you will work with more complex JSON files.

The final step is to read in the configuration file and get the CarName value. Update the using statements in Program.cs to the following:

```
using System;
using System.IO;
using Microsoft.Extensions.Configuration;
```
