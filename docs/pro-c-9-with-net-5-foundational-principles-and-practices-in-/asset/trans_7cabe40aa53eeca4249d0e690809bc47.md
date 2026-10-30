

- 内置样式引擎，允许你为 WPF 应用程序定义“主题”。
- 使用矢量图形，允许内容自动调整大小，以适应承载应用程序的屏幕尺寸和分辨率。
- 支持 2D 和 3D 图形、动画以及视频和音频播放。
- 丰富的排版 API，例如支持 XML Paper Specification（XPS）文档、固定文档（所见即所得）、流式文档和文档批注（例如 Sticky Notes API）。
- 支持与旧版 GUI 模型进行互操作（例如 Windows Forms、ActiveX 和 Win32 HWND）。例如，你可以将自定义 Windows Forms 控件嵌入 WPF 应用程序，反之亦然。

现在你已经大致了解了 WPF 所提供的功能，接下来让我们看看可以使用此 API 创建的各种应用程序类型。其中许多功能将在后续章节中详细介绍。

## 探索 WPF 程序集

WPF 归根结底只是 .NET Core 程序集内打包的一组类型。表 24-3 描述了用于构建 WPF 应用程序的核心程序集；创建新项目时必须引用其中每一个程序集。正如你所期望的那样，Visual Studio 的 WPF 项目会自动引用这些必需的程序集。

**表 24-3。WPF 核心程序集**

| 程序集 | 作用 |
|---|---|
| PresentationCore | 此程序集定义了众多命名空间，构成 WPF GUI 层的基础。例如，此程序集包含对 WPF Ink API、动画基元以及众多图形呈现类型的支持。 |
| PresentationFramework | 此程序集包含大多数 WPF 控件、Application 和 Window 类、对交互式 2D 图形的支持，以及数据绑定所使用的众多类型。 |
| System.Xaml.dll | 此程序集提供了一些命名空间，允许你在运行时针对 XAML 文档进行编程。总的来说，只有在编写 WPF 支持工具，或需要在运行时完全控制 XAML 时，这个库才有用。 |
| WindowsBase.dll | 此程序集定义了构成 WPF API 基础设施的类型，包括表示 WPF 线程类型和安全类型的类型、各种类型转换器，以及对依赖属性和路由事件的支持（第 27 章将对此进行介绍）。 |

总的来说，这四个程序集定义了新的命名空间以及 .NET Core 类、接口、结构、枚举和委托。表 24-4 描述了一些重要命名空间的作用（当然并不包括全部命名空间）。

**表 24-4。WPF 核心命名空间**

| 命名空间 | 作用 |
|---|---|
| System.Windows | 这是 WPF 的根命名空间。在这里，你可以找到任何 WPF 桌面项目都需要的核心类（例如 Application 和 Window）。 |
| System.Windows.Controls | 此命名空间包含所有预期的 WPF 小部件，包括用于构建菜单系统、工具提示以及各种布局管理器的类型。 |
| System.Windows.Data | 此命名空间包含用于处理 WPF 数据绑定引擎的类型，以及对数据绑定模板的支持。 |
| System.Windows.Documents | 此命名空间包含用于处理文档 API 的类型。通过 XML Paper Specification（XPS）协议，该 API 允许你将类似 PDF 的功能集成到 WPF 应用程序中。 |
| System.Windows.Ink | 此命名空间为 Ink API 提供支持，允许你捕获来自手写笔或鼠标的输入、响应输入手势等。这对于 Tablet PC 编程很有用；不过，任何 WPF 应用程序都可以使用此 API。 |
| System.Windows.Markup | 此命名空间定义了多个类型，允许以编程方式分析和处理 XAML 标记（以及与之对应的二进制格式 BAML）。 |
| System.Windows.Media | 这是多个以媒体为中心的命名空间的根命名空间。在这些命名空间中，你可以找到用于处理动画、3D 呈现、文本呈现以及其他多媒体基元的类型。 |
| System.Windows.Navigation | 此命名空间提供了用于处理 XAML 浏览器应用程序（XBAP）所采用的导航逻辑，以及需要导航页面模型的标准桌面应用程序的类型。 |
| System.Windows.Shapes | 此命名空间定义了允许你呈现交互式 2D 图形的类，这些图形会自动响应鼠标输入。 |

为了开始学习 WPF 编程模型，你将研究 System.Windows 命名空间中的两个成员，它们在任何传统桌面开发工作中都很常见：Application 和 Window。

