


- 测试应用程序
- 部署应用程序

这听起来有点复杂，但别担心，我们会一步一步来。让我们一起来看看。

## Entity Framework (EF) Core 历史

开发一个需要从某种数据库读取数据并向其中写入数据的应用程序时，最令人沮丧的部分之一，就是要在你的代码和数据库之间建立通信层。

至少过去是这样，直到 Entity Framework 出现！

Entity Framework 是一个 **对象关系映射器 (ORM)**。它将你的 .NET 代码对象映射到关系型数据库实体。就这么简单。现在，你不再需要为处理普通的 CRUD 操作而去搭建所需的数据访问代码。

Entity Framework 的第一个版本随 .NET 3.5 SP1 于 2008 年 8 月发布，最初的反响并不理想——以至于一群开发人员就该框架签署了一份*不信任票*。幸运的是，大部分提出的问题都得到了解决，Entity Framework 4.0 连同 .NET 4.0 的发布，让关于框架稳定性的许多批评不再被提及。

Entity Framework Core 是一次完整的重写。微软已表示，他们并非试图让 EF6 和 EF Core 功能完全等同；虽然它们支持许多相同的功能，但 EF Core 提供了机会来解决一些原本可能已不再合理的初始决策。

## Code-First 与 Model-First 与 Database-First 方法

使用 Entity Framework 时，你有三种实现方式可供选择。但是，在 Entity Framework Core 中，实际上只有一种：Code-First。尽管 Database-First 的概念确实存在，但它是单向的：也就是说，你可以从数据库反向工程生成代码模型，但之后，你将从代码生成数据库模型。

## 开发数据库设计

在我们知道自己在做什么之前，我们无法知道自己在做什么。在跳入并使用我们的数据库、模型和控制器创建解决方案之前，我们需要弄清楚如何设计数据库。

根据 Microsoft TechNet，我们可以遵循五个基本步骤来规划数据库：

- 收集信息
- 识别对象
- 建模对象
- 识别每个对象的信息类型
- 识别对象之间的关系

我们的需求非常简单。我们只需保存一个网络链接以便日后导航，因此不会有相互关联的多个对象。

不过，我们确实需要澄清想要为我们的对象（网络链接）保存哪些类型的信息。显然，我们需要 URL，但还需要什么？请确保你理解解决方案需要哪些信息以及它们将如何使用。

用日常生活中的例子来想——如果你为朋友家的房子写地址，你可能不仅需要街道，可能还需要知道朋友的名字或某种备注。

在我们的解决方案中，我们想知道 URL 是什么，但我们也想知道保存时间，并有一个地方来记录备注，以便为条目添加更多个人细节。因此，我们的模型将包含以下内容：

- URL
- DateSaved
- Notes

当我们开始创建模型时，我们会深入探讨更多细节，但不要操之过急。我们仍需先创建项目。

## 设置项目

使用 Visual Studio 2019，创建 ASP.NET Core Web 应用程序：

- 创建一个新的 ASP.NET Core Web 应用程序：

![](img/762492a8287f369cc1ac4621a212f193_167_0.png)

- 让我们将应用程序命名为 WebResearch。如下图所示：

![](img/762492a8287f369cc1ac4621a212f193_168_0.png)

- 在下一个屏幕上，选择 Web 应用程序 (模型-视图-控制器) 作为项目模板。为了保持简单，请将身份验证保持为无身份验证。请参阅以下屏幕截图：

![](img/762492a8287f369cc1ac4621a212f193_169_0.png)

- 创建的项目将如下所示：

![](img/762492a8287f369cc1ac4621a212f193_170_0.png)

现在我们已经设置了项目，让我们安装所需的包！

## 安装所需的包

我们需要为解决方案安装三个 NuGet 包，它们将在我们的探索之旅中提供帮助。这通过包管理器控制台完成。

转到 Tools | NuGet Package Manager | Package Manager Console：

