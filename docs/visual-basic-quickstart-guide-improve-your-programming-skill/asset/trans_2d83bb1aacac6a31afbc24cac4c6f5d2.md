
网站名称。请通过copyright@packt.com联系我们并提供材料链接。

**如果您有兴趣成为作者：** 如果您有擅长的主题，并且有兴趣撰写或参与编写一本书，请访问authors.packtpub.com。

## 分享您的想法

阅读完《*Visual Basic 快速入门指南*》后，我们很乐意听到您的想法！请[点击此处直接前往本书的亚马逊评论页面](https://www.amazon.com)并分享您的反馈。

您的评论对我们和技术社区都很重要，将帮助我们确保提供优质的内容。

## 下载本书的免费PDF副本

感谢您购买本书！

您是否喜欢随时随地阅读，但又无法随身携带纸质书？

您的电子书购买是否与您选择的设备不兼容？

别担心，现在每本Packt图书都附带一个无DRM的PDF版本，完全免费。

可在任何地点、任何设备上阅读。从您最喜爱的技术书籍中直接搜索、复制和粘贴代码到您的应用程序中。

福利还不止这些，您还可以每天在收件箱中独家获得折扣、新闻通讯和精彩免费内容。

按照以下简单步骤即可享受这些福利：

- 1. 扫描二维码或访问下面的链接

![](img/421317ea489d4f3f077fe0c3dcdfc6fe_26_0.png)

https://packt.link/free-ebook/9781805125310

- 2. 提交您的购买凭证
- 3. 就这样！我们会将免费PDF和其他福利直接发送到您的邮箱

## 第1部分：Visual Basic 编程与脚本

在本书的第一部分，我们将重点教授您编写Visual Basic家族任何成员核心代码所需的基本编程技能。如果您学过任何其他编程语言，这些主题都会很熟悉，因为大多数编程语言都有类似的主题。

本部分包含以下章节：

- [第1章](#)，Visual Basic 编程语言家族
- [第2章](#)，控制台输入与输出
- [第3章](#)，数据类型与变量
- [第4章](#)，决策分支
- [第5章](#)，迭代
- [第6章](#)，函数与过程
- [第7章](#)，项目第一部分

Windows Forms 提供了一组预构建的控件，如按钮、文本框、标签和菜单，可以轻松放置在窗体上以创建用户界面。这些控件可以通过更改颜色、大小和字体等属性进行自定义，并且可以编程以响应用户输入和事件。

Visual Basic.NET 是一种可用于为 Windows Forms 应用程序编写代码的编程语言。它是一种易于学习和使用的高级语言，并提供了开发复杂应用程序的广泛功能。

Visual Basic.NET Windows Forms 的一些功能包括以下内容：

- **快速应用程序开发**：Windows Forms 提供了一种使用可视化设计器创建桌面应用程序的快捷简便方法。
- **数据绑定**：Windows Forms 支持数据绑定，允许开发人员将控件连接到数据源（如数据库或 XML 文件）。
- **高级控件**：Windows Forms 包含一组高级控件，如 `DataGridView` 和 Chart 控件，可用于创建更复杂的用户界面。
- **面向对象编程（OOP）**：Visual Basic.NET 支持 OOP，允许开发人员编写可重用且可维护的代码。

### 面向对象编程（OOP）

*OOP 是一种基于对象概念的编程范式，对象是包含数据和行为的实体。在 OOP 中，对象由类创建，类定义了对象将拥有的数据和行为。OOP 的主要原则包括：*

- **封装**：*该原则涉及将数据和行为捆绑到对象中，并向外界隐藏实现细节。封装有助于确保对象被正确使用，并确保其包含的数据不会以意外方式被修改。*
- **继承**：*该原则涉及通过扩展现有类来创建新类。继承允许新类继承其父类的数据和行为，并允许它们添加自己独特的特性。*
- **多态**：*该原则允许将不同类的对象视为相同的对象类型。多态通过接口或抽象类实现，这些接口或抽象类定义了一组标准方法，所有实现该接口或继承该抽象类的对象都可以使用这些方法。*

*OOP 有许多优点，包括改进代码组织、提高代码复用性和更好的可维护性。*

总体而言，Visual Basic.NET Windows Forms 是在 Windows 平台上创建桌面应用程序的强大工具，由于其易用性和广泛的功能，它成为开发人员的热门选择。

不幸的是，微软已宣布未来不会继续发展 Visual Basic.NET 语言。这一公告并不意味着该语言已死——相反，现有数十亿行生产代码将在未来几十年内继续运行。这只是意味着不会向该语言添加新的语言特性。既然我们已经熟悉了 Visual Basic.NET Windows Forms，让我们来看一个简单的编码示例。

### Visual Basic.NET Windows Forms 中的 Hello, World!

Visual Basic.NET Windows Forms 代码存储在扩展名为 .vb 的文本文件中。以下是示例 Visual Basic.NET Windows Form 代码，将存储在名为 program.vb 之类的文件中。运行该程序将在启动时显示一个消息框，显示 Hello World!：

```
Public Class Form1
    Private Sub Form1_Load(sender As Object, e As EventArgs) Handles MyBase.Load
        MsgBox("Hello, World!")
    End Sub
End Class
```

接下来，我们将了解如何实现此代码。

### 安装 Visual Basic.NET

以下步骤将在 Windows 机器上安装 Visual Basic.NET：

- 1. 从此链接下载 Visual Studio Community Edition：https://learn.microsoft.com/en-us/visualstudio/install/install-visual-studio?view=vs-2022。
- 2. 双击 VisualStudioSetup.exe 引导程序以开始安装。
- 3. 选择“.NET 桌面开发”工作负载。
- 4. 让安装程序完成。

大多数用户将使用 Microsoft Windows 操作系统（OS）和 Microsoft 实现的 Visual Basic.NET，但也有一些项目和产品允许在不同操作系统上安装，我们将在接下来讨论。

### Visual Basic.NET 的替代安装

以下列表显示了使用 Visual Basic.NET 兼容代码的其他选项：

- Mono：Mono 是一个基于 .NET 框架的开源开发平台。它包含一个 Visual Basic.NET 编译器和 VB.NET 运行时。您可以从此处下载 Mono：http://mono-project.com。
- .NET SDK：.NET 软件开发工具包（SDK）是一组用于开发面向 .NET 平台的应用程序的工具、库和组件。它包含构建和部署 .NET 应用程序所需的一切，包括编译器、运行时和各种库。

图 1.1 – 运行您的程序

随着我们继续学习，我们将在不同的 Visual Basic 家族成员中使用相同的应用程序示例。

### 使用 Visual Basic Classic 进行编程

**Visual Basic Classic**（也称为 **VB6**）是一种编程语言和**集成开发环境（IDE）**，由 Microsoft 开发。它于 1998 年发布，广泛用于开发 Windows 操作系统的桌面应用程序。

VB6 是 .NET Framework 引入之前 Visual Basic 编程语言的最后一个版本，其语法与早期版本的 Visual Basic 类似。该语言旨在易于学习和使用，在初学者和经验丰富的开发人员中都很受欢迎。

VB6 应用程序可以使用 Visual Basic IDE 开发，该 IDE 包含可视化窗体设计器、代码编辑器和调试器。该语言支持 OOP 概念，如封装、继承和多态。

尽管 Microsoft 已停止支持 VB6，但许多遗留应用程序仍在使用该语言，并且它仍然是小型 Windows 桌面应用程序开发的热门选择。

### Visual Basic Classic 中的 Hello, World!

Visual Basic Classic 代码存储在扩展名为 `.bas` 的文本文件中：

```
Private Sub Form_Load()
    MsgBox "Hello, World!"
End Sub
```

您接下来将实现此代码。

### 安装 Visual Basic 6

以下步骤旨在将原始 Visual Basic 6 Enterprise 安装到 Windows 机器上。我们建议在虚拟机或 Docker 容器中执行此操作，因为某些支持文件非常陈旧，容易受到遗留攻击。

### 虚拟机（VM）

*虚拟机（VM）是物理计算机或服务器的软件模拟。它允许多个操作系统在单个物理机器上同时运行。每个操作系统都可以访问虚拟硬件资源，如 CPU、RAM 和存储。*
