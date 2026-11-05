
## 10 使用 Entity Framework Core 处理数据

本章介绍如何使用名为 **实体框架 Core (EF Core)** 的对象到数据存储映射技术，通过 Microsoft SQL Server、SQLite 和 Azure Cosmos DB 等数据存储进行读写。

本章将涵盖以下主题：

- 了解现代数据库
- 设置 EF Core
- 定义 EF Core 模型
- 查询 EF Core 模型
- EF Core 中的加载模式
- 使用 EF Core 操作数据
- 处理事务
- Code First EF Core 模型

### 了解现代数据库

存储数据的两个最常见位置是在 **关系数据库管理系统 (RDBMS)** 中，例如 Microsoft SQL Server、PostgreSQL、MySQL 和 SQLite，或在 **NoSQL** 数据库中，例如 Microsoft Azure Cosmos DB、Redis、MongoDB 和 Apache Cassandra。

### 了解旧版 Entity Framework

Entity Framework (EF) 最早于 2008 年底作为 .NET Framework 3.5 Service Pack 1 的一部分发布。此后，随着 Microsoft 观察到程序员在现实世界中使用对象关系映射 (ORM) 工具的方式，Entity Framework 不断发展。

ORM 使用映射定义将表中的列与类中的属性关联起来。然后，程序员可以以熟悉的方式与不同类型的对象交互，而不必处理如何将值存储在关系表或 NoSQL 数据存储提供的其他结构中。

随 .NET Framework 包含的 EF 版本是 Entity Framework 6 (EF6)。它成熟、稳定，并支持使用 EDMX（XML 文件）定义模型以及复杂继承模型和一些其他高级功能。

EF 6.3 及更高版本已从 .NET Framework 中提取为单独的包，以便可在 .NET Core 3.0 及更高版本上受支持。这使得现有的项目（如 Web 应用程序和服务）可以移植并跨平台运行。然而，EF6 应被视为遗留技术，因为它在跨平台运行时存在一些限制，并且不会再添加新功能。

### 使用旧版 Entity Framework 6.3 或更高版本

若要在 .NET Core 3.0 或更高版本的项目中使用旧版 Entity Framework，必须在项目文件中添加对其的包引用，如以下标记所示：

```
<PackageReference Include="EntityFramework" Version="6.4.4" />
```

![](img/c63b00dca7192cc58cfb9e1f1e64f483_438_0.png)

> 良好实践：只有在必须使用时才使用旧版 EF6，例如在迁移使用它的 WPF 应用时。本书关于现代跨平台开发，因此在本章剩余部分，我只会介绍现代的 Entity Framework Core。在本章的项目中，您无需引用上面所示的旧版 EF6 包。

### 了解 Entity Framework Core

真正的跨平台版本 EF Core 与旧版 Entity Framework 不同。虽然 EF Core 名称相似，但您应了解它与 EF6 的不同之处。最新的 EF Core 版本为 6.0，以匹配 .NET 6.0。

EF Core 5 及更高版本仅支持 .NET 5 及更高版本。EF Core 3.0 及更高版本仅在支持 .NET Standard 2.1 的平台上运行，即 .NET Core 3.0 及更高版本。它不支持 .NET Standard 2.0 平台，如 .NET Framework 4.8。

除了传统的 RDBMS 外，EF Core 还支持现代基于云的非关系型、无模式数据存储，例如 Microsoft Azure Cosmos DB 和 MongoDB，有时使用第三方提供程序。

EF Core 的改进如此之多，本章无法全部涵盖。我将重点介绍所有 .NET 开发人员应了解的基础知识以及一些更酷的新功能。

使用 EF Core 有两种方法：

- 1. **数据库优先**：数据库已存在，因此您构建一个与其结构和功能相匹配的模型。
- 2. **代码优先**：不存在数据库，因此您构建一个模型，然后使用 EF Core 创建一个与其结构和功能相匹配的数据库。

我们将从使用 EF Core 与现有数据库开始。

### 为使用 EF Core 创建控制台应用

首先，我们将为本章创建一个控制台应用项目：