![](img/762492a8287f369cc1ac4621a212f193_170_1.png)

我们之前见过 **包管理器控制台**；不过，值得指出的是，这不仅仅是它表面上的样子（或者说，不仅仅是其名称所暗示的那样）。**包管理器控制台**实际上是在你的解决方案上下文中运行的 PowerShell 控制台。

### EF Core SQL Server

EF Core 支持多种数据库提供程序，包括 Microsoft SQL Server、PostgreSQL、SQLite 和 MySQL。我们将在这里使用 SQL Server 作为数据库提供程序。

有关数据库提供程序的完整列表，请参阅 [官方 Microsoft 文档](https://docs.microsoft.com/en-us/ef/core/providers/index)。

在控制台窗口中，键入以下命令并按 *Enter*：

```
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

你应该会看到几行响应，显示成功安装的项目。

### EF Core 工具

接下来，我们将安装一些 EF Core 工具，以帮助我们从模型创建数据库。

在控制台窗口中，键入以下命令并按 *Enter*：

```
Install-Package Microsoft.EntityFrameworkCore.Tools
```

同样，你应该会看到几行响应，显示成功安装的项目。

### 代码生成设计

我们不需要亲自编写所有代码，可以使用一些 ASP.NET Core 代码生成工具来协助脚手架。

在控制台窗口中，键入以下命令并按 Enter：

```
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
```

如往常一样，检查是否出现了“成功安装”的项目。

如果你在安装任何 NuGet 包时遇到问题，这可能指向访问控制问题。一般而言，我将 Visual Studio 设置为以管理员身份运行，这可以解决大多数此类问题。

安装后，我们的解决方案将在 **依赖项** 部分下反映添加的 NuGet 包，如下所示：

![](img/762492a8287f369cc1ac4621a212f193_172_0.png)

### 创建模型

右键单击项目中的 **模型** 文件夹，添加一个名为 `ResearchModel.cs` 的类：

![](img/762492a8287f369cc1ac4621a212f193_173_0.png)

我们实际上需要两个类：一个是 Research 类，代表我们的实体对象；另一个是 ResearchContext 类，是 DbContext 的子类。为了保持简单，我们可以将这两个类都放在 ResearchModel 文件中。代码如下：

```
using Microsoft.EntityFrameworkCore;
using System;

namespace WebResearch.Models
{
    public class Research
    {
        public int Id { get; set; }
        public string Url { get; set; }
        public DateTime DateSaved { get; set; }
        public string Note { get; set; }
    }

    public class ResearchContext : DbContext
    {
        public ResearchContext(DbContextOptions<ResearchContext>
        options) : base(options)
        {
        }

        public DbSet<Research> ResearchLinks { get; set; }
    }
}
```

我们将其分解如下：

- 首先，我们有 Research 类，即我们的实体对象表示。正如我们在开发数据库设计部分提到的，对于每个链接，我们将保存 URL、日期和备注。ID 字段是存储信息的数据库表的标准做法。
- 我们的第二个类，ResearchContext，是 DbContext 的子类。该类将有一个接受 DbContextOptions 作为参数的空构造函数，以及一个用于数据集合的 DbSet<TEntity> 属性。

我可以在这里简要介绍 DbSet<TEntity>，但我宁愿让 Visual Studio 帮助我们。让我们来看下面的屏幕：

![](img/762492a8287f369cc1ac4621a212f193_174_0.png)

如果你将鼠标悬停在 DbSet 上，你将获得一个信息弹出窗口，包含你需要知道的一切！

### 配置服务

在 Startup.cs 类的 ConfigureServices 方法中，使用以下代码添加 DbContext 服务：

```
string connection =
Configuration.GetConnectionString("LocalDBConnection");
services.AddDbContext<ResearchContext>(options =>
options.UseSqlServer(connection));
```

如你所见，我们从配置中设置一个连接字符串变量，然后将其作为 SqlServer 的选项参数传递给我们的 DbContext。

