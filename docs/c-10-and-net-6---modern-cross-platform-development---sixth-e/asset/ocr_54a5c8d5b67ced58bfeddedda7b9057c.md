3. Modify the file to define a Calculator class (with a deliberate bug!), as shown in the following code:

```
namespace Packt
{
    public class Calculator
    {
        public double Add(double a, double b)
        {
            return a * b;
        }
    }
}
```

4. Compile your class library project:

+   1. In Visual Studio 2022, navigate to **Build | Build CalculatorLib**.
+   2. In Visual Studio Code, in **TERMINAL**, enter the command `dotnet build`.

5. Use your preferred coding tool to add a new xUnit Test Project [C#] to the Chapter04 workspace/solution named CalculatorLibUnitTests. The dotnet new template is named xunit.

6. If you are using Visual Studio, in **Solution Explorer**, select the CalculatorLibUnitTests project, navigate to **Project | Add Project Reference...**, check the box to select the CalculatorLib project, and then click OK.

7. If you are using Visual Studio Code, use the dotnet add reference command or click on the file named CalculatorLibUnitTests.csproj, and modify the configuration to add an item group with a project reference to the CalculatorLib project, as shown highlighted in the following markup:

```
<Project Sdk="Microsoft.NET.Sdk">

    <PropertyGroup>
        <TargetFramework>net6.0</TargetFramework>
        <Nullable>enable</Nullable>

        <IsPackable>false</IsPackable>
    </PropertyGroup>

    <ItemGroup>
        <PackageReference Include="Microsoft.NET.Test.Sdk" Version="16.10.0" />
        <PackageReference Include="xunit" Version="2.4.1" />
        <PackageReference Include="xunit.runner.visualstudio" Version="2.4.3">
            <IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>
            <PrivateAssets>all</PrivateAssets>
        </PackageReference>
        <PackageReference Include="coverlet.collector" Version="3.0.2">
```