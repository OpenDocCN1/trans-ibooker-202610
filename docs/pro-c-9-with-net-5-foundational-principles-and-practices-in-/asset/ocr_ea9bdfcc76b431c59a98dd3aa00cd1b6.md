> Note The Visual Studio property pages still refer to the root namespace as the Default namespace. You will see next why I refer to it as the root namespace.

If not using Visual Studio (or even with Visual Studio), you can also configure the root namespace by updating the project (*.csproj) file. With .NET Core projects, editing the project file in Visual Studio is as easy as double-clicking the project file in Solution Explorer (or right-clicking the project file in Solution Explorer and selecting “Edit project file”). Once the file is open, update the main PropertyGroup by adding the RootNamespace node, like this:

```
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net5.0</TargetFramework>
    <RootNamespace>CustomNamespaces2</RootNamespace>
  </PropertyGroup>

</Project>
```

So far, so good. Now that you have seen some details regarding how to package your custom types into well-organized namespaces, let’s quickly review the benefits and format of the .NET Core assembly. After this, you will delve into the details of creating, deploying, and configuring your custom class libraries.

# The Role of .NET Core Assemblies

.NET Core applications are constructed by piecing together any number of assemblies. Simply put, an assembly is a versioned, self-describing binary file hosted by the .NET Core Runtime. Now, despite that .NET Core assemblies have the same file extensions (*.exe or *.dll) as previous Windows binaries, they have little in common under the hood with those files. Before unpacking that last statement, let’s consider some of the benefits provided by the assembly format.

# Assemblies Promote Code Reuse

As you have built your Console Application projects over the previous chapters, it might have seemed that all the applications’ functionality was contained within the executable assembly you were constructing. Your applications were leveraging numerous types contained within the always-accessible .NET Core base class libraries.

As you might know, a code library (also termed a class library) is a *.dll that contains types intended to be used by external applications. When you are creating executable assemblies, you will no doubt be leveraging numerous system-supplied and custom code libraries as you create your application. Do be aware, however, that a code library need not take a *.dll file extension. It is perfectly possible (although certainly not common) for an executable assembly to use types defined within an external executable file. In this light, a referenced *.exe can also be considered a code library.

Regardless of how a code library is packaged, the .NET Core platform allows you to reuse types in a language-independent manner. For example, you could create a code library in C# and reuse that library in any other .NET Core programming language. It is possible not only to allocate types across languages but also to derive from them. A base class defined in C# could be extended by a class authored in Visual Basic.