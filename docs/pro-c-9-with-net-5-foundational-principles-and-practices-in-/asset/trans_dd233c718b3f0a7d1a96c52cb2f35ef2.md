
此时，MyTypeViewer.exe 已准备好进行试驾。例如，运行应用程序并输入以下完全限定名称（请注意，Type.GetType() 要求区分大小写的字符串名称）：

- System.Int32
- System.Collections.ArrayList
- System.Threading.Thread
- System.Void
- System.IO.BinaryWriter
- System.Math
- MyTypeViewer.Program

例如，以下是指定 System.Math 时的一些部分输出：

```
**** Welcome to MyTypeViewer ****
Enter a type name to evaluate
or enter Q to quit: System.Math

**** Various Statistics ****
Base class is: System.Object
Is type abstract? True
Is type sealed? True
Is type generic? False
Is type a class type? True

**** Fields ****
->PI
->E

**** Properties ****

**** Methods ****
->Acos
->Asin
->Atan
->Atan2
->Ceiling
->Cos
...
```

## 反射静态类型

如果你在之前的方法中输入 System.Console，第一个辅助方法中将抛出异常，因为 t 的值将为 null。静态类型无法使用 Type.GetType(typeName) 方法加载。相反，你必须使用另一种机制，即来自 System.Type 的 typeof 函数。如下更新程序以处理 System.Console 这种特殊情况：

```
Type t = Type.GetType(typeName);
if (t == null && typeName.Equals("System.Console",
StringComparison.OrdinalIgnoreCase))
{
    t = typeof(System.Console);
}
```

## 反射泛型类型

当你调用 Type.GetType() 来获取泛型类型的元数据描述时，必须使用一种特殊的语法，包含一个“反引号”字符（`），后跟一个数值，该数值表示类型所支持的类型参数个数。例如，如果你想打印 System.Collections.Generic.List<T> 的元数据描述，你需要向应用程序传入以下字符串：

```
System.Collections.Generic.List`1
```

这里，你使用数值 1，因为 List<T> 只有一个类型参数。然而，如果你想反射 Dictionary<TKey, TValue>，则提供数值 2，如下所示：

```
System.Collections.Generic.Dictionary`2
```

## 反射方法参数和返回值

到目前为止，一切顺利！接下来，我们将对当前应用程序进行一个小改进。具体来说，你将更新 ListMethods() 辅助函数，使其不仅列出给定方法的名称，还列出返回类型和传入参数类型。MethodInfo 类型为此提供了 ReturnType 属性和 GetParameters() 方法。在以下修改后的代码中，请注意你正在使用嵌套的 foreach 循环（不使用 LINQ）构建一个包含每个参数类型和名称的字符串：

```
static void ListMethods(Type t)
{
    Console.WriteLine("***** Methods *****");
    MethodInfo[] mi = t.GetMethods();
    foreach (MethodInfo m in mi)
    {
        // Get return type.
        string retVal = m.ReturnType.FullName;
        string paramInfo = "( ";
        // Get params.
        foreach (ParameterInfo pi in m.GetParameters())
        {
            paramInfo += string.Format("{0} {1} ", pi.ParameterType, pi.Name);
        }
        paramInfo += " )";

        // Now display the basic method sig.
        Console.WriteLine("->{0} {1} {2}", retVal, m.Name, paramInfo);
    }
    Console.WriteLine();
}
```

如果你现在运行这个更新后的应用程序，你会发现给定类型的方法更加详细了。如果向程序输入你的老朋友 System.Object，将显示以下方法：

```
**** Methods ****
->System.Type GetType ( )
->System.String ToString ( )
->System.Boolean Equals ( System.Object obj )
->System.Boolean Equals ( System.Object objA System.Object objB )
->System.Boolean ReferenceEquals ( System.Object objA System.Object objB )
->System.Int32 GetHashCode ( )
```

当前 ListMethods() 的实现很有帮助，因为你可以直接使用 System.Reflection 对象模型检查每个参数和方法返回类型。作为一个极端的捷径，请注意所有 XXXInfo 类型（MethodInfo、PropertyInfo、EventInfo 等）都重写了 ToString() 来显示所请求项的签名。因此，你也可以像下面这样实现 ListMethods()（再次使用 LINQ，你只需选择所有 MethodInfo 对象，而不是仅选择 Name 值）：

```
static void ListMethods(Type t)
{
    Console.WriteLine("**** Methods ****");
    var methodNames = from n in t.GetMethods() select n;
    foreach (var name in methodNames)
    {
        Console.WriteLine("->{0}", name);
    }
    Console.WriteLine();
}
```

有趣吧？显然，System.Reflection 命名空间和 System.Type 类允许你反射类型中许多其他方面的信息，而不仅仅是 MyTypeViewer 当前显示的内容。正如你所期望的那样，你可以获取类型的事件、获取给定成员的任何泛型参数列表，并收集许多其他细节。

然而，此时你已经创建了一个（具有一定能力的）对象浏览器。这个特定示例的主要限制是，你无法反射当前程序集（MyTypeViewer）或基类库中始终被引用的程序集（如 mscorlib.dll）之外的内容。这就引出了一个问题：“我如何构建能够加载（并反射）编译时未引用的程序集的应用程序？”很高兴你问到这。

## 动态加载程序集

有时你需要以编程方式即时加载程序集，即使清单中没有该程序集的记录。正式地说，按需加载外部程序集的行为称为动态加载。

System.Reflection 定义了一个名为 Assembly 的类。使用这个类，你可以动态加载程序集，并发现程序集本身的属性。使用 Assembly 类型，你可以动态加载程序集，也可以加载位于任意位置的程序集。本质上，Assembly 类提供了允许你以编程方式从磁盘加载程序集的方法。

为了演示动态加载，创建一个名为 ExternalAssemblyReflector 的新控制台应用程序项目。你的任务是编写代码，提示输入要动态加载的程序集名称（不带任何扩展名）。你将把 Assembly 引用传递给一个名为 DisplayTypes() 的辅助方法，该方法将简单地打印它所包含的每个类、接口、结构、枚举和委托的名称。代码出奇地简单。

```
using System;
using System.Reflection;
using System.IO; // For FileNotFoundException definition.

