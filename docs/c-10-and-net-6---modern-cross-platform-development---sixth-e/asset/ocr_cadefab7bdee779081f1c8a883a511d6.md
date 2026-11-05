So, from .NET 6 onward, Microsoft will include preview features in general availability (GA) releases. Developers can opt into these preview features and provide Microsoft with feedback. In a later GA release, they can be enabled for everyone.

> **Good Practice:** Preview features are not supported in production code.
> Preview features are likely to have breaking changes before the final release.
> Enable preview features at your own risk.

# Requiring preview features

The [RequiresPreviewFeatures] attribute is used to indicate assemblies, types, or members that use and therefore require warnings about preview features. A code analyzer then scans for this assembly and generates warnings if needed. If your code does not use any preview features, you will not see any warnings. If you use any preview features, then your code should warn consumers of your code that you use preview features.

# Enabling preview features

Let's look at an example of a preview feature available in .NET 6, the ability to define an interface with a static abstract method:

+   1. Use your preferred code editor to add a new console app named `UsingPreviewFeatures` to the Chapter07 solution/workspace.
+   2. In Visual Studio Code, select UsingPreviewFeatures as the active OmniSharp project. When you see the pop-up warning message saying that required assets are missing, click **Yes** to add them.

3. In the project file, add an element to enable preview features and an element to enable preview language features, as shown highlighted in the following markup:

```
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net6.0</TargetFramework>
    <Nullable>enable</Nullable>
    <ImplicitUsings>enable</ImplicitUsings>
    <EnablePreviewFeatures>true</EnablePreviewFeatures>
    <LangVersion>preview</LangVersion>
  </PropertyGroup>
</Project>
```

4. In Program.cs, delete the comment and statically import the Console class.