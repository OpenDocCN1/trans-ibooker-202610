2. In Program.cs, add statements to add two children to Sam, and then access the first and second child using the longer Children field and the shorter indexer syntax, as shown in the following code:

```
sam.Children.Add(new() { Name = "Charlie" });
sam.Children.Add(new() { Name = "Ella" });

WriteLine($"Sam's first child is {sam.Children[0].Name}");
WriteLine($"Sam's second child is {sam.Children[1].Name}");

WriteLine($"Sam's first child is {sam[0].Name}");
WriteLine($"Sam's second child is {sam[1].Name}");
```

3. Run the code and view the result, as shown in the following output:

```
Sam's first child is Charlie
Sam's second child is Ella
Sam's first child is Charlie
Sam's second child is Ella
```

# Pattern matching with objects

In Chapter 3, Controlling Flow, Converting Types, and Handling Exceptions, you were introduced to basic pattern matching. In this section, we will explore pattern matching in more detail.

# Creating and referencing a .NET 6 class library

The enhanced pattern matching features are only available in modern .NET class libraries that support C# 9 or later.

+   1. Use your preferred coding tool to add a new class library named PacktLibraryModern to the workspace/solution named Chapter05.
+   2. In the PeopleApp project, add a reference to the PacktLibraryModern class library, as shown highlighted in the following markup:

```
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net6.0</TargetFramework>
    <Nullable>enable</Nullable>
    <ImplicitUsings>enable</ImplicitUsings>
  </PropertyGroup>

  <ItemGroup>
    <ProjectReference Include="../PacktLibrary/PacktLibrary.csproj" />
  </ItemGroup>
</Project>
```