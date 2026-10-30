

```
-> Name, Version: System.Console:5.0.0.0
-> Name, Version: System.Threading:5.0.0.0
-> Name, Version: System.Text.Encoding.Extensions:5.0
```

现在明白了，已加载程序集的列表会在你编写新的 C# 代码时随时发生变化。例如，假设你已更新 `ListAllAssembliesInAppDomain()` 方法，使其使用 LINQ 查询，按名称对已加载程序集进行排序，如下所示：

```
using System.Linq;
static void ListAllAssembliesInAppDomain()
{
    // Get access to the AppDomain for the current thread.
    AppDomain defaultAD = AppDomain.CurrentDomain;

    // Now get all loaded assemblies in the default AppDomain.
    var loadedAssemblies =
        defaultAD.GetAssemblies().OrderBy(x=>x.GetName().Name);
    Console.WriteLine("***** Here are the assemblies loaded in {0} ****\n", defaultAD.FriendlyName);
    foreach(Assembly a in loadedAssemblies)
    {
        Console.WriteLine($"-> Name, Version: {a.GetName().Name}:{a.GetName().Version}" );
    }
}
```

如果再次运行程序，你会看到 `System.Linq.dll` 也已加载到内存中。

```
** Here are the assemblies loaded in DefaultAppDomainApp **
-> Name, Version: DefaultAppDomainApp:1.0.0.0
-> Name, Version: System.Console:5.0.0.0
-> Name, Version: System.Linq:5.0.0
-> Name, Version: System.Private.CoreLib:5.0.0.0
-> Name, Version: System.Runtime:5.0.0.0
-> Name, Version: System.Text.Encoding.Extensions:5.0.0.0
-> Name, Version: System.Threading:5.0.0
```

## 使用应用程序加载上下文进行程序集隔离

正如你刚才所见，AppDomain 是用于托管 .NET Core 程序集的逻辑分区。此外，一个应用程序域还可以进一步细分为多个加载上下文边界。从概念上讲，加载上下文为加载、解析以及可能卸载一组程序集创建了一个作用域。简而言之，.NET Core 加载上下文提供了一种方式，使单个 AppDomain 能够为给定对象建立一个“特定位置”。

> 注意 虽然理解进程和应用程序域非常重要，但大多数 .NET Core 应用程序永远不需要你处理对象上下文。我只是加入了这部分概述材料，以便呈现更完整的图景。

`AssemblyLoadContext` 类提供了将额外程序集加载到各自上下文中的能力。为了演示，首先添加一个名为 `ClassLibrary1` 的类库项目并将其添加到当前解决方案中。使用 .NET Core CLI，在包含当前解决方案的目录中执行以下命令：

```
dotnet new classlib -lang c# -n ClassLibrary1 -o .\ClassLibrary1 -f net5.0
dotnet sln .\chapter14_AllProjects.sln add .\ClassLibrary1
```

接下来，通过执行以下 CLI 命令，从 `DefaultAppDomainApp` 添加对 `ClassLibrary1` 项目的引用：

```
dotnet add DefaultAppDomainApp reference ClassLibrary1
```

如果你使用的是 Visual Studio，在解决方案资源管理器中右键单击解决方案节点，选择 添加 ▶ 新建项目，并添加一个名为 `ClassLibrary1` 的 .NET Core 类库。这将创建项目并将其添加到解决方案中。接下来，通过右键单击 `DefaultAppDomainApp` 项目并选择 添加 ▶ 引用 来添加对该新项目的引用。在左侧栏中选择 项目 ▶ 解决方案，并选中 `ClassLibrary1` 复选框，如图 14-3 所示。

![](img/587021f4d5ba039be70a11107130377b_587_0.png)

图 14-3. 在 Visual Studio 中添加项目引用

在此新的类库中，添加一个 `Car` 类，如下所示：

```
namespace ClassLibrary1
{
    public class Car
    {
        public string PetName { get; set; }
        public string Make { get; set; }
        public int Speed { get; set; }
    }
}
```

有了这个新程序集后，添加以下 using 语句：

```
using System.IO;
using System.Runtime.Loader;
```

接下来你将添加的方法需要 `System.IO` 和 `System.Runtime.Loader` using 语句，而你已在 `Program.cs` 中添加了它们。该方法如下所示：

