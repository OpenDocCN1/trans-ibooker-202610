
2. 在 Visual Studio Code 中，选择 WorkingWithReflection 作为活动的 OmniSharp 项目。

3. 在 Program.cs 顶部，导入反射的命名空间，如以下代码所示：

```
using System.Reflection; // Assembly
```

4. 添加语句以获取控制台应用程序的程序集，输出其名称和位置，并获取所有程序集级属性并输出其类型，如以下代码所示：

```
WriteLine("Assembly metadata:");
Assembly? assembly = Assembly.GetEntryAssembly();
if (assembly is null)
{
    WriteLine("Failed to get entry assembly.");
    return;
}

WriteLine($"  Full name: {assembly.FullName}");
WriteLine($"  Location: {assembly.Location}");

IEnumerable<Attribute> attributes = assembly.GetCustomAttributes();

WriteLine($"  Assembly-level attributes:");
foreach (Attribute a in attributes)
{
    WriteLine($"    {a.GetType()}");
}
```

5. 运行代码并查看结果，如以下输出所示：

```
Assembly metadata:
    Full name: WorkingWithReflection, Version=1.0.0.0, Culture=neutral,
PublicKeyToken=null
    Location: /Users/markjprice/Code/Chapter08/WorkingWithReflection/bin/
Debug/net6.0/WorkingWithReflection.dll
    Assembly-level attributes:
    System.Runtime.CompilerServices.CompilationRelaxationsAttribute
    System.Runtime.CompilerServices.RuntimeCompatibilityAttribute
    System.Diagnostics.DebuggableAttribute
    System.Runtime.Versioning.TargetFrameworkAttribute
    System.Reflection.AssemblyCompanyAttribute
    System.Reflection.AssemblyConfigurationAttribute
    System.Reflection.AssemblyFileVersionAttribute
    System.Reflection.AssemblyInformationalVersionAttribute
    System.Reflection.AssemblyProductAttribute
    System.Reflection.AssemblyTitleAttribute
```

请注意，因为程序集的全名必须唯一标识程序集，所以它是由以下内容组合而成的：

- **名称**，例如，`WorkingWithReflection`
- **版本**，例如，`1.0.0.0`
- **区域性**，例如，`neutral`
- **公钥令牌**，尽管它可以为 `null`

现在我们知道了装饰程序集的一些属性，可以具体查询它们。

6. 添加语句以获取 AssemblyInformationalVersionAttribute 和 AssemblyCompanyAttribute 类，然后输出它们的值，如以下代码所示：

```
AssemblyInformationalVersionAttribute? version = assembly
    .GetCustomAttribute<AssemblyInformationalVersionAttribute>();

WriteLine($"  Version: {version?.InformationalVersion}");

AssemblyCompanyAttribute? company = assembly
    .GetCustomAttribute<AssemblyCompanyAttribute>();

WriteLine($"  Company: {company?.Company}");
```

7. 运行代码并查看结果，如以下输出所示：

```
Version: 1.0.0
Company: WorkingWithReflection
```

嗯，除非设置版本，否则默认为 1.0.0，除非设置公司，否则默认为程序集的名称。让我们显式设置此信息。旧版 .NET Framework 设置这些值的方式是在 C# 源代码文件中添加属性，如以下代码所示：

```
[assembly: AssemblyCompany("Packt Publishing")]
[assembly: AssemblyInformationalVersion("1.3.0")]
```

.NET 使用的 Roslyn 编译器会自动设置这些属性，因此我们不能使用旧方法。相反，它们必须在项目文件中设置。

8. 编辑 WorkingWithReflection.csproj 项目文件以添加版本和公司的元素，如以下标记中突出显示所示：

```
<Project Sdk="Microsoft.NET.Sdk">

    <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>net6.0</TargetFramework>
<Nullable>enable</Nullable>
<ImplicitUsings>enable</ImplicitUsings>
<Version>6.3.12</Version>
<Company>Packt Publishing</Company>
</PropertyGroup>

</Project>
```

9. 运行代码并查看结果，如以下输出所示：

```
Version: 6.3.12
Company: Packt Publishing
```

