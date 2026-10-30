

## 构建 Multipproject ExtendableApp 解决方案

到目前为止，本书中的大多数应用程序都是独立的项目，只有少数例外（如前一个）。这样做是为了保持示例简单且专注。然而，在实际开发中，你通常会在一个解决方案中一起处理多个项目。

## 使用 CLI 创建解决方案和项目

要开始使用 CLI，输入以下命令来创建一个新的解决方案、类库和控制台应用程序，以及项目引用：

```
dotnet new sln -n Chapter17_ExtendableApp

dotnet new classlib -lang c# -n CommonSnappableTypes -o .\CommonSnappableTypes -f net5.0
dotnet sln .\Chapter17_ExtendableApp.sln add .\CommonSnappableTypes

dotnet new classlib -lang c# -n CSharpSnapIn -o .\CSharpSnapIn -f net5.0
dotnet sln .\Chapter17_ExtendableApp.sln add .\CSharpSnapIn
dotnet add CSharpSnapin reference CommonSnappableTypes

dotnet new classlib -lang vb -n VBSnapIn -o .\VBSnapIn -f net5.0
dotnet sln .\Chapter17_ExtendableApp.sln add .\VBSnapIn
dotnet add VBSnapIn reference CommonSnappableTypes

dotnet new console -lang c# -n MyExtendableApp -o .\MyExtendableApp -f net5.0
dotnet sln .\Chapter17_ExtendableApp.sln add .\MyExtendableApp
dotnet add MyExtendableApp reference CommonSnappableTypes
```

## 向项目文件添加 PostBuild 事件

当项目构建时（无论是从 Visual Studio 还是从命令行），都可以挂钩到事件。例如，我们希望在每次成功构建后，将两个 snap-in 程序集复制到控制台应用程序项目目录（使用 dotnet run 调试时）和控制台应用程序输出目录（使用 Visual Studio 调试时）。为此，我们将利用多个内置宏。

将以下标记块复制到 CSharpSnapIn.csproj 和 VBSnapIn.vbproj 文件中，该块会将编译后的程序集复制到 MyExtendableApp 项目目录以及输出目录（MyExtendableApp\bin\debug\net5.0）：

```
<Target Name="PostBuild" AfterTargets="PostBuildEvent">
    <Exec Command="copy $(TargetPath) $(SolutionDir)MyExtendableApp\$(OutDir)$(TargetFileName) /Y &copy $(TargetPath) $(SolutionDir)MyExtendableApp\$(TargetFileName) /Y" />
</Target>
```

现在，每次构建项目时，其程序集也会被复制到 MyExtendableApp 的目标目录中。

## 使用 Visual Studio 创建解决方案和项目

请记住，默认情况下，Visual Studio 会将解决方案命名为在该解决方案中创建的第一个项目。然而，你可以轻松更改解决方案的名称，如图 17-3 所示。

## 配置你的新项目

Class Library (.NET Core) C# Windows Linux macOS Library

Project name
CommonSnappableTypes

Location
C:\GitHub\Books\csharp8-wf\Code\Chapter_17

Solution
Create new solution

Solution name
ExtendableApp

Place solution and project in the same directory

Back Create

# Figure 17-3. Creating the CommonSnappableTypes project and the ExtendableApp solution

要创建 ExtendableApp 解决方案，首先选择 File ► New Project 以加载 New Project 对话框。选择 Class Library 并输入名称 CommonSnappableTypes。在点击 OK 之前，输入解决方案名称 ExtendableApp，如图 17-3 所示。

要向解决方案中添加另一个项目，在 Solution Explorer 中右键单击解决方案名称（ExtendableApp）（或点击 File ► Add ► New Project）并选择 Add ► New Project。向现有解决方案添加另一个项目时，Add New Project 对话框现在略有不同；解决方案选项不再存在，因此你将只看到项目信息（名称和位置）。将 Class Library 项目命名为 CSharpSnapIn 并点击 Create。

接下来，从 CSharpSnapIn 项目添加对 CommonSnappableTypes 项目的引用。为此在 Visual Studio 中，右键单击 CSharpSnapIn 项目并选择 Add ► Project Reference。在 Reference Manager 对话框中，从左侧选择 Projects ► Solution（如果尚未选择），并选中 CommonSnappableTypes 旁边的复选框。

