

> 注意 两章的内容远不足以涵盖 Entity Framework Core 的全部内容，因为整本书（有些甚至与本书规模相当）都只专门讲解 EF Core。这些章节的意图是让你获得足够的工作知识，以便开始在你的业务应用中使用 EF Core。

## 对象关系映射器

ADO.NET 为你提供了一个框架，让你能够使用连接、命令和数据读取器来选择、插入、更新和删除数据。虽然这本身不错，但 ADO.NET 的这些方面迫使你以紧密耦合物理数据库模式的方式处理获取到的数据。例如，回忆一下从数据库获取记录时，你打开一个连接，创建并执行一个命令对象，然后使用数据读取器通过数据库特定的列名迭代每一行记录。

当你使用 ADO.NET 时，你必须始终牢记后端数据库的物理结构。你必须知道每个数据表的模式，为与数据表交互编写可能复杂的 SQL 查询，跟踪对检索到（或添加的）数据的更改等。这可能迫使你编写相当冗长的 C# 代码，因为 C# 本身并不能直接使用数据库模式的语言。

更糟糕的是，物理数据库通常的构建方式完全集中在数据库构造上，例如外键、视图、存储过程和数据规范化，而不是面向对象编程。

应用程序开发人员的另一个关注点是更改跟踪。从数据库获取数据只是过程的一步，但任何更改、添加和/或删除都必须由开发人员跟踪，以便它们能持久化回数据存储。

.NET 中对象关系映射框架（通常称为 ORM）的可用性极大地增强了数据访问体验，因为它们为开发人员管理大部分创建、读取、更新和删除（CRUD）数据访问任务。开发人员在 .NET 对象和关系数据库之间创建映射，而 ORM 管理连接、查询生成、更改跟踪和持久化数据。这让开发人员可以自由地专注于应用程序的业务需求。

> 注意 重要的是要记住 ORM 并不是骑在彩虹上的神奇独角兽。每个决策都涉及权衡。ORM 减少了开发人员创建数据访问层的工作量，但在不当使用时也可能引入性能和扩展问题。ORM 适用于 CRUD 操作，而使用数据库的强大功能进行基于集合的操作。

尽管不同的 ORM 在操作方式和使用方式上略有差异，但它们本质上都有相同的组件和部件，并追求相同的目标——使数据访问操作更简单。实体是映射到数据库表的类。一种专门的集合类型包含一个或多个实体。更改跟踪机制跟踪实体的状态以及对其所做的任何更改、添加和/或删除，而一个中心构造作为总指挥控制操作。

## 理解 Entity Framework Core 的作用

在底层，EF Core 使用你在上一章中已经检查过的 ADO.NET 基础设施。与任何对数据存储的 ADO.NET 交互一样，EF Core 使用 ADO.NET 数据提供程序进行数据存储交互。在 EF Core 可以使用 ADO.NET 数据提供程序之前，它必须更新以完全集成到 EF Core 中。由于增加了这些功能，你可用的 EF Core 数据提供程序可能比 ADO.NET 数据提供程序更少。

EF Core 使用 ADO.NET 数据库提供程序模式的好处是，它使你能够在同一项目中结合 EF Core 和 ADO.NET 数据访问范式，从而增强你的能力。例如，使用 EF Core 提供连接、模式和表名用于批量复制操作，利用了 EF Core 的映射能力和 ADO.NET 内置的 BCP 功能。这种混合方法使 EF Core 成为你工具箱中的又一个工具。

当你看到有多少基本的数据访问管道以方便且高效的方式为你处理时，EF Core 很可能会成为你数据访问的首选机制。

> 注意 许多第三方数据库（例如 Oracle 和 MySQL）提供 EF 感知的提供程序。如果你不使用 SQL Server，请咨询你的数据库厂商获取详细信息或导航到 https://docs.microsoft.com/en-us/ef/core/providers 查看可用的 EF Core 数据提供程序列表。

EF Core 最适合在表单-数据（或 API-数据）形式的情况中融入开发流程。使用工作单元模式对少量实体进行操作以确保一致性是 EF Core 的最佳使用场景。它不太适合大规模数据操作，例如提取-转换-加载（ETL）数据仓库应用程序或大型报表场景。