## 创建自定义属性

你可以通过继承 Attribute 类来定义自己的属性：

1. 在项目中添加一个名为 CoderAttribute.cs 的类文件。

2. 定义一个属性类，该属性类可以用两个属性装饰类或方法，以存储编码人员的姓名和他们上次修改代码的日期，如以下代码所示：

```
namespace Packt.Shared;

[AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, AllowMultiple = true)]
public class CoderAttribute : Attribute
{
    public string Coder { get; set; }
    public DateTime LastModified { get; set; }

    public CoderAttribute(string coder, string lastModified)
    {
        Coder = coder;
        LastModified = DateTime.Parse(lastModified);
    }
}
```

3. 在 Program.cs 中，导入一些命名空间，如以下代码所示：

```
using System.Runtime.CompilerServices; // CompilerGeneratedAttribute
using Packt.Shared; // CoderAttribute
```

4. 在 Program.cs 底部，添加一个包含方法的类，并使用有关两位编码人员的数据用 Coder 属性装饰该方法，如以下代码所示：

```
class Animal
{
    [Coder("Mark Price", "22 August 2021")]
    [Coder("Johnni Rasmussen", "13 September 2021")]
    public void Speak()
    {
        WriteLine("Woof...");
    }
}
```

5. 在 Program.cs 中，在 Animal 类上方，添加代码以获取类型，枚举其成员，读取这些成员上的任何 Coder 属性并输出信息，如以下代码所示：

```
WriteLine();
WriteLine($"** Types:");
Type[] types = assembly.GetTypes();

foreach (Type type in types)
{
    WriteLine();
    WriteLine($"Type: {type.FullName}");
    MemberInfo[] members = type.GetMembers();

    foreach (MemberInfo member in members)
    {
        WriteLine("{0}: {1} ({2})",
            arg0: member.MemberType,
            arg1: member.Name,
            arg2: member.DeclaringType?.Name);

        IOrderedEnumerable<CoderAttribute> coders =
            member.GetCustomAttributes<CoderAttribute>()
            .OrderByDescending(c => c.LastModified);

        foreach (CoderAttribute coder in coders)
        {
            WriteLine("-> Modified by {0} on {1}",
                coder.Coder, coder.LastModified.ToShortDateString());
        }
    }
}
```

6. 运行代码并查看结果，如以下部分输出所示：

```
* Types:
...
```

```
Type: Animal
Method: Speak (Animal)
-> Modified by Johnni Rasmussen on 13/09/2021
-> Modified by Mark Price on 22/08/2021
Method: GetType (Object)
Method: ToString (Object)
Method: Equals (Object)
Method: GetHashCode (Object)
Constructor: .ctor (Program)
...
Type: <Program>$+<>c
Method: GetType (Object)
Method: ToString (Object)
Method: Equals (Object)
Method: GetHashCode (Object)
Constructor: .ctor (<<>c)
Field: <>9 (<<>c)
Field: <>9__0_0 (<<>c)
```

<Program>$+<>c 类型是什么？

它是一个编译器生成的显示类。<> 表示编译器生成，c 表示显示类。它们是编译器的未记录实现细节，可能会随时更改。你可以忽略它们，因此作为可选挑战，添加语句到你的控制台应用程序中，通过跳过用 CompilerGeneratedAttribute 装饰的类型来过滤编译器生成的类型。

## 使用反射做更多事情

这只是反射所能实现的功能的冰山一角。我们仅使用反射从代码中读取元数据。反射还可以执行以下操作：

- **动态加载当前未引用的程序集：** https://docs.microsoft.com/en-us/dotnet/standard/assembly/unloadability
- **动态执行代码：** https://docs.microsoft.com/en-us/dotnet/api/system.reflection.methodbase.invoke
- **动态生成新的代码和程序集：** https://docs.microsoft.com/en-us/dotnet/api/system.reflection.emit.assemblybuilder

## 处理图像

ImageSharp 是一个第三方跨平台 2D 图形库。在 .NET Core 1.0 开发期间，社区对缺少用于处理 2D 图像的 System.Drawing 命名空间提出了负面反馈。
