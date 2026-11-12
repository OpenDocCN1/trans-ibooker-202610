
## 联系我们

读者的反馈始终受欢迎。

**一般反馈**：如果您对本书的任何方面有疑问，请在邮件主题中注明书名，并发送邮件至 customercare@packtpub.com。

**勘误**：虽然我们已竭力确保内容的准确性，但错误难免。如果您在本书中发现了错误，我们将不胜感激如果您能向我们报告。请访问 www.packt.com/submit-errata，选择您的书，单击“勘误提交表单”链接，并填写详细信息。

**盗版**：如果您在互联网上以任何形式发现我们作品的非法副本，我们将不胜感激如果您能向我们提供位置地址或网站名称。请通过 copyright@packt.com 与我们联系，并附上材料链接。

**如果您有兴趣成为作者**：如果您有擅长的主题，并且有兴趣撰写或参与编写一本书，请访问 authors.packtpub.com。

## 评论

请留下评论。阅读并使用本书后，为什么不在购买该书的网站上留下评论呢？这样，潜在读者就可以看到并使用您客观公正的意见来做出购买决定，我们在 Packt 可以了解您对我们产品的看法，我们的作者可以看到您对其书籍的反馈。谢谢！

有关 Packt 的更多信息，请访问 packt.com。

## 1 电子书管理器和目录应用 - 用于桌面的 .NET Core

.NET Core 3 标志着 .NET 重启的一个重要版本。既然基础框架已经到位，Microsoft 现在能够审视那些虽不再流行，但在全世界数百万台机器上运行的技术。

WinForms 和 WPF 成为其自身成功的牺牲品：Microsoft 根本不敢改变围绕它们的框架，以免冒风险破坏那些可能已经成功运行多年的应用程序。

C# 8 有着类似的主题，即它引入了可为 null 的引用类型以及旨在改进旧代码库的接口实现等功能。

> 旧代码库是指任何已经编写的代码，无论那是 10 年前还是 10 分钟前！

在本章，即第一章中，我们将创建电子书管理器应用程序。随后，我们将使用 .NET Core 2 构建的电子书管理器迁移到 .NET Core 3。

在 .NET Core 2 中，进行了多项重大性能增强，因此升级现有 WinForms 应用程序到 .NET Core 3 是实实在在的驱动力。Microsoft 曾夸耀 .NET Core 2.1 为 Bing 带来了超过 30% 的性能提升。

我们将涵盖以下主题：

- 在 .NET Core 3.0 中创建新的 WinForms 应用程序
- 将现有的 WinForms 应用程序迁移到 .NET Core 3.0
- 可为 null 的引用类型
- XAML 岛屿，以及如何使用它们为现有 WinForms 应用程序添加功能
- Tree shaking 和编译

### 技术要求

要跟随本章的第一部分，您需要 WinForms 设计器。撰写本文时，该设计器处于预发布阶段，可以从 https://aka.ms/winforms-designer 下载。

对于 XAML 岛屿部分，您需要运行 Windows 10 1903 或更高版本。预计到本书出版时，1903 版本将自动推送到所有 Windows 10 机器上；但是，如果您运行的是较早版本，则可以通过访问以下链接强制更新：https://www.microsoft.com/en-us/software-download/windows10。

> 撰写本文时，此 WinForms 设计器距离可用于生产环境还很遥远。跟随本章时可以试用；但是，如果您发现它太不稳定，请随意从 GitHub 项目复制设计器代码。

### 创建新的 WinForms 应用程序

让我们先创建一个新的 .NET Core 3.0 WinForms 应用程序，随后我们还将看到如何将旧的 .NET Core WinForms 应用升级到 3.0，以便展示两种实现方式。

要跟随本节，您需要安装 *技术要求* 部分中描述的 WinForms 设计器。值得指出的是，撰写本文时该工具处于预览阶段，因此存在多种限制，所以说明已更改以适应这些限制。

使用 Visual Studio 2019，我们将创建一个简单 **Windows 窗体应用** 模板项目。您可以随意命名应用程序，但我将我的命名为 `eBookManager`：

![](img/762492a8287f369cc1ac4621a212f193_22_0.png)

在 Visual Studio 2019 中，创建新项目的流程略有变化，您需要先选择应用程序类型，然后选择创建位置：

![](img/762492a8287f369cc1ac4621a212f193_23_0.png)

### 创建项目后，效果如下：

![](img/762492a8287f369cc1ac4621a212f193_24_0.png)

由于这是 .NET Core，您可以全部从命令行完成。在 PowerShell 中，运行以下命令将创建相同的项目：dotnet new winforms -n eBookManager。

我们的解决方案需要一个类库项目来包含驱动 eBookManager 应用程序的类。在 .NET Core 3.0 中，我们可以选择创建 .NET Core 类库或 .NET Standard 类库。

.NET Standard 是一个有点奇怪的概念。它本身不是一种技术，而是一份协议；创建 .NET Standard 类库仅仅是禁止使用任何 .NET Core（或框架）特定的内容，且不符合 .NET Standard。以下文档说明了 .NET Standard 版本，以及 Core 和 Framework 的哪些版本支持它们：https://github.com/dotnet/standard/blob/master/docs/versions.md。

向您的解决方案添加一个新 **类库 (.NET Standard)** 项目，并将其命名为 *eBookManager.Engine*：

![](img/762492a8287f369cc1ac4621a212f193_25_0.png)

一个类库项目已添加到解决方案中，带有默认类名。将此类更改为 *Document*：

![](img/762492a8287f369cc1ac4621a212f193_26_0.png)

`Document` 类将表示一本电子书。在考虑一本书时，我们可以有多个属性来表示一本书，但这将代表所有书籍。这的一个例子就是作者。所有书籍都必须有作者；否则，它们就不会存在。

我添加到类中的属性仅仅是我对可能代表一本书的内容的诠释。请随意添加其他代码，使其成为您自己的。打开 `Document.cs` 文件，并将以下代码添加到类中：

```
public class Document
{
    public string Title { get; set; }
    public string FileName { get; set; }
    public string Extension { get; set; }
    public DateTime LastAccessed { get; set; }
    public DateTime Created { get; set; }
    public string FilePath { get; set; }
    public string FileSize { get; set; }
    public string ISBN { get; set; }
    public string Price { get; set; }
    public string Publisher { get; set; }
    public string Author { get; set; }
    public DateTime PublishDate { get; set; }
    public DeweyDecimal Classification { get; set; }
    public string Category { get; set; }
}
```

您会发现，我包含了一个名为 Classification 的属性，类型为 DeweyDecimal。我们尚未添加此类，稍后将添加。向 eBookManager.Engine 项目添加一个名为 DeweyDecimal 的类。如果您不想为电子书进行到这种分类级别，可以省略此类。我包含它是为了完整性。我们将介绍一个 Visual Studio 中已存在一段时间的实用小功能：如果您将鼠标悬停在 DeweyDecimal 文本上，将看到一个灯泡出现（您可以通过按住 Ctrl 键和点号键（Ctrl + .）手动调出此菜单。我在本书余下部分将大量使用此快捷键！）

![](img/762492a8287f369cc1ac4621a212f193_27_0.png)
