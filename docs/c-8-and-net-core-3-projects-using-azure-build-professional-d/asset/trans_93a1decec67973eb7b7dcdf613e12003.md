

现在，我们需要创建我们的微服务。其逻辑实际上相对简单；该服务应执行以下操作：

- 检查销售订单队列是否有新请求；如果没有请求，则服务应在一段时间后再次检查（我们将设置为 1 分钟）
- 如果找到一条消息，则服务应将销售订单添加到数据库中
- 服务应向新队列发送一条消息，表明订单已成功

> 显然，此流程已被精简，但它本身是完全功能的。

## 创建我们的新 Docker 容器

首先，让我们创建一个新的控制台应用程序；我们将在 Linux 容器中编译和运行它。控制台应用程序的创建方式与其他任何控制台应用程序完全相同，但由于我们要处理 Linux，因此命名必须为小写且不能包含点号。这使得我们的解决方案看起来有点杂乱，但尽管如此，让我们创建 `salesorder-process`：

![](img/762492a8287f369cc1ac4621a212f193_382_0.png)

下一步是添加 Docker 支持。在这里，你可以手动创建 Dockerfile，但 Visual Studio 很贴心地为我们提供了帮助。右键单击项目，现在有一个“添加 | Docker 支持”选项：

![](img/762492a8287f369cc1ac4621a212f193_383_0.png)

你将看到可以创建 Windows 容器或 Linux 容器的选项；我们将选择 Linux。这将为你创建 Dockerfile。

Docker for Windows 要么运行 Linux 容器，要么运行 Windows 容器，但在撰写本文时，它不能同时运行两者（你可以切换）。看来 Microsoft 正全力押注 Linux 容器，因此我的建议是选择 Linux 并坚持使用它。当然，这只是我个人的看法。

Dockerfile 出现后，将出现一个新的右键上下文菜单。选择 **生成 Docker 镜像**：

![](img/762492a8287f369cc1ac4621a212f193_384_0.png)

经过相当长的构建过程后，你应该会收到镜像已成功创建的报告。为了检查这一点，让我们运行一个 PowerShell 实例并查看当前可用的镜像列表；列出镜像的命令如下：

```
docker images
```

你的新镜像应该列在顶部：

![](img/762492a8287f369cc1ac4621a212f193_384_1.png)

现在，我们可以运行此镜像，只是为了检查它是否构建正确。为此，请使用以下命令：

```
docker run salesorderprocess
```

你应该会看到控制台应用程序的输出；由于我们尚未更改任何内容，这是默认的 Hello World。

既然我们已成功创建容器，让我们开始添加构成微服务的逻辑。

## 创建微服务逻辑

逻辑本身相对简单；但是，你会注意到这里有一些空白，我们正在使用不存在或不可访问的方法和类。我们将首先将逻辑放入一个单独的类中；为了让类易于理解，我们将创建一个名为 Run 的公共函数：

```
public async Task Run()
{
    while (true)
    {
        if (!await ProcessEachMessage())
        {
            await Task.Delay(60000);
            continue;
        }
    }
}

public async Task<bool> ProcessEachMessage()
{
    SalesOrder.Models.SalesOrder? salesOrder = await
    _serviceBusHelper.GetNextOrderFromMessageQueue();

    if (salesOrder == null) return false;

    _salesOrderService.Create(salesOrder);
    await _serviceBusHelper.ConfirmSalesOrderToMessageQueue(salesOrder);

    return true;
}
```

如果我们确实从队列中获取了销售订单，我们会调用服务方法来创建销售订单；然后，我们向队列添加一条确认消息。

这是这种设计的基础；当你需要与系统的其他部分通信时，你需要通过一种不需要该部分立即（甚至根本不）响应的机制来进行。在这种情况下，如果我们停止此服务，然后运行生成器，生成器不会因为服务未运行而失败。

我们使用单独的类是为了能够注入依赖项；如你所见，我们正在使用几个，而且它们目前大多不存在。我们将稍后介绍那些，但让我们先介绍一下新类的构造函数：

```
private readonly IServiceBusHelper _serviceBusHelper;
private readonly IProductService _productService;
private readonly ISalesOrderService _salesOrderService;

public SalesOrderProcessor(IServiceBusHelper serviceBusHelper,
IProductService productService,
ISalesOrderService salesOrderService)
{
    _serviceBusHelper = serviceBusHelper;
    _productService = productService;
    _salesOrderService = salesOrderService;
}
```

此时，你需要添加对 SalesOrder.ServiceBus 和 SalesOrder.Models 项目的引用。我们现在已经到了这样一个阶段：SalesOrder.Generate 内部的大部分代码应该移至其自己的项目，以便从其他地方访问。由于这大部分是将代码从一个项目移到另一个项目，我不会详细介绍每个类，而只是展示新旧项目的结构；以下是移动后的 SalesOrder.Generate 项目：

![](img/762492a8287f369cc1ac4621a212f193_387_0.png)

如你所见，我们已将 SalesOrder.Generate 中的几乎所有内容移出，并转移到一个新项目：SalesOrder.Data。让我们看看 SalesOrder.Data 中有什么：

![](img/762492a8287f369cc1ac4621a212f193_387_1.png)

如你所见，我们只是将文件从一个项目复制并粘贴到另一个项目。唯一必要的其他步骤是重命名命名空间；例如，`ProductService` 的命名空间现在将是 `SalesOrder.Data.Products`。

> 在这种情况下，Ctrl + Shift + H 是你的最佳朋友，它将允许你将旧命名空间的所有实例替换为新命名空间。

你还需要添加一些项目引用；`SalesOrder.Generate` 项目现在应引用 `SalesOrder.Data` 项目。新的 `SalesOrder.Data` 项目反过来依赖于 `SalesOrder.Models`。

既然我们已经处理完这些，可以开始填充我们的存根函数了；让我们从 `SalesOrder` 服务开始。

> 为了说明方便，我们将服务和数据存储库类分组在单个项目中。通常，应该避免这样做；如果你决定扩展此项目，将存储库和服务分开可能是个好主意。

## 创建新的 Azure SQL 数据库

当你为容器编写代码时，尤其是在你希望获得顺畅的开发体验时，记住 Docker 命令变得不那么重要了；事实上，在本章的过程中，在开始将容器移至 AKS 之前，甚至有可能不需要接触命令行。因此，很容易忘记你正在容器内运行，而且是 Linux 容器。EF Core 允许你连接到 SQL 实例，因此连接到 (LocalDB) 会很容易。但是，你无法从容器内部访问它，因为它是仅限 Windows 的概念，并且当然存在于主机上，而不是容器中。

那么，我们可以从容器内部访问 SQL Server 吗？如果可以，怎么做？答案是你可以访问它，但你需要记住你在哪里：当你在容器内部时，你的 localhost 不是你的机器——而是你的容器。如果你想托管自己的 SQL Server 实例，当然可以，但你需要通过外部 IP 地址连接。

在我们的案例中，我们打算将此移至 AKS，因此将数据库托管在 Azure 上是合理的。让我们快速创建一个新的 Azure SQL 数据库和服务器。从 Azure 中，选择 SQL 数据库边栏选项卡：

![](img/762492a8287f369cc1ac4621a212f193_389_0.png)