Console.WriteLine("***** External Assembly Viewer *****");
string asmName = "";
Assembly asm = null;
do
{
Console.WriteLine("\nEnter an assembly to evaluate");
Console.Write("or enter Q to quit: ");
// Get name of assembly.
asmName = Console.ReadLine();
// Does user want to quit?
if (asmName.Equals("Q",StringComparison.OrdinalIgnoreCase))
{
    break;
}

// Try to load assembly.
try
{
    asm = Assembly.LoadFrom(asmName);
    DisplayTypesInAsm(asm);
}
catch
{
    Console.WriteLine("Sorry, can't find assembly.");
}
} while (true);

static void DisplayTypesInAsm(Assembly asm)
{
    Console.WriteLine("\n***** Types in Assembly *****");
    Console.WriteLine("{0}", asm.FullName);
    Type[] types = asm.GetTypes();
    foreach (Type t in types)
    {
        Console.WriteLine("Type: {0}", t);
    }
    Console.WriteLine("");
}
```

如果你想反射 CarLibrary.dll，你需要将 CarLibrary.dll 二进制文件（来自上一章）复制到 ExternalAssemblyReflector 应用程序的项目目录（如果使用 Visual Studio Code）或 \bin\Debug\net5.0 目录（如果使用 Visual Studio）中，才能运行此程序。在提示时输入 **CarLibrary**（扩展名可选），输出将如下所示：

```
**** External Assembly Viewer ****
Enter an assembly to evaluate
or enter Q to quit: CarLibrary

**** Types in Assembly ****
->CarLibrary, Version=1.0.0.1, Culture=neutral, PublicKeyToken=null
Type: CarLibrary.MyInternalClass
Type: CarLibrary.EngineStateEnum
Type: CarLibrary.MusicMedia
Type: CarLibrary.Car
Type: CarLibrary.MiniVan
Type: CarLibrary.SportsCar
```

LoadFrom 方法还可以接受你要查看的程序集的绝对路径（例如，C:\MyApp\MyAsm.dll）。使用此方法，你可以传入控制台应用程序项目的完整路径。因此，如果 CarLibrary.dll 位于 C:\MyCode 下，你可以输入 C:\MyCode\CarLibrary（请注意扩展名是可选的）。

## 反射框架程序集

Assembly.Load() 方法有多个重载。其中一个变体允许你指定区域性值（用于本地化程序集），以及版本号和公钥标记值（用于框架程序集）。总的来说，标识程序集的一组项被称为*显示名称*。显示名称的格式是一个以逗号分隔的名称-值对字符串，以程序集的友好名称开头，后跟可选限定符（可以按任意顺序出现）。以下是需要遵循的模板（可选项目出现在括号中）：

```
Name (,Version = major.minor.build.revision) (,Culture = culture token) (,PublicKeyToken= public key token)
```

在构造显示名称时，约定 PublicKeyToken=null 表示需要绑定和匹配非强命名程序集。此外，Culture="" 表示匹配目标计算机的默认区域性。以下是一个示例：

```
// Load version 1.0.0.0 of CarLibrary using the default culture.
Assembly a =
    Assembly.Load("CarLibrary, Version=1.0.0.0, PublicKeyToken=null, Culture=\"\"");
// The quotes must be escaped with back slashes in C#
```

另外请注意，System.Reflection 命名空间提供了 AssemblyName 类型，它允许你在一个方便的对象变量中表示上述字符串信息。通常，此类与 System.Version 一起使用，System.Version 是程序集版本号的面向对象包装器。一旦你建立了显示名称，就可以将其传递给重载的 Assembly.Load() 方法，如下所示：

```
// Make use of AssemblyName to define the display name.
AssemblyName asmName;
asmName = new AssemblyName();
```

