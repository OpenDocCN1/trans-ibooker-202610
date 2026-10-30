

```
<AssemblyVersion>1.0.0.1</AssemblyVersion>
<FileVersion>1.0.0.2</FileVersion>
<Version>1.0.0.3</Version>
</PropertyGroup>
```

> **注意** 图16-4中其余的条目（以及项目文件列表）在从程序集生成NuGet包时使用。这将在本章后面介绍。

## 探索CIL

回顾一下，程序集并不包含平台特定的指令；相反，它包含与平台无关的公共中间语言（CIL）指令。当.NET Core运行时将程序集加载到内存中时，底层的CIL会被编译（使用JIT编译器）成目标平台可以理解的指令。例如，`SportsCar`类的`TurboBoost()`方法由以下CIL表示：

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

与本书中的其他CIL示例一样，大多数.NET Core开发人员不需要深入了解细节。第19章将更详细地介绍其语法和语义，这在构建需要高级服务（如程序集的运行时构造）的更复杂应用程序时可能会有所帮助。

## 探索类型元数据

在构建使用自定义.NET库的应用程序之前，先检视一下`CarLibrary.dll`程序集中类型的元数据。例如，以下是`EngineStateEnum`的TypeDef：

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

正如下一章所解释的，程序集的元数据是.NET Core平台的一个重要元素，并且是众多技术（对象序列化、后期绑定、可扩展应用程序等）的支柱。无论如何，现在你已经查看了`CarLibrary.dll`程序集的内部，可以构建一些使用你的类型的客户端应用程序了。

## 构建C#客户端应用程序

由于`CarLibrary`的每个类型都是用`public`关键字声明的，其他.NET Core应用程序也可以使用它们。回顾一下，你也可以使用C#的`internal`关键字定义类型（实际上，这是C#类的默认访问模式）。内部类型只能由定义它们的程序集使用。外部客户端既不能看到也不能创建标记为`internal`关键字的类型。

> **注意** 此规则的例外情况是，程序集使用`InternalsVisibleTo`特性显式允许另一个程序集访问，这一点稍后会介绍。

为了使用库的功能，在与`CarLibrary`相同的解决方案中创建一个名为`CSharpCarClient`的新C#控制台应用程序项目。你可以使用Visual Studio（右键单击解决方案并选择“添加 ▶ 新建项目”）或使用命令行（三行，每行单独执行）来完成此操作。

```
dotnet new console -lang c# -n CSharpCarClient -o .\CSharpCarClient -f net5.0
dotnet add CSharpCarClient reference CarLibrary
dotnet sln .\Chapter16_AllProjects.sln add .\CSharpCarClient
```

上述命令创建了控制台应用程序，为新项目添加了对CarLibrary项目的项目引用，并将其添加到解决方案中。

> **注意** `add reference`命令创建的是项目引用。这对于开发很方便，因为`CSharpCarClient`将始终使用最新版本的`CarLibrary`。你也可以直接引用程序集。直接引用是通过引用编译后的类库来创建的。

如果你仍然在Visual Studio中打开解决方案，你会注意到新项目无需任何干预即可出现在解决方案资源管理器中。

最后一项更改是右键单击解决方案资源管理器中的`CSharpCarClient`，然后选择“设为启动项目”。如果你不使用Visual Studio，可以在项目目录中执行`dotnet run`来运行新项目。

> **注意** 你也可以在Visual Studio中设置项目引用，方法是右键单击解决方案资源管理器中的`CSharpCarClient`项目，选择“添加 ▶ 引用”，然后从项目的节点中选择`CarLibrary`项目。

此时，你可以构建客户端应用程序来使用外部类型。将你的初始C#文件更新如下：

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

这段代码看起来与本书迄今开发的其他应用程序代码完全相同。唯一值得注意的是，该C#客户端应用程序现在使用了在单独的自定义库中定义的类型。运行程序并验证你是否看到消息显示。

你可能想知道引用`CarLibrary`项目时究竟发生了什么。当建立项目引用时，解决方案的构建顺序会被调整，以便依赖项目（此例中为`CarLibrary`）首先构建，然后该构建的输出会复制到父项目（`CSharpCarClient`）的输出目录中。编译后的客户端库引用编译后的类库。当客户端项目重新构建时，依赖库也会重新构建，新版本会再次复制到目标文件夹。

> **注意** 如果你使用的是Visual Studio，可以单击解决方案资源管理器中的“显示所有文件”按钮，查看所有输出文件并验证编译后的`CarLibrary`是否在那里。如果你使用的是Visual Studio Code，请在资源管理器选项卡中导航到`bin/debug/net5.0`目录。

当建立直接引用时，编译后的库也会复制到客户端库的输出目录中，但这是在建立引用时进行的。如果没有项目引用，项目可以彼此独立构建，文件可能会变得不同步。简而言之，如果你在开发依赖库（在真实软件项目中通常如此），最好是引用项目而不是项目输出。

## 构建Visual Basic客户端应用程序

回顾一下，.NET Core平台允许开发人员跨编程语言共享编译代码。为了说明.NET Core平台的语言无关性，让我们创建另一个控制台应用程序项目（`VisualBasicCarClient`），这次使用Visual Basic（请注意，每条命令都是一行命令）。

```
dotnet new console -lang vb -n VisualBasicCarClient -o .\VisualBasicCarClient -f net5.0
dotnet add VisualBasicCarClient reference CarLibrary
dotnet sln .\Chapter16_AllProjects.sln add VisualBasicCarClient
```

与C#一样，Visual Basic允许你列出当前文件中使用的每个命名空间。但是，Visual Basic提供`Imports`关键字而不是C#的`using`关键字，因此在`Program.vb`代码文件中添加以下`Imports`语句：

```
Imports CarLibrary
Module Program
    Sub Main()
    End Sub
End Module
```

请注意，`Main()`方法是在Visual Basic模块类型中定义的。简而言之，模块是Visual Basic中定义只能包含静态方法的类的一种表示法（很像C#的静态类）。无论如何，为了使用Visual Basic语法来使用`MiniVan`和`SportsCar`类型，请将你的`Main()`方法更新如下：

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

当你编译并运行应用程序时（确保在Visual Studio中将`VisualBasicCarClient`设置为启动项目），你将再次看到一系列显示的消息。此外，这个新的客户端应用程序在`bin\Debug\net5.0`文件夹下有自己本地副本的`CarLibrary.dll`。

## 跨语言继承的实际应用

.NET Core开发的一个诱人方面是跨语言继承的概念。为了说明这一点，让我们创建一个新的Visual Basic类，它派生自`SportsCar`（该类是用C#编写的）。首先，在你的当前Visual Basic应用程序中添加一个名为`PerformanceCar.vb`的新类文件。通过使用`Inherits`关键字从`SportsCar`类型派生，更新初始类定义。然后，使用`Overrides`关键字重写抽象的`TurboBoost()`方法，如下所示：

```
Imports CarLibrary
' This VB class is deriving from the C# SportsCar.
Public Class PerformanceCar
```

