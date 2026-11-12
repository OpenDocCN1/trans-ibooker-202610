

```csharp
var item = new Models.Item();
item.Name = dirInfo.Name;
rootItem.Children.Add(item);
PopulateBookList(dirInfo.FullName, item);
}
```

在这里，我们正在递归遍历目录结构并填充我们的新模型。请注意，我们在开始时设置了项类型和 FullName（目录路径），然后遍历所有子目录，重新调用我们的方法。

> 递归是指从方法内部调用该方法本身的做法。在像这样希望对嵌套对象执行完全相同操作的场景中，它非常有用。它比使用循环更快；但是，如果使用不当，它有可能非常快地填满堆栈。

对于函数的第二部分，我们将处理当前目录中的任何文件（即，递归堆栈顶部在该时刻所在的目录）：

```csharp
foreach (FileInfo fileInfo in dir.GetFiles().Where(x =>
    AllowedExtensions.Contains(x.Extension)).ToList())
{
    var item = new Models.Item();
    item.Name = fileInfo.Name;

    item.FullName = fileInfo.FullName;
    item.ItemType = (Models.ItemType)Enum.Parse(typeof(Extension),
    fileInfo.Extension.TrimStart('.'), true);
    rootItem.Children.Add(item);
}
}
```

我们接下来的更改是针对 ItemInvoked 方法；新方法应如下所示：

```csharp
private void _tvFoundBooks_ItemInvoked(Windows.UI.Xaml.Controls.TreeView sender, Windows.UI.Xaml.Controls.TreeViewItemInvokedEventArgs args)
{
    var selectedItem = (Models.Item)args.InvokedItem;
    DocumentEngine engine = new DocumentEngine();
    string path = selectedItem.FullName.ToString();
    if (File.Exists(path))
    {
    var (dateCreated, dateLastAccessed, fileName, fileExtension,
    fileLength, hasError) =
    engine.GetFileProperties(selectedItem.FullName.ToString());

    if (!hasError)
    {
        txtFileName.Text = fileName;
        txtExtension.Text = fileExtension;
        dtCreated.Value = dateCreated;
        dtLastAccessed.Value = dateLastAccessed;
        txtFilePath.Text = selectedItem.FullName.ToString();
        txtFileSize.Text = $"{Round(fileLength.ToMegabytes(),
        2).ToString()} MB";
    }
    }
}
```

同样，这只是微小的更改；我们不再将完整文件名（带路径）存储在节点 `tag` 属性中，而是直接引用基础模型，因此更加清晰。我们的下一步是删除现有的 WinForms TreeView 控件。

## 删除现有的 TreeView

以下代码应从 ImportBooks.Designer.cs 中删除：

```csharp
//
// tvFoundBooks
//
this.tvFoundBooks.Location = new System.Drawing.Point(12, 41);
this.tvFoundBooks.Name = "tvFoundBooks";
this.tvFoundBooks.Size = new System.Drawing.Size(513, 246);
this.tvFoundBooks.TabIndex = 8;
this.tvFoundBooks.AfterSelect += new System.Windows.Forms.TreeViewEventHandler(this.tvFoundBooks_AfterSelect);
```

这将删除控件本身。稍后，我们需要删除将 TreeView 添加到控件集合的以下代码：

```csharp
this.Controls.Add(this.tvFoundBooks);
```

就这样。如果您现在运行项目，您将看到 WinForms 应用程序中间有一个 UWP TreeView 控件。

## 树摇（Tree shaking）和编译为单个可执行文件

Web 应用程序日益流行并超越其桌面应用的主要原因，是部署问题。乍看之下，这似乎是个微不足道的问题，但绝非如此。为了解决这个问题，已有许多尝试，从 ClickOnce 之类的技术到 App Store 模式（UWP、Apple 和 Google）。之所以在桌面世界中如此困难而在 Web 世界中如此简单，其中一个原因是：虽然两者都可能拥有复杂的依赖项树，但 Web 允许这些依赖项大多位于服务器上，因此无需直接部署到客户端机器。

.NET Core 3 的一个有用功能是能够将所有依赖项打包到单个可执行文件中。

![](img/762492a8287f369cc1ac4621a212f193_85_0.png)

过去可以使用 IL weaver 的概念来实现这一点。此主题超出了本书的范围；但是，因为 IL 不是编译的，所以它为在项目部署后修改它打开了大门。

在 .NET Core 3 中，我们可以通过将以下行添加到 .csproj 文件来将项目编译为单个可执行文件：

```xml
<PublishSingleFile>true</PublishSingleFile>
<RuntimeIdentifier>win-x64</RuntimeIdentifier>
<PublishReadyToRun>true</PublishReadyToRun>
```