> 注意：如果你曾使用 Windows Forms API 创建桌面 UI，请注意，System.Windows.Forms.* 和 System.Drawing.* 程序集与 WPF 无关。这些库代表原始的 .NET GUI 工具包，即 Windows Forms/GDI+。

## Application 类的作用

System.Windows.Application 类表示正在运行的 WPF 应用程序的全局实例。此类提供了一个 Run() 方法（用于启动应用程序），以及一系列可供处理的事件，以便与应用程序的生命周期交互（例如 Startup 和 Exit）。表 24-5 详细介绍了一些关键属性。

**表 24-5。Application 类型的关键属性**

| 属性 | 作用 |
|---|---|
| Current | 此静态属性允许你从代码中的任何位置访问正在运行的 Application 对象。当窗口或对话框需要访问创建它的 Application 对象时，此属性非常有用，通常用于访问应用程序范围的变量和功能。 |
| MainWindow | 此属性允许你以编程方式获取或设置应用程序的主窗口。 |
| Properties | 此属性允许你建立和获取可在 WPF 应用程序各个部分（窗口、对话框等）中访问的数据。 |
| StartupUri | 此属性获取或设置一个 URI，用于指定应用程序启动时自动打开的窗口或页面。 |
| Windows | 此属性返回 WindowCollection 类型的对象，该对象提供对创建 Application 对象的线程所创建的每个窗口的访问。当你想要遍历应用程序中的每个打开窗口并更改其状态（例如最小化所有窗口）时，此属性会很有用。 |

## 构造 Application 类

任何 WPF 应用程序都需要定义一个继承自 Application 的类。在此类中，你将定义程序的入口点（Main() 方法），该方法会创建此子类的实例，并通常根据需要处理 Startup 和 Exit 事件。下面是一个示例：

```
// 定义全局应用程序对象
// 用于此 WPF 程序。
class MyApp : Application
{
    [STAThread]
    static void Main(string[] args)
    {
        // 创建应用程序对象。
        MyApp app = new MyApp();

        // 注册 Startup/Exit 事件。
        app.Startup += (s, e) => { /* 启动应用程序 */ };
        app.Exit += (s, e) => { /* 退出应用程序 */ };
    }
}
```

在 Startup 处理程序中，你通常会处理传入的命令行参数并启动程序的主窗口。正如你所期望的那样，Exit 处理程序用于编写程序所需的关闭逻辑（例如保存用户首选项、写入 Windows 注册表）。

> 注意：WPF 应用程序的 Main() 方法必须使用 [STAThread] 特性进行标记，以确保应用程序使用的任何旧版 COM 对象都是线程安全的。如果不以这种方式标注 Main()，运行时将引发异常。即使 C# 9.0 引入了顶级语句，你仍然会希望在 WPF 应用程序中使用更传统的 Main() 方法。事实上，Main() 方法会自动为你生成。

## 枚举 Windows 集合

Application 公开的另一个有趣属性是 Windows，它提供了一个集合，用于表示当前 WPF 应用程序中加载到内存的每个窗口。当你创建新的 Window 对象时，它们会自动添加到 Application.Windows 集合中。下面是一个示例方法，它会最小化应用程序中的每个窗口（例如响应最终用户触发的某个键盘手势或菜单选项）：

```
static void MinimizeAllWindows()
{
    foreach (Window wnd in Application.Current.Windows)
    {
        wnd.WindowState = WindowState.Minimized;
    }
}
```

你很快就会构建一些 WPF 应用程序，但在此之前，让我们先了解 Window 类型的核心功能，并在此过程中学习一些重要的 WPF 基类。

## Window 类的作用

System.Windows.Window 类（位于 PresentationFramework.dll 程序集中）表示一个由 Application 派生类拥有的单独窗口，其中包括主窗口显示的任何对话框。不出所料，Window 拥有一系列父类，每个父类都会带来更多功能。请看图 24-1，其中展示了 System.Windows.Window 的继承链（以及所实现的接口），该信息来自 Visual Studio 对象浏览器。

![](img/587021f4d5ba039be70a11107130377b_1007_0.png)

图 24-1。Window 类的层次结构

随着你继续学习本章及后续章节，你将逐渐了解许多基类所提供的功能。不过，为了激发你的兴趣，下面几节将分别介绍每个基类所提供的功能（完整细节请参阅 .NET 5 文档）。