对新的 Visual Basic 类库（VBSnapIn）重复此过程，该库引用 CommonSnappableTypes 项目。

要添加的最后一个项目是名为 MyExtendableApp 的 .NET Core 控制台应用程序。添加对 CommonSnappableTypes 项目的引用并将控制台应用程序设置为解决方案的启动项目。为此，在 Solution Explorer 中右键单击 MyExtendableApp 项目并选择 Set as StartUp Project。

> Note 如果你右键单击 ExtendableApp 解决方案而不是其中一个项目，上下文菜单中显示的选项是 Set StartUp Projects。除了点击 Run 时只执行一个项目外，你还可以设置多个项目同时执行。这将在后面的章节中演示。

## 设置项目构建依赖项

当 Visual Studio 被指示运行解决方案时，启动项目和所有引用的项目如果检测到更改将被构建；然而，任何未引用的项目都不会被构建。这可以通过设置项目依赖项来更改。为此，在 Solution Explorer 中右键单击解决方案，选择 Project Build Order，并在结果对话框中选择 Dependencies 选项卡并更改项目为 MyExtendableApp。

请注意 CommonSnappableTypes 项目已被选中并且复选框被禁用。这是因为它被直接引用。同样选中 CSharpSnapIn 和 VBSnapIn 项目的复选框，如图 17-4 所示。

![](img/587021f4d5ba039be70a11107130377b_704_0.png)

Figure 17-4. Accessing the Project Build Order context menu

现在，每次构建 MyExtendableApp 项目时，CSharpSnapIn 和 VBSnapIn 项目也会构建。

## 添加 PostBuild 事件

打开 CSharpSnapIn 的项目属性（右键单击 Solution Explorer 并选择 Properties）并导航到 Build Events 页面（C#）。点击 Edit Post-build 按钮然后点击 Macros>>。在这里你可以查看可用的宏，它们都引用路径和/或文件名。使用这些宏在构建事件中的优势在于它们是机器无关的并且在相对路径上工作。例如，我正在 c-sharp-wf\code\chapter17 目录下工作。你可能（并且很可能）使用不同的目录。通过使用宏，MSBuild 将始终使用相对于 *.csproj 文件的正确路径。

在 PostBuild 框中输入以下内容（分两行）：

```
copy $(TargetPath) $(SolutionDir)MyExtendableApp\$(OutDir)$(TargetFileName) /Y
copy $(TargetPath) $(SolutionDir)MyExtendableApp\$(TargetFileName) /Y
```

对 VBSnapIn 项目执行相同的操作，只是属性页称为 Compile，从那里你可以点击 Build Events 按钮。

在添加这些 post-build 事件命令后，每次编译时，每个程序集都会被复制到 MyExtendableApp 的项目和输出目录中。

## 构建 CommonSnappableTypes.dll

在 CommonSnappableTypes 项目中，删除默认的 Class1.cs 文件，添加一个名为 IAppFunctionality.cs 的新接口文件，并将其更新为以下内容：

```
namespace CommonSnappableTypes
{
    public interface IAppFunctionality
    {
        void DoIt();
    }
}
```

添加一个名为 CompanyInfoAttribute.cs 的类文件并将其更新为以下内容：

```
using System;
namespace CommonSnappableTypes
{
    [AttributeUsage(AttributeTargets.Class)]
    public sealed class CompanyInfoAttribute : System.Attribute
    {
        public string CompanyName { get; set; }
        public string CompanyUrl { get; set; }
    }
}
```

IAppFunctionality 接口为所有可以被可扩展应用程序消费的 snap-in 提供了多态接口。鉴于此示例纯粹是说明性的，你提供了一个名为 DoIt() 的单个方法。

CompanyInfoAttribute 类型是一个自定义属性，可以应用于任何想要被容器 snap-in 的类类型。正如该类的定义所示，[CompanyInfo] 允许 snap-in 的开发人员提供有关组件来源的一些基本详细信息。

## 构建 C# Snap-In

在 CSharpSnapIn 项目中，删除 Class1.cs 文件并添加一个名为 CSharpModule.cs 的新文件。将代码更新为以下内容：

```
using System;
using CommonSnappableTypes;

namespace CSharpSnapIn
{
    [CompanyInfo(CompanyName = "FooBar", CompanyUrl = "www.FooBar.com")]
    public class CSharpModule : IAppFunctionality
```