- 1. 使用您喜欢的代码编辑器创建一个名为 `Chapter10` 的新解决方案/工作区。
- 2. 添加控制台应用项目，如以下列表中定义：
  - 1. 项目模板：**控制台应用** / `console`
  - 2. 工作区/解决方案文件和文件夹：`Chapter10`
  - 3. 项目文件和文件夹：`WorkingWithEFCore`

### 使用示例关系数据库

为了学习如何使用 .NET 管理 RDBMS，有一个示例数据库会很有用，这样您就可以在具有中等复杂度和相当数量示例记录的数据库上练习。Microsoft 提供了多个示例数据库，其中大多数对于我们的需求来说过于复杂，因此我们将使用一个名为 **Northwind** 的数据库，该数据库首次创建于 1990 年代初。

让我们花一点时间查看 Northwind 数据库的图表。您可以在本书编写代码和查询时参考以下图表：

![](img/c63b00dca7192cc58cfb9e1f1e64f483_440_0.png)

图 10.1：Northwind 数据库表及关系

您将在本章后面编写代码来处理 Categories 和 Products 表，并在后续章节中处理其他表。但在这样做之前，请注意：

- 每个类别都有唯一的标识符、名称、描述和图片。
- 每个产品都有唯一的标识符、名称、单价、库存单位和其他字段。
- 每个产品通过存储类别的唯一标识符与类别关联。
- Categories 和 Products 之间的关系是一对多，这意味着每个类别可以有零个或多个产品。

### 在 Windows 上使用 Microsoft SQL Server

Microsoft 为 Windows、Linux 和 Docker 容器提供其流行且功能强大的 SQL Server 产品的各种版本。我们将使用一个可独立运行的免费版本，即 SQL Server Developer Edition。您也可以使用 Express 版本或可随 Visual Studio for Windows 安装的免费 SQL Server LocalDB 版本。

如果您没有 Windows 计算机或想要使用跨平台数据库系统，则可以跳到 *使用 SQLite* 主题。

### 下载和安装 SQL Server

您可以通过以下链接下载 SQL Server 版本：

```
https://www.microsoft.com/en-us/sql-server/sql-server-downloads
```

- 1. 下载 **Developer** 版本。
- 2. 运行安装程序。
- 3. 选择 **Custom** 安装类型。
- 4. 选择安装文件的文件夹，然后单击 **Install**。
- 5. 等待 1.5 GB 的安装程序文件下载完成。
- 6. 在 **SQL Server Installation Center** 中，单击 **Installation**，然后单击 **New SQL Server stand-alone installation or add features to an existing installation**。
- 7. 选择 **Developer** 作为免费版本，然后单击 **Next**。
- 8. 接受许可条款，然后单击 **Next**。
- 9. 查看安装规则，修复任何问题，然后单击 **Next**。
- 10. 在 **Feature Selection** 中，选择 **Database Engine Services**，然后单击 **Next**。
- 11. 在 **Instance Configuration** 中，选择 **Default instance**，然后单击 **Next**。如果您已配置默认实例，则可以创建命名实例，也许称为 cs10dotnet6。
- 12. 在 **Server Configuration** 中，注意 **SQL Server Database Engine** 已配置为自动启动。将 **SQL Server Browser** 设置为自动启动，然后单击 **Next**。
- 13. 在 **Database Engine Configuration** 中，在 **Server Configuration** 选项卡上，将 **Authentication Mode** 设置为 **Mixed**，将 sa 账户密码设置为强密码，单击 **Add Current User**，然后单击 **Next**。
- 14. 在 **Ready to Install** 中，查看将要执行的操作，然后单击 **Install**。
- 15. 在 **Complete** 中，注意已成功执行的操作，然后单击 **Close**。
- 16. 在 **SQL Server Installation Center** 中，在 **Installation** 下，单击 **Install SQL Server Management Tools**。
- 17. 在浏览器窗口中，单击下载最新版本的 SSMS。
- 18. 运行安装程序并单击 **Install**。
- 19. 安装程序完成后，如有需要单击 **Restart** 或 **Close**。