## Entity Framework 的构建块

EF Core 的主要组件是 DbContext、ChangeTracker、DbSet 专门的集合类型、数据库提供程序以及应用程序的实体。要完成本节，请创建一个名为 AutoLot.Samples 的新控制台应用程序，并添加 Microsoft.EntityFrameworkCore、Microsoft.EntityFrameworkCore.Design 和 Microsoft.EntityFrameworkCore.SqlServer 包。

```text
dotnet new sln -n Chapter22_AllProjects
dotnet new console -lang c# -n AutoLot.Samples -o .\AutoLot.Samples -f net5.0
dotnet sln .\Chapter22_AllProjects.sln add .\AutoLot.Samples
dotnet add AutoLot.Samples package Microsoft.EntityFrameworkCore
dotnet add AutoLot.Samples package Microsoft.EntityFrameworkCore.Design
dotnet add AutoLot.Samples package Microsoft.EntityFrameworkCore.SqlServer
```

## DbContext 类

DbContext 是 EF Core 的总指挥组件，并通过 Database 属性提供对数据库的访问。DbContext 管理 ChangeTracker 实例，公开虚拟的 OnModelCreating 方法以访问 Fluent API，保存所有 DbSet<T> 属性，并提供 SaveChanges 方法将数据持久化到数据存储。它不是直接使用，而是通过继承 DbContext 的自定义类使用。DbSet<T> 属性就放在这个类中。

表 22-1 显示了 DbContext 的一些常用成员。

## 表 22-1. DbContext 的常用成员

| DbContext 的成员 | 意义 |
|---|---|
| Database | 提供对数据库相关信息和功能的访问，包括执行 SQL 语句。 |
| Model | 关于实体的形状、它们之间的关系以及它们如何映射到数据库的元数据。注意：此属性通常不直接交互。 |
| ChangeTracker | 提供对该 DbContext 跟踪的实体实例的信息和操作的访问。 |
| DbSet<T> | 并非真正是 DbContext 的成员，而是添加到自定义派生 DbContext 类的属性。这些属性的类型为 DbSet<T>，用于查询和保存应用程序实体的实例。对 DbSet<T> 属性的 LINQ 查询会被翻译成 SQL 查询。 |
| Entry | 提供对实体更改跟踪信息和操作的访问，例如显式加载相关实体或更改 EntityState。也可以在未跟踪的实体上调用以将其状态更改为已跟踪。 |
| Set<TEntity> | 创建可用于查询和持久化数据的 DbSet<T> 属性的实例。 |
| SaveChanges/SaveChangesAsync | 将所有实体更改保存到数据库并返回受影响的记录数。在事务（隐式或显式）中执行。 |
| Add/AddRange, Update/UpdateRange, Remove/RemoveRange | 用于添加、更新和删除实体实例的方法。更改只有在 SaveChanges 成功执行时才会被持久化。异步版本也可用。注意：虽然在派生的 DbContext 上可用，但这些方法通常直接在 DbSet<T> 属性上调用。 |
| Find | 查找具有给定主键值的指定类型的实体。异步版本也可用。注意：虽然在派生的 DbContext 上可用，但这些方法通常直接在 DbSet<T> 属性上调用。 |
| Attach/AttachRange | 开始跟踪实体（或实体列表）。异步版本也可用。注意：虽然在派生的 DbContext 上可用，但这些方法通常直接在 DbSet<T> 属性上调用。 |
| SavingChanges | 在调用 SaveChanges/SaveChangesAsync 开始时触发的事件。 |
| SavedChanges | 在调用 SaveChanges/SaveChangesAsync 结束时触发的事件。 |
| SaveChangesFailed | 在调用 SaveChanges/SaveChangesAsync 失败时触发的事件。 |
| OnModelCreating | 在模型初始化后但在最终确定之前调用。将 Fluent API 的方法放在此方法中以最终确定模型的形状。 |
| OnConfiguring | 用于创建或修改 DbContext 选项的构建器。每次创建 DbContext 实例时执行。注意：建议不要使用此方法，而是在运行时使用 DbContextOptions 配置 DbContext 实例，并在设计时使用 IDesignTimeDbContextFactory 的实例。 |

