

编译项目后，导航到 `\obj\Debug\net5.0` 目录，并查找 `AttributedCarLibrary.AssemblyInfo.cs` 文件。打开该文件，你将看到这些属性以特性的形式存在（遗憾的是，这种格式的可读性不是很好）：

```
using System;
using System.Reflection;
```

```
[assembly: System.Runtime.CompilerServices.InternalsVisibleToAttribute("CSharpCarClient")]
[assembly: System.Reflection.AssemblyCompanyAttribute("Philip Japikse")]
[assembly: System.Reflection.AssemblyConfigurationAttribute("Debug")]
[assembly: System.Reflection.AssemblyDescriptionAttribute("This is a sample car library with attributes")]
[assembly: System.Reflection.AssemblyFileVersionAttribute("1.0.0.0")]
[assembly: System.Reflection.AssemblyInformationalVersionAttribute("1.0.0")]
[assembly: System.Reflection.AssemblyProductAttribute("AttributedCarLibrary")]
[assembly: System.Reflection.AssemblyTitleAttribute("AttributedCarLibrary")]
[assembly: System.Reflection.AssemblyVersionAttribute("1.0.0.0")]
```

最后，关于程序集特性需要说明的是，如果你希望自行管理这一过程，可以关闭 `AssemblyInfo.cs` 类的生成。

## 使用早期绑定反射特性

请记住，在其他软件通过反射读取特性的值之前，特性几乎没有任何作用。一旦发现某个特性，相关软件就可以采取任何必要的操作。现在，与其他应用程序一样，这个“其他软件”可以使用早期绑定或晚期绑定来发现自定义特性的存在。如果希望使用早期绑定，则需要客户端应用程序在编译时定义相关特性（本例中为 `VehicleDescriptionAttribute`）。由于 `AttributedCarLibrary` 程序集已将此自定义特性定义为公共类，因此早期绑定是最佳选择。

为了演示如何反射自定义特性，请向解决方案中添加一个名为 `VehicleDescriptionAttributeReader` 的新 C# 控制台应用程序项目。接下来，添加对 `AttributedCarLibrary` 项目的引用。使用 CLI 执行以下命令（每条命令必须独占一行）：

```
dotnet new console -lang c# -n VehicleDescriptionAttributeReader -o .\
VehicleDescriptionAttributeReader -f net5.0
dotnet sln .\Chapter17_AllProjects.sln add .\VehicleDescriptionAttributeReader
dotnet add VehicleDescriptionAttributeReader reference .\AttributedCarLibrary
```

使用以下代码更新 `Program.cs` 文件：

```
using System;
using AttributedCarLibrary;
```

```
Console.WriteLine("***** Value of VehicleDescriptionAttribute *****
");
ReflectOnAttributesUsingEarlyBinding();
Console.ReadLine();
```

```
static void ReflectOnAttributesUsingEarlyBinding()
```
```
{ // Get a Type representing the Winnebago. Type t = typeof(Winnebago); // Get all attributes on the Winnebago. object[] customAtts = t.GetCustomAttributes(false); // Print the description. foreach (VehicleDescriptionAttribute v in customAtts) { Console.WriteLine("-> {0}\n", v.Description); } }
```

`Type.GetCustomAttributes()` 方法返回一个对象数组，其中包含应用于 `Type` 所表示成员的全部特性（布尔参数控制搜索是否应沿继承链向上扩展）。获取特性列表后，遍历每个 `VehicleDescriptionAttribute` 类，并输出通过 `Description` 属性获得的值。

## 使用晚期绑定反射特性

前面的示例使用早期绑定输出了 `Winnebago` 类型的车辆描述数据。之所以能够做到这一点，是因为 `VehicleDescriptionAttribute` 类类型在 `AttributedCarLibrary` 程序集中被定义为公共成员。还可以使用动态加载和晚期绑定来反射特性。

向解决方案中添加一个名为 `VehicleDescriptionAttributeReaderLateBinding` 的新项目，将其设置为启动项目，并将 `AttributedCarLibrary.dll` 复制到该项目的文件夹中（如果使用 Visual Studio，则复制到 `\bin\Debug\net5.0`）。现在，按如下方式更新 `Program` 类：

```
using System; using System.Reflection; Console.WriteLine("***** Value of VehicleDescriptionAttribute ****\n"); ReflectAttributesUsingLateBinding(); Console.ReadLine(); static void ReflectAttributesUsingLateBinding() { try { // Load the local copy of AttributedCarLibrary. Assembly asm = Assembly.LoadFrom("AttributedCarLibrary"); // Get type info of VehicleDescriptionAttribute. Type vehicleDesc = asm.GetType("AttributedCarLibrary.VehicleDescriptionAttribute"); // Get type info of the Description property. PropertyInfo propDesc = vehicleDesc.GetProperty("Description");
```