发布应用程序时，您将获得一个单个可执行文件。

您甚至可以使用内置的树摇（Tree shaking）来减小此可执行文件的大小。这是删除应用程序未使用的依赖项的过程；这需要在 .csproj 文件中添加以下附加行：

```xml
<PublishTrimmed>true</PublishTrimmed>
```

![](img/762492a8287f369cc1ac4621a212f193_85_1.png)

撰写本文时，此方法不会复制资产（图像），因此直到该问题修复之前，您需要手动执行此操作。有关此功能的最新详细信息，请参阅以下链接：https://docs.microsoft.com/en-us/dotnet/core/whats-new/dotnet-core-3-0.

## 摘要

在本章中，我们将现有的 WinForms 应用程序迁移到了 .NET Core 3。这意味着，即使我们有一个已经使用了 10 年或 15 年的应用程序，也可以将其更新为使用最新框架，并利用 .NET Core 3 带来的性能改进和新功能。

利用此次升级，我们随后研究了 C# 8 的一个关键功能：可空引用类型。这意味着，无需使用任何第三方软件，我们就能够在遗留代码库中暴露出数十个潜在错误。

我们并没有止步于此；随后我们使用 XAML Islands 将 UWP 功能集成到 WinForms 中来扩展我们的应用程序。这可能是最令人兴奋的功能，因为它基本上意味着您可以从外到内重写遗留的 WinForms 应用程序。

在最后一部分，我们利用 .NET Core 3 中提供的新打包方法。我们使用树摇过程来减小输出文件的大小，并将代码编译为单个可执行文件。

和我一样，您可能会发现这些功能中存在一种规律。.NET Core 在此版本中纳入了仅限 Windows 的功能，这意味着您现在可以将遗留的 WinForms 应用转换为使用最新的 .NET Core，从而从所有性能改进中受益。您可以通过在 WPF 或 UWP 中使用组件创建来扩展它，而无需重写应用程序。此外，部署现在变得更加容易，因为我们现在可以编译为单个可执行文件。

在下一章中，我们将探讨 ASP.NET Core 3 并创建一个使用 Cosmos DB 的 MVC 应用程序。

## 2 使用 Cosmos DB 的 ASP.NET Core MVC 任务/错误日志记录应用

在本章中，我们将通过创建任务/错误日志记录应用程序，来了解如何将 Cosmos DB 与 ASP.NET Core MVC 结合使用。个人任务管理器很有用，而记录错误尤其方便，特别是在您无法立即处理它们时。

本章将涵盖以下主题：

- 在 Azure 上设置 Cosmos DB 实例
- Cosmos DB 的缩放和复制功能
- 创建 ASP.NET Core MVC 应用程序并集成 Cosmos DB

Cosmos DB 是 Microsoft 对名为 DocumentDB 的文档数据库的重新品牌化。除了提供 NoSQL 数据库的功能外，它还提供了几乎无需任何努力即可实现全局缩放的解决方案。

### 技术要求

本章的代码可在 GitHub 上找到：https://github.com/PacktPublishing/C-8-and-.NET-Core-3-Projects-Using-Azure-Second-Edition.

### 使用 Cosmos DB 的优势

值得花些时间探讨为什么您可能会选择使用 Cosmos 而不是众多其他数据库引擎之一。这里有两个问题：为什么选择云，以及为什么特别选择 Cosmos？

### 为什么选择云？缩放是 Microsoft 的工作，不是您的

由于 Cosmos DB 由 Microsoft 管理，转向它意味着您突然不需要担心一些事情。例如，您是否曾尝试配置 SQL Server 进行故障转移？或者如果您位于美国而客户在澳大利亚呢？Cosmos 按需付费的方式为您处理所有这些场景。显然，如果您希望数据在四大洲复制并具有巨大的吞吐量，那么您将支付更多费用。引用丘吉尔的话来说：

> “使用云解决方案是您能做的最昂贵的事情——除了所有其他选择之外！”

如果您曾参与过关于购买优质服务器、管理故障转移以及雇佣人员设置和支持这些服务器的讨论，您会发现这其实并不那么昂贵。

### 为什么选择 Cosmos？与行业领先 API 的兼容性和全球分布

Cosmos 支持所谓的多模型 API。这意味着，例如，如果您有一个使用 DocumentDB 或 MongoDB 的现有应用，您可以简单地将其指向 Cosmos 实例，并告诉它您想要使用该特定 API。您可以要求将数据复制到靠近数据访问位置的区域，从而降低延迟。