```
static void LoadAdditionalAssembliesDifferentContexts()
{
    var path =
        Path.Combine(AppDomain.CurrentDomain.BaseDirectory,
            "ClassLibrary1.dll");
    AssemblyLoadContext lc1 =
        new AssemblyLoadContext("NewContext1",false);
    var cl1 = lc1.LoadFromAssemblyPath(path);
    var c1 = cl1.CreateInstance("ClassLibrary1.Car");

    AssemblyLoadContext lc2 =
        new AssemblyLoadContext("NewContext2",false);
    var cl2 = lc2.LoadFromAssemblyPath(path);
    var c2 = cl2.CreateInstance("ClassLibrary1.Car");
    Console.WriteLine("*** Loading Additional Assemblies in Different Contexts ***");
    Console.WriteLine($"Assembly1.Equals(Assembly2) {cl1.Equals(cl2)}");
    Console.WriteLine($"Assembly1 == Assembly2 {cl1 == cl2}");
    Console.WriteLine($"Class1.Equals(Class2) {c1.Equals(c2)}");
    Console.WriteLine($"Class1 == Class2 {c1 == c2}");
}
```

第一行使用静态 `Path.Combine` 方法构建 `ClassLibrary1` 程序集的目录。

> 注意 你可能会想，为什么要为一个将动态加载的程序集创建引用。这是为了确保在项目构建时，`ClassLibrary1` 程序集也会构建，并且与 `DefaultAppDomainApp` 位于同一目录中。这只是本示例的便利做法。动态加载的程序集无需引用。

接下来，代码创建了一个名为 `NewContext1` 的新 `AssemblyLoadContext`（方法的第一个参数）并且不支持卸载（第二个参数）。此 `LoadContext` 用于加载 `ClassLibrary1` 程序集，然后创建 `Car` 类的实例。如果其中一些代码对你来说是新的，它将在第 19 章中更详细地解释。该过程使用新的 `AssemblyLoadContext` 重复，然后比较程序集和类的相等性。当你运行此新方法时，你会看到以下输出：

```
*** Loading Additional Assemblies in Different Contexts ***
Assembly1.Equals(Assembly2) False
Assembly1 == Assembly2 False
Class1.Equals(Class2) False
Class1 == Class2 False
```

这证明同一个程序集已被加载两次到应用程序域中。类也是不同的，正如预期的那样。

接下来，添加一个新方法，该方法将从同一个 `AssemblyLoadContext` 加载程序集。

```
static void LoadAdditionalAssembliesSameContext()
{
    var path =
        Path.Combine(AppDomain.CurrentDomain.BaseDirectory,
            "ClassLibrary1.dll");
    AssemblyLoadContext lc1 =
        new AssemblyLoadContext(null,false);
    var cl1 = lc1.LoadFromAssemblyPath(path);
    var c1 = cl1.CreateInstance("ClassLibrary1.Car");
    var cl2 = lc1.LoadFromAssemblyPath(path);
    var c2 = cl2.CreateInstance("ClassLibrary1.Car");
    Console.WriteLine("*** Loading Additional Assemblies in Same Context ***");
    Console.WriteLine($"Assembly1.Equals(Assembly2) {cl1.Equals(cl2)}");
    Console.WriteLine($"Assembly1 == Assembly2 {cl1 == cl2}");
    Console.WriteLine($"Class1.Equals(Class2) {c1.Equals(c2)}");
    Console.WriteLine($"Class1 == Class2 {c1 == c2}");
}
```

此代码的主要区别在于只创建了一个 `AssemblyLoadContext`。现在，当 `ClassLibrary1` 程序集被加载两次时，第二个程序集只是指向第一个程序集实例的指针。运行代码会产生以下输出：

```
*** Loading Additional Assemblies in Same Context ***
Assembly1.Equals(Assembly2) True
Assembly1 == Assembly2 True
Class1.Equals(Class2) False
Class1 == Class2 False
```

## 总结进程、AppDomain 和加载上下文

到目前为止，你应该对 .NET Core 程序集如何由运行时托管有了更清晰的认识。如果之前的页面对你来说似乎过于底层，请不必担心。在大多数情况下，.NET Core 会自动代表你处理进程、应用程序域和加载上下文的细节。好消息是，这些信息为理解 .NET Core 平台下的多线程编程提供了坚实的基础。