```
// Get all types in the assembly.
Type[] types = asm.GetTypes();

// Iterate over each type and obtain any VehicleDescriptionAttributes.
foreach (Type t in types)
{
    object[] objs = t.GetCustomAttributes(vehicleDesc, false);

    // Iterate over each VehicleDescriptionAttribute and print
    // the description using late binding.
    foreach (object o in objs)
    {
        Console.WriteLine("-> {0}: {1}\n", t.Name,
            propDesc.GetValue(o, null));
    }
}
catch (Exception ex)
{
    Console.WriteLine(ex.Message);
}
```

如果你能够跟随本章中的示例进行操作，那么这段代码应该（大体上）是不言自明的。唯一值得注意的是 `PropertyInfo.GetValue()` 方法的使用，该方法用于触发属性的访问器。下面是当前示例的输出：

```
***** Value of VehicleDescriptionAttribute *****
-> Motorcycle: My rocking Harley

-> HorseAndBuggy: The old gray mare, she ain't what she used to be...

-> Winnebago: A very long, slow, but feature-rich auto
```

## 综合理解反射、晚期绑定和自定义特性

尽管你已经看到许多这些技术实际应用的示例，但你可能仍然会思考：在自己的程序中，何时应该使用反射、动态加载、晚期绑定和自定义特性。诚然，从编程角度来看，这些主题可能显得有些偏学术化（不过这是否算坏事，取决于你的观点）。为了帮助你将这些主题与现实场景联系起来，我们需要一个扎实的示例。暂且假设你所在的编程团队正在构建一个具有以下要求的应用程序：

产品必须能够通过额外的第三方工具进行扩展。

“可扩展”究竟是什么意思？可以考虑一下 Visual Studio IDE。当开发该应用程序时，开发人员在代码库中插入了各种“挂钩”，以便其他软件供应商能够将自定义模块“插入”（或加载）到 IDE 中。显然，Visual Studio 开发团队不可能为尚未开发的外部 .NET 程序集设置引用（因此无法使用早期绑定），那么应用程序究竟应该如何提供所需的挂钩呢？下面是一种可行的解决方案：

- 1. 首先，可扩展应用程序必须提供某种输入机制，以允许用户指定要插入的模块（例如对话框或命令行标志）。这需要使用*动态加载*。
- 2. 其次，可扩展应用程序必须能够确定模块是否支持正确的功能（例如一组必需的接口），从而判断它是否可以插入环境。这需要使用*反射*。
- 3. 最后，可扩展应用程序必须获取对所需基础设施（例如一组接口类型）的引用，并调用这些成员来触发底层功能。这可能需要使用*晚期绑定*。

简单来说，如果可扩展应用程序已经预先编程为查询特定接口，那么它就能够在运行时确定某个类型是否可以被激活。一旦通过此验证测试，相关类型可能还支持其他接口，而这些接口则为其功能提供多态结构。这正是 Visual Studio 团队采用的方法，而且尽管你可能会这样想，它一点也不难！

## 构建可扩展应用程序

在接下来的章节中，我将通过一个示例，带你了解如何构建一个能够借助外部程序集功能进行扩展的应用程序。作为路线图，可扩展应用程序包含以下程序集：

- CommonSnappableTypes.dll：此程序集包含每个插件对象都将使用的类型定义，并且 Windows Forms 应用程序会直接引用该程序集。
- CSharpSnapIn.dll：一个使用 C# 编写的插件，它利用 `CommonSnappableTypes.dll` 中的类型。
- VBSnapIn.dll：一个使用 Visual Basic 编写的插件，它利用 `CommonSnappableTypes.dll` 中的类型。
- MyExtendableApp.exe：一个控制台应用程序，可以通过各个插件的功能进行扩展。

此应用程序将使用动态加载、反射和晚期绑定，动态获得它之前并不了解的程序集所提供的功能。

> 注意 你可能会想：“我的老板从来没有要求我构建控制台应用程序。”你很可能是对的！使用 C# 构建的业务线应用程序通常属于智能客户端（WinForms 或 WPF）、Web 应用程序/RESTful 服务（ASP.NET Core），或无头进程（Azure Functions、Windows 服务等）。我们使用控制台应用程序，是为了专注于本示例中的特定概念，即动态加载、反射和晚期绑定。在本书后面的内容中，你将使用 WPF 和 ASP.NET Core 探索面向“真实”用户的应用程序。

