### 第22章：介绍 Entity Framework Core

上一章探讨了 ADO.NET 的基础知识。 自 .NET 平台首次发布以来，ADO.NET 已使 .NET 程序员能够以相对直接的方式处理关系数据。 在 ADO.NET 的基础上，微软在 .NET 3.5 Service Pack 1 中引入了 ADO.NET API 的一个新组件，称为 Entity Framework 或简称 EF。

EF 的总体目标是让你能够使用直接映射到应用程序中业务对象 或 领域对象 的对象模型与关系数据库中的数据进行交互。 例如，与其将一批数据视为行和列的集合，不如对一组强类型对象 即 实体 的集合进行操作。 这些实体存储在专门的集合类中，该集合类感知 LINQ，从而可以使用 C# 代码进行数据访问操作。 集合类提供使用你在第 13 章中学到的相同 LINQ 语法对数据存储进行查询。

与 .NET Core 框架一样，Entity Framework Core 是 Entity Framework 6 的完全重写。 它构建在 .NET Core 框架之上，使 EF Core 能够在多个平台上运行。 重写 EF Core 使团队能够添加 EF 6 中无法合理实现的新功能和性能改进。

从头重新创建整个框架需要仔细考虑新框架中将支持哪些功能，哪些功能将被放弃。 EF 6 的一个功能不在 EF Core 中 也可能永远不会添加 是对 Entity Designer 的支持。 EF Core 仅支持代码优先开发范式。 如果你目前使用代码优先，可以安全地忽略上一句话。

> 注意 EF Core 可与现有数据库以及空白和/或新数据库一起使用。 这两种机制都称为代码优先，这可能不是最好的名称。 实体类和派生的 DbContext 可以从现有数据库生成，数据库也可以从实体类创建和更新。 你将在 EF Core 章节中学习这两种方法。

随着每次发布，EF Core 都增加了 EF 6 中存在的功能以及 EF 6 中从未存在的新功能。 3.1 版本显著缩短了 EF Core 相对于 EF 6 缺失的基本功能列表，而 5.0 版本又进一步缩小了差距。 事实上，对于大多数项目而言，EF Core 已包含你所需的一切。

本章和下一章将介绍使用 Entity Framework Core 进行数据访问。 你将学习如何创建领域模型，将实体类和属性映射到数据库表和列，实现更改跟踪，使用 EF Core 命令行界面 CLI 进行脚手架和迁移，以及 DbContext 类的作用。 你还将学习如何使用导航属性关联实体、事务和并发检查，仅举几项所探索的功能。

完成这些章节后，你将拥有我们 AutoLot 数据库的数据访问层的最终版本。 在进入 EF Core 之前，让我们先谈谈对象关系映射器 的一般概念。





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





## 创建派生 DbContext

EF Core 的第一步是创建一个继承自 DbContext 的自定义类。然后添加一个构造函数，该构造函数接受强类型的 `DbContextOptions` 实例（下一节介绍）并将该实例传递给基类。

```csharp
namespace AutoLot.Samples
{
    public class ApplicationDbContext : DbContext
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }
    }
}
```

这个类用于访问数据库、处理实体、变更跟踪器以及 EF Core 的所有组件。

## 配置 DbContext

DbContext 实例通过 `DbContextOptions` 类的实例进行配置。由于 `DbContextOptions` 类不应在代码中直接构造，因此 `DbContextOptions` 实例是通过 `DbContextOptionsBuilder` 创建的。通过 `DbContextOptionsBuilder` 实例，可以选择数据库提供程序（以及任何提供程序特定的设置），并设置 EF Core DbContext 的通用选项（例如日志记录）。然后在运行时将 `Options` 属性注入到基类 DbContext 中。

这种动态配置能力使得只需选择不同的选项（例如，使用 MySQL 而不是 SQL Server 提供程序）并创建派生 DbContext 的新实例，即可在运行时更改设置。

## 设计时 DbContext 工厂

设计时 DbContext 工厂是一个实现 `IDesignTimeDbContextFactory<T>` 接口的类，其中 T 是派生的 DbContext 类。该接口有一个方法 `CreateDbContext()`，你必须实现该方法来创建派生 DbContext 的实例。

下面的 `ApplicationDbContextFactory` 类使用 `CreateDbContext()` 方法为 `ApplicationDbContext` 类创建一个强类型的 `DbContextOptionsBuilder`，将数据库提供程序设置为 SQL Server 提供程序（使用第 21 章中的 Docker 实例连接字符串），然后创建并返回 `ApplicationDbContext` 的新实例：

```csharp
using System;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Design;
```

```csharp
namespace AutoLot.Samples
{
    public class ApplicationDbContextFactory : IDesignTimeDbContextFactory<ApplicationDbContext>
    {
        public ApplicationDbContext CreateDbContext(string[] args)
        {
            var optionsBuilder = new DbContextOptionsBuilder<ApplicationDbContext>();
            var connectionString = @"server=.,5433;Database=AutoLotSamples;User Id=sa;Password=P@ssw0rd;";
            optionsBuilder.UseSqlServer(connectionString);
            Console.WriteLine(connectionString);
            return new ApplicationDbContext(optionsBuilder.Options);
        }
    }
}
```

上下文工厂被命令行界面用来创建派生 DbContext 类的实例，以执行数据库迁移创建和应用等操作。由于它被设计为设计时构造而非运行时使用，因此开发数据库的连接字符串通常是硬编码的。

EF Core 5 中的新功能是，可以从命令行将参数传递给 `CreateDbContext()` 方法。本章后面会详细讲解。

## OnModelCreating

基类 DbContext 类公开了 `OnModelCreating` 方法，该方法用于使用 Fluent API 塑造实体。这将在本章后面深入介绍，但现在，请将以下代码添加到 `ApplicationDbContext` 类中：

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    // Fluent API calls go here
    OnModelCreatingPartial(modelBuilder);
}
partial void OnModelCreatingPartial(ModelBuilder modelBuilder);
```

## 保存更改

要触发 DbContext 和 ChangeTracker 持久化跟踪实体中的任何更改，请在派生 DbContext 上调用 `SaveChanges()`（或 `SaveChangesAsync()`）方法。

```csharp
static void SampleSaveChanges()
{
    //The factory is not meant to be used like this, but it’s demo code :-)
    var context = new ApplicationDbContextFactory().CreateDbContext(null);
    //make some changes
    context.SaveChanges();
}
```

在本章（以及本书）的其余部分中，将有许多保存更改的示例。

## 事务和保存点支持

EF Core 使用数据库的隔离级别将每次对 `SaveChanges`/`SaveChangesAsync` 的调用包装在隐式事务中。为了获得更多控制，你还可以将派生 DbContext 纳入显式事务中。要执行显式事务，请使用派生 DbContext 的 `Database` 属性创建事务。像平常一样进行操作，然后提交或回滚事务。以下代码片段演示了这一点：

```csharp
using var trans = context.Database.BeginTransaction();
try
{
    //Create, change, delete stuff
    context.SaveChanges();
    trans.Commit();
}
catch (Exception ex)
{
    trans.Rollback();
}
```

EF Core 5 中引入了 EF Core 事务的保存点。当调用 `SaveChanges()`/`SaveChangesAsync()` 时，如果事务已经在进行中，EF Core 会在该事务中创建一个保存点。如果调用失败，事务会回滚到保存点，而不是事务的开头。也可以通过调用事务上的 `CreateSavePoint()` 和 `RollbackToSavepoint()` 以编程方式管理保存点，如下所示：

```csharp
using var trans = context.Database.BeginTransaction();
try
{
    //Create, change, delete stuff
    trans.CreateSavepoint("check point 1");
    context.SaveChanges();
    trans.Commit();
}
catch (Exception ex)
{
    trans.RollbackToSavepoint("check point 1");
}
```

## 事务和执行策略

当执行策略处于活动状态时（例如，使用 `EnableRetryOnFailure()` 时），在创建显式事务之前，必须获取 EF Core 正在使用的当前执行策略的引用。然后调用策略上的 `Execute()` 方法来创建显式事务。

```csharp
var strategy = context.Database.CreateExecutionStrategy();
strategy.Execute(() =>
{
    using var trans = context.Database.BeginTransaction();
    try
    {
        actionToExecute();
        trans.Commit();
    }
    catch (Exception ex)
    {
        trans.Rollback();
    }
});
```

## 保存中/已保存更改事件

EF Core 5 引入了三个由 `SaveChanges()`/`SaveChangesAsync()` 方法触发的新事件。`SavingChanges` 在调用 `SaveChanges()` 时触发（但在对数据存储执行 SQL 语句之前），`SavedChanges` 在 `SaveChanges()` 完成后触发。以下（简单的）代码示例展示了事件及其处理程序的实际用法：

```csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
    SavingChanges += (sender, args) =>
    {
        Console.WriteLine($"Saving changes for {((DbContext)sender).Database.GetConnectionString()}");
    };
    SavedChanges += (sender, args) =>
    {
        Console.WriteLine($"Saved {args.EntitiesSavedCount} entities");
    };
    SaveChangesFailed += (sender, args) =>
    {
        Console.WriteLine($"An exception occurred! {args.Exception.Message} entities");
    };
}
```

## DbSet<T> 类

对于对象模型中的每个实体，你都需要添加一个类型为 `DbSet<T>` 的属性。`DbSet<T>` 类是一种专用的集合属性，用于与数据库提供程序交互，从而获取、添加、更新或删除数据库中的记录。每个 `DbSet<T>` 都为每个集合提供了一些核心服务，用于数据库交互。对 `DbSet<T>` 类执行的任何 LINQ 查询都会由数据库提供程序转换为数据库查询。表 22-2 描述了 `DbSet<T>` 类的一些核心成员。

## 表 22-2. DbSet<T> 的常见成员和扩展方法

| DbSet<T> 的成员 | 作用 |
|---|---|
| Add/AddRange | 开始以 Added 状态跟踪实体。调用 SaveChanges 时将添加该项。也提供异步版本。 |
| AsAsyncEnumerable | 将集合作为 IAsyncEnumerable<T> 返回。 |
| AsQueryable | 将集合作为 IQueryable<T> 返回。 |
| Find | 按主键在 ChangeTracker 中搜索实体。如果在变更跟踪器中找不到，则查询数据存储以获取该对象。也提供异步版本。 |
| Update/UpdateRange | 开始以 Modified 状态跟踪实体。调用 SaveChanges 时将更新该项。也提供异步版本。 |
| Remove/RemoveRange | 开始以 Deleted 状态跟踪实体。调用 SaveChanges 时将移除该项。也提供异步版本。 |
| Attach/AttachRange | 开始跟踪实体。将数值主键定义为 identity 且值等于零的实体跟踪为 Added。所有其他实体跟踪为 Unchanged。也提供异步版本。 |
| FromSqlRaw/FromSqlInterpolated | 基于表示 SQL 查询的原始字符串或插值字符串创建 LINQ 查询。可以与额外的 LINQ 语句组合以在服务器端执行。 |
| AsQueryable() | 从 DbSet<T> 返回 IQueryable<T> 实例。 |

`DbSet<T>` 实现了 `IQueryable<T>`，通常是 LINQ to Entities 查询的目标。除了 EF Core 添加的扩展方法之外，`DbSet<T>` 还支持你在第 13 章中学到的扩展方法，例如 `ForEach()`、`Select()` 和 `All()`。你将在“实体”部分中将 `DbSet<T>` 属性添加到 `ApplicationDbContext`。

> 注意：表 22-2 中列出的许多方法与表 22-1 中的方法名称相同。主要区别在于 `DbSet<T>` 方法已经知道要操作的类型并拥有实体列表，而 DbContext 方法必须使用反射来确定要操作的内容。使用 `DbSet<T>` 的方法比使用 DbContext 上的方法要常见得多。

## 查询类型

查询类型是 `DbSet<T>` 集合，用于表示视图、SQL 语句或无主键的表。早期版本的 EF Core 使用 `DbQuery<T>` 来实现这些，但从 EF Core 3.1 开始，`DbQuery` 类型已被弃用。查询类型通过 `DbSet<T>` 属性添加到派生的 DbContext 中，并配置为无键（keyless）。

例如，`CustomerOrderViewModel`（你将在构建完整的 AutoLot 数据访问库时创建）配置了 `Keyless` 特性。

```csharp
[Keyless]
public class CustomerOrderViewModel
{
...
}
```





配置的其余部分在 Fluent API 中进行。以下示例将实体设为无键，并将查询类型映射到 dbo.CustomerOrderView 数据库视图（请注意，如果模型上已有 Keyless 数据注释，则 HasNoKey() Fluent API 方法不是必需的，反之亦然，但在本文示例中为完整性起见仍显示）：

```
modelBuilder.Entity<CustomerOrderViewModel>().HasNoKey().ToView("CustomerOrderView", "dbo");
```

查询类型也可以映射到 SQL 查询，如下所示：

```
modelBuilder.Entity<CustomerOrderViewModel>().HasNoKey().ToSqlQuery(
    @"SELECT c.FirstName, c.LastName, i.Color, i.PetName, m.Name AS Make
      FROM   dbo.Orders o
      INNER JOIN dbo.Customers c ON o.CustomerId = c.Id
      INNER JOIN dbo.Inventory  i ON o.CarId = i.Id
      INNER JOIN dbo.Makes m ON m.Id = i.MakeId");
```

查询类型可使用的最后两种机制是 FromSqlRaw() 和 FromSqlInterpolated() 方法。以下是使用 FromSqlRaw() 的同一查询示例：

```
public IEnumerable<CustomerOrderViewModel> GetOrders()
{
    return CustomerOrderViewModels.FromSqlRaw(
        @"SELECT c.FirstName, c.LastName, i.Color, i.PetName, m.Name AS Make
          FROM   dbo.Orders o
          INNER JOIN dbo.Customers c ON o.CustomerId = c.Id
          INNER JOIN dbo.Inventory  i ON o.CarId = i.Id
          INNER JOIN dbo.Makes m ON m.Id = i.MakeId");
}
```

## 灵活的查询/表映射

EF Core 5 引入了将同一类映射到多个数据库对象的能力。这些对象可以是表、视图或函数。例如，第 21 章中的 CarViewModel 可以映射到返回带 Car 数据的 make 名称的视图以及 Inventory 表。EF Core 然后将从视图中查询并将更新发送到表。

```
modelBuilder.Entity<CarViewModel>()
    .ToTable("Inventory")
    .ToView("InventoryWithMakesView");
```

## ChangeTracker

ChangeTracker 实例跟踪在 DbContext 实例中加载到 DbSet<T> 内的对象的状态。表 22-3 列出了对象状态的可能值。

## 表 22-3. 实体状态枚举值

| Value | Meaning in Life |
| --- | --- |
| Added | The entity is being tracked but does not yet exist in the database. |
| Deleted | The entity is being tracked and is marked for deletion from the database. |
| Detached | The entity is not being tracked by the change tracker. |
| Modified | The entry is being tracked and has been changed. |
| Unchanged | The entity is being tracked, exists in the database, and has not been modified. |

如果需要检查对象的状态，请使用以下代码：

```
EntityState state = context.Entry(entity).State;
```

你也可以使用相同的机制以编程方式更改对象的状态。要将状态更改为 Deleted（例如），请使用以下代码：

```
context.Entry(entity).State = EntityState.Deleted;
```

## ChangeTracker 事件

ChangeTracker 可以引发两个事件。第一个是 StateChanged，第二个是 Tracked。StateChanged 事件在实体状态更改时触发。当实体首次被跟踪时不会触发。Tracked 事件在实体开始被跟踪时触发，无论是通过以编程方式添加到 DbSet<T> 实例还是从查询返回。

## 重置 DbContext 状态

EF Core 5 中新增了重置 DbContext 的能力。ChangeTracker.Clear() 方法通过将其状态设置为分离来清除 DbSet<T> 属性中的所有实体。

## 实体

映射到数据库表的强类型类正式称为实体。应用程序中的实体集合构成了物理数据库的概念模型。正式地说，该模型称为实体数据模型（EDM），通常简称为模型。模型映射到应用程序/业务域。实体及其属性使用 Entity Framework Core 约定、配置和 Fluent API（代码）映射到表和列。实体不需要直接映射到数据库模式。你可以自由地构建实体类以适应应用程序需求，然后将你的唯一实体映射到数据库模式。

数据库与实体之间的这种松耦合意味着你可以根据业务域塑造实体，而与数据库设计和结构无关。例如，考虑 AutoLot 数据库中简单的 Inventory 表和上一章中的 Car 实体类。名称不同，但 Car 实体映射到 Inventory 表。EF Core 检查模型中实体的配置，将 Inventory 表的客户端表示（在我们的示例中为 Car 类）映射到 Inventory 表的正确列。

接下来的几个部分详细说明了 EF Core 约定、数据注释和代码（使用 Fluent API）如何将模型中的实体、属性以及实体之间的关系映射到数据库中的表、列和外键关系。

## 将属性映射到列

在使用关系型数据存储时，EF Core 约定将所有读写公共属性映射到实体所映射表的列中。如果属性是自动属性，EF Core 通过 getter 和 setter 读写。如果属性具有支持字段，EF Core 将读写支持字段而不是公共属性，即使支持字段是私有的。虽然 EF Core 可以读写私有字段，但仍必须有一个封装支持字段的公共读写属性。

支持字段支持的两个场景是在 Windows Presentation Foundation（WPF）应用程序中使用 INotifyPropertyChanged 模式时，以及数据库默认值与 .NET Core 默认值冲突时。使用 EF Core 与 WPF 在第 28 章中介绍，数据库默认值稍后在本章中介绍。

列的名称、数据类型和可空性通过约定、数据注释和/或 Fluent API 配置。这些主题中的每一个将在本章后面深入介绍。

## 将类映射到表

EF Core 中有两种可用的类到表映射方案：表-per-层次结构（TPH）和表-per-类型（TPT）。TPH 映射是默认的，将继承层次结构映射到单个表。EF Core 5 中新增的 TPT 将层次结构中的每个类映射到其自己的表。

> 注：类也可以映射到视图和原始 SQL 查询。这些被称为查询类型，并在本章后面介绍。

## 表-per-层次结构映射（TPH）

考虑以下示例，显示了第 21 章中的 Car 类拆分为两个类：一个用于 Id 和 TimeStamp 属性的基类，以及剩余属性保留在 Car 类中。这两个类都应创建在 AutoLot.Samples 项目的 Models 目录中。

```
using System.Collections.Generic;

namespace AutoLot.Samples.Models
{
    public abstract class BaseEntity
    {
        public int Id { get; set; }
        public byte[] TimeStamp { get; set; }
    }
}
```

```
using System.Collections.Generic;
namespace AutoLot.Samples.Models
{
    public class Car : BaseEntity
    {
        public string Color { get; set; }
        public string PetName { get; set; }
        public int MakeId { get; set; }
    }
}
```

要使 EF Core 知道实体类是对象模型的一部分，请为实体添加 DbSet<T> 属性。将以下 using 语句添加到 ApplicationDbContext 类：

```
using AutoLot.Samples.Models;
```

将以下代码添加到 ApplicationDbContext 类的构造函数和 OnModelCreating() 方法之间：

```
public DbSet<Car> Cars { get; set; }
```

请注意，基类未作为 DbSet<T> 实例添加。即使迁移细节将在本章后面介绍，让我们创建数据库和 Cars 表。在与 AutoLot.Samples 项目相同目录的命令提示符中，运行以下命令（全部在一行）：

```
dotnet tool install --global dotnet-ef --version 5.0.1
dotnet ef migrations add TPH -o Migrations -c AutoLot.Samples.ApplicationDbContext
dotnet ef database update TPH -c AutoLot.Samples.ApplicationDbContext
```

第一个命令将 EF Core 命令行工具作为全局工具安装。这只需在你的机器上执行一次。第二个命令使用 AutoLot.Samples 命名空间中的 ApplicationDbContext 在 Migrations 目录中创建名为 TPH 的迁移。第三个命令从 TPH 迁移更新数据库。

当 EF Core 用于在数据库中创建此表时，继承的 BaseEntity 类合并到 Car 类中，并创建单个表，如下所示：

```
CREATE TABLE [dbo].[Cars](
[Id] [int] IDENTITY(1,1) NOT NULL,
[MakeId] [int] NOT NULL,
[Color] [nvarchar](max) NULL,
[PetName] [nvarchar](max) NULL,
[TimeStamp] [varbinary](max) NULL,
CONSTRAINT [PK_Cars] PRIMARY KEY CLUSTERED
(
[Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]
```

前面的示例依赖于 EF Core 约定（即将介绍）来创建表和列属性。

## 表-per-类型映射（TPT）

为了探索 TPT 映射方案，可以使用相同的实体，即使基类标记为抽象。由于 TPH 是默认的，EF Core 必须被指示将每个类映射到表。这可以使用数据注释或 Fluent API 完成。将以下代码添加到 ApplicationDbContext：

```
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<BaseEntity>().ToTable("BaseEntities");
    modelBuilder.Entity<Car>().ToTable("Cars");
    OnModelCreatingPartial(modelBuilder);
}
partial void OnModelCreatingPartial(ModelBuilder modelBuilder);
```





要“重置”数据库和项目，请删除 Migrations 文件夹和数据库。要使用 CLI 强制删除数据库，请输入以下命令：

```
dotnet ef database drop -f -c AutoLot.Samples.ApplicationDbContext
```

现在为 TPT 模式创建并应用迁移。

```
dotnet ef migrations add TPT -o Migrations -c AutoLot.Samples.ApplicationDbContext
dotnet ef database update TPT -c AutoLot.Samples.ApplicationDbContext
```

EF Core 在更新数据库时会创建以下表。索引也表明表之间存在一对一映射。

```
CREATE TABLE [dbo].[BaseEntities](
    [Id] [int] IDENTITY(1,1) NOT NULL,
    [TimeStamp] [varbinary](max) NULL,
    CONSTRAINT [PK_BaseEntities] PRIMARY KEY CLUSTERED
(
    [Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]
GO
```

```
CREATE TABLE [dbo].[Inventory](
    [Id] [int] NOT NULL,
    [MakeId] [int] NOT NULL,
    [Color] [nvarchar](max) NULL,
    [PetName] [nvarchar](max) NULL,
    CONSTRAINT [PK_Inventory] PRIMARY KEY CLUSTERED
(
    [Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]
GO
```

```
ALTER TABLE [dbo].[Inventory] WITH CHECK ADD  CONSTRAINT [FK_Inventory_BaseEntities_Id]
FOREIGN KEY([Id])
REFERENCES [dbo].[BaseEntities] ([Id])
GO
ALTER TABLE [dbo].[Inventory] CHECK CONSTRAINT [FK_Inventory_BaseEntities_Id]
GO
```

> **注意：** 表-per-类型映射具有显著的性能影响，在使用此映射方案之前应予以考虑。有关更多信息，请参阅文档：https://docs.microsoft.com/en-us/ef/core/performance/modeling-for-performance#inheritance-mapping。

要“重置”数据库和项目以准备下一组示例，请注释掉 `OnModelCreating()` 方法中的代码，并再次删除 Migrations 文件夹和数据库。

```
dotnet ef database drop -f -c AutoLot.Samples.ApplicationDbContext
```

## 导航属性和外键

*导航属性* 表示实体类之间的关系，并使代码能够从一个实体实例遍历到另一个实体实例。根据定义，导航属性是映射到数据库提供程序定义的非标量类型的任何属性。实际上，导航属性映射到另一个实体（称为 *引用导航属性*）或另一个实体的集合（称为 *集合导航属性*）。在数据库端，导航属性被转换为表之间的外键关系。EF Core 直接支持一对一、一对多以及（EF Core 5 中的新功能）多对多关系。实体类也可以具有指向自身的导航属性，表示自引用表。

> **注意：** 我认为将具有导航属性的对象视为链表是有帮助的，如果导航属性是双向的，则对象就像双向链表一样。

在介绍导航属性和实体关系模式的细节之前，请参阅表 22-4。这些术语在所有三种关系模式中都使用。

**表 22-4. 用于描述导航属性和关系的术语**

| 术语 | 生活中含义 |
|---|---|
| 主体实体 | 关系的父级。 |
| 从属实体 | 关系的子级。 |
| 主体键 | 用于定义主体实体的属性/属性。可以是主键或备用键。键可以使用单个属性或多个属性进行配置。 |
| 外键 | 子实体保存主体键的属性/属性。 |
| 必需关系 | 外键值是必需的（非空）关系。 |
| 可选关系 | 外键值不是必需的（可空）关系。 |

### 缺失的外键属性

如果具有引用导航属性的实体没有用于外键值的属性，EF Core 将在实体上创建必要的属性/属性。这些被称为 *影子外键属性*，其命名格式为 `<导航属性名称><主体键属性名称>` 或 `<主体实体名称><主体键属性名称>`。这适用于所有关系类型（一对多、一对一、多对多）。在实体中构建显式外键属性/属性比让 EF Core 为您创建要干净得多。

### 一对多关系

要创建一对多关系，位于一侧（主体）的实体类添加主体侧实体类的集合属性（从属）。从属实体还应具有指向主体的外键属性。如果没有，EF Core 将创建影子外键属性，如前所述。

例如，在第 21 章中创建的数据库中，Makes 表（由 Make 实体类表示）和 Inventory 表（由 Car 实体类表示）之间存在一对多关系。为了使这些示例保持简单，Car 实体将映射到 Cars 表。以下代码显示了表示此关系的双向导航属性：

```
using System.Collections.Generic;
namespace AutoLot.Samples.Models
{
    public class Make : BaseEntity
    {
        public string Name { get; set; }
        public IEnumerable<Car> Cars { get; set; } = new List<Car>();
    }
}
```

```
using System.Collections.Generic;
namespace AutoLot.Samples.Models
{
    public class Car : BaseEntity
    {
        public string Color { get; set; }
        public string PetName { get; set; }
        public int MakeId { get; set; }
        public Make MakeNavigation { get; set; }
    }
}
```

> **注意：** 在脚手架现有数据库时，EF Core 将引用导航属性命名为与属性类型名相同（例如，`public Make Make { get; set; }`）。这会导致导航和 IntelliSense 出现问题，更不用说使代码难以使用。我更喜欢为引用导航属性添加后缀 `Navigation` 以提高清晰度，如前例所示。

在 Car/Make 示例中，Car 实体是从属实体（一对多中的多），Make 实体是主体实体（一对多中的一）。

将 `DbSet<Make>` 实例添加到 `ApplicationDbContext`，如下所示：

```
public DbSet<Car> Cars { get; set; }
public DbSet<Make> Makes { get; set; }
```

使用以下命令创建迁移并更新数据库：

```
dotnet ef migrations add One2Many -o Migrations -c AutoLot.Samples.ApplicationDbContext
dotnet ef database update One2Many -c AutoLot.Samples.ApplicationDbContext
```

当使用 EF Core 迁移更新数据库时，会创建以下表：

```
CREATE TABLE [dbo].[Makes](
    [Id] [int] IDENTITY(1,1) NOT NULL,
    [Name] [nvarchar](max) NULL,
    [TimeStamp] [varbinary](max) NULL,
    CONSTRAINT [PK_Makes] PRIMARY KEY CLUSTERED
(
    [Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]
GO
```

```
CREATE TABLE [dbo].[Cars](
    [Id] [int] IDENTITY(1,1) NOT NULL,
    [Color] [nvarchar](max) NULL,
    [PetName] [nvarchar](max) NULL,
    [TimeStamp] [varbinary](max) NULL,
    [MakeId] [int] NOT NULL,
    CONSTRAINT [PK_Cars] PRIMARY KEY CLUSTERED
(
    [Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]
GO
```

```
ALTER TABLE [dbo].[Cars] WITH CHECK ADD  CONSTRAINT [FK_Cars_Makes_MakeId] FOREIGN KEY([MakeId])
REFERENCES [dbo].[Makes] ([Id])
ON DELETE CASCADE
GO
ALTER TABLE [dbo].[Cars] CHECK CONSTRAINT [FK_Cars_Makes_MakeId]
GO
```

请注意在从属表（Cars）上创建的外键和检查约束。

### 一对一关系

在一对一关系中，两个实体都具有指向另一个实体的引用导航属性。虽然一对多关系清楚地指出了主体和从属实体，但在构建一对一关系时，EF Core 必须被告知哪一侧是主体，要么是通过具有指向主体实体的明确定义的外键，要么是通过使用 Fluent API 指示主体。如果 EF Core 未被告知，它将根据其检测外键的能力进行选择。实际上，您应该通过添加外键属性来明确定义从属方。

```
namespace AutoLot.Samples.Models
{
    public class Car : BaseEntity
    {
```



```
public string Color { get; set; }
public string PetName { get; set; }
public int MakeId { get; set; }
public Make MakeNavigation { get; set; }
public Radio RadioNavigation { get; set; }
}
```

```
namespace AutoLot.Samples.Models
{
    public class Radio : BaseEntity
    {
        public bool HasTweeters { get; set; }
        public bool HasSubWoofers { get; set; }
        public string RadioId { get; set; }
        public int CarId { get; set; }
        public Car CarNavigation { get; set; }
    }
}
```

由于Radio具有指向Car类的外键（基于约定，稍后介绍），因此Radio是依赖实体，Car是主体实体。EF Core会在依赖实体的外键属性上隐式创建所需的唯一索引。如果要更改索引名称，可以通过数据注释或Fluent API来实现。

将 `DbSet<Radio>` 添加到 `ApplicationDbContext`。

```
public virtual DbSet<Car> Cars { get; set; }
public virtual DbSet<Make> Makes { get; set; }
public virtual DbSet<Radio> Radios { get; set; }
```

使用以下命令创建迁移并更新数据库：

```
dotnet ef migrations add One2One -o Migrations -c AutoLot.Samples.ApplicationDbContext
dotnet ef database update One2One -c AutoLot.Samples.ApplicationDbContext
```

使用EF Core迁移更新数据库时，Cars表保持不变，并创建以下Radios表：

```
CREATE TABLE [dbo].[Radios](
    [Id] [int] IDENTITY(1,1) NOT NULL,
    [HasTweeters] [bit] NOT NULL,
    [HasSubWoofers] [bit] NOT NULL,
    [RadioId] [nvarchar](max) NULL,
    [TimeStamp] [varbinary](max) NULL,
    [CarId] [int] NOT NULL,
    CONSTRAINT [PK_Radios] PRIMARY KEY CLUSTERED
    (
        [Id] ASC
    )WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]
GO
```

```
ALTER TABLE [dbo].[Radios] WITH CHECK ADD CONSTRAINT [FK_Radios_Cars_CarId] FOREIGN KEY([CarId])
REFERENCES [dbo].[Cars] ([Id])
ON DELETE CASCADE
GO
ALTER TABLE [dbo].[Radios] CHECK CONSTRAINT [FK_Radios_Cars_CarId]
GO
```

请注意在依赖表（Radios）上创建的外键和检查约束。

## 多对多关系（EF Core 5 新增）

在多对多关系中，两个实体都拥有指向对方的集合导航属性。在数据存储中，这是通过在两个实体表之间使用联接表实现的。该联接表以两个表的名称命名，格式为<Entity1Entity2>。可以通过Fluent API以编程方式更改名称。联接实体与每个实体表之间都是一对多关系。

```
namespace AutoLot.Samples.Models
{
    public class Car : BaseEntity
    {
        public string Color { get; set; }
        public string PetName { get; set; }
        public int MakeId { get; set; }
        public Make MakeNavigation { get; set; }
        public Radio RadioNavigation { get; set; }
        public IEnumerable<Driver> Drivers { get; set; } = new List<Driver>();
    }
}
```

```
namespace AutoLot.Samples.Models
{
    public class Driver : BaseEntity
    {
        public string FirstName { get; set; }
        public string LastName { get; set; }
        public IEnumerable<Car> Cars { get; set; } = new List<Car>();
    }
}
```

等效的做法是显式创建这三张表，在EF Core 5之前的版本中必须这样做。下面是一个简化的示例：

```
public class Driver
{
    ...
    public IEnumerable<CarDriver> CarDrivers { get; set; }
}
```

```
public class Car
{
...
    public IEnumerable<CarDriver> CarDrivers { get; set; }
}
public class CarDriver
{
    public int CarId {get;set;}
    public Car CarNavigation {get;set;}
    public int DriverId {get;set;}
    public Driver DriverNavigation {get;set;}
}
```

将 `DbSet<Driver>` 添加到 `ApplicationDbContext`。

```
public virtual DbSet<Car> Cars { get; set; }
public virtual DbSet<Make> Makes { get; set; }
public virtual DbSet<Radio> Radios { get; set; }
public virtual DbSet<Driver> Drivers { get; set; }
```

使用以下命令创建迁移并更新数据库：

```
dotnet ef migrations add Many2Many -o Migrations -c AutoLot.Samples.ApplicationDbContext
dotnet ef database update many2Many -c AutoLot.Samples.ApplicationDbContext
```

使用EF Core迁移更新数据库时，Cars表保持不变，并创建Drivers和CarDriver表。

```
CREATE TABLE [dbo].[Drivers](
    [Id] [INT] IDENTITY(1,1) NOT NULL,
    [FirstName] [NVARCHAR](MAX) NULL,
    [LastName] [NVARCHAR](MAX) NULL,
    [TimeStamp] [VARBINARY](MAX) NULL,
    CONSTRAINT [PK_Drivers] PRIMARY KEY CLUSTERED
(
    [Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]
GO
```

```
CREATE TABLE [dbo].[CarDriver](
    [CarsId] [int] NOT NULL,
    [DriversId] [int] NOT NULL,
    CONSTRAINT [PK_CarDriver] PRIMARY KEY CLUSTERED
(
    [CarsId] ASC,
    [DriversId] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
```

```
GO
ALTER TABLE [dbo].[CarDriver] WITH CHECK ADD CONSTRAINT [FK_CarDriver_Cars_CarsId] FOREIGN KEY([CarsId])
REFERENCES [dbo].[Cars] ([Id])
ON DELETE CASCADE
GO
ALTER TABLE [dbo].[CarDriver] CHECK CONSTRAINT [FK_CarDriver_Cars_CarsId]
GO
ALTER TABLE [dbo].[CarDriver] WITH CHECK ADD CONSTRAINT [FK_CarDriver_Drivers_DriversId] FOREIGN KEY([DriversId])
REFERENCES [dbo].[Drivers] ([Id])
ON DELETE CASCADE
GO
ALTER TABLE [dbo].[CarDriver] CHECK CONSTRAINT [FK_CarDriver_Drivers_DriversId]
GO
```

请注意，复合主键、检查约束（外键）以及级联行为都是由EF Core创建的，以确保CarDriver表被配置为正确的联接表。

> 注意 在撰写本文时，尚不支持对多对多关系进行脚手架搭建。多对多关系是基于表结构进行脚手架搭建的，如第二个包含CarDriver实体的示例所示。该问题正在这里跟踪：https://github.com/dotnet/efcore/issues/22475。

## 级联行为

大多数数据存储（如SQL Server）都有控制删除行时行为的规则。如果相关（依赖）记录也应被删除，则称为级联删除。在EF Core中，当删除主体实体（且依赖实体已加载到内存中）时，可能发生三种操作。

- 删除依赖记录。
- 将依赖实体的外键设置为null。
- 依赖实体保持不变。

可选关系和必需关系的默认行为不同。该行为也可以配置为七个值之一，尽管只有五个值推荐使用。该行为通过Fluent API使用DeleteBehavior枚举进行配置。该枚举中可用的选项如下：

- Cascade
- ClientCascade
- ClientNoAction（不建议使用）
- ClientSetNull
- NoAction（不建议使用）
- SetNull
- Restrict

在EF Core中，仅在删除实体并在派生的DbContext上调用SaveChanges()之后，才会触发指定的行为。有关EF Core何时与数据存储交互的更多详细信息，请参阅“查询执行”一节。

## 可选关系

回顾表22-4，可选关系是指依赖实体可以将外键值设置为null的关系。对于可选关系，默认行为是ClientSetNull。表22-5显示了使用SQL Server时，依赖实体的级联行为以及对数据库记录的影响。

### 表22-5. 可选关系下的级联行为

| 删除行为 | 对依赖实体的影响（内存中） | 对依赖实体的影响（数据库中） |
|---|---|---|
| Cascade | 实体被删除。 | 数据库删除实体。 |
| ClientCascade | 实体被删除。 | 对于不支持级联删除的数据库，EF Core会删除实体。 |
| ClientSetNull（默认） | 外键属性被设置为null。 | 无。 |
| SetNull | 外键属性被设置为null。 | 外键属性被设置为null。 |
| Restrict | 无。 | 无。 |

## 必需关系

必需关系是指依赖实体不能将外键值设置为null的关系。对于必需关系，默认行为是Cascade。表22-6显示了使用SQL Server时，依赖实体的级联行为以及对数据库记录的影响。

### 表22-6. 必需关系下的级联行为

| 删除行为 | 对依赖实体的影响（内存中） | 对依赖实体的影响（数据库中） |
|---|---|---|
| Cascade（默认） | 实体被删除。 | 实体被删除。 |
| ClientCascade | 实体被删除。 | 对于不支持级联删除的数据库，EF Core会删除实体。 |
| ClientSetNull | SaveChanges抛出异常。 | 无。 |
| SetNull | SaveChanges抛出异常。 | SaveChanges抛出异常。 |
| Restrict | 无。 | 无。 |




# 实体约定

EF Core 使用许多约定来定义实体及其与数据存储的关系。除非被数据注解或 Fluent API 中的代码覆盖，否则这些约定始终启用。表 22-7 列出了一些更重要的 EF Core 约定。

## 表 22-7：一些 EF Core 约定

| 约定 | 作用 |
|---|---|
| 包含的表 | 所有具有 DbSet 属性的类以及所有可以通过 DbSet 类（通过导航属性）访问的类都会在数据库中创建。 |
| 包含的列 | 所有具有 getter 和 setter 的公共属性（包括自动属性）都会映射到列。 |
| 表名 | 映射到派生 DbContext 中 DbSet 属性的名称。如果不存在 DbSet，则使用类名。 |
| 架构 | 表在数据存储的默认架构（SQL Server 上的 dbo）中创建。 |
| 列名 | 列名映射到类的属性名。 |
| 列数据类型 | 数据类型基于 .NET Core 数据类型选择，并由数据库提供程序（SQL Server）转换。DateTime 映射到 datetime2(7)，string 映射到 nvarchar(max)。作为主键一部分的字符串映射到 nvarchar(450)。 |
| 列可空性 | 不可为 null 的数据类型创建为 Not Null 持久化列。EF Core 遵循 C# 8 的可空性。 |
| 主键 | 名为 Id 或 <EntityTypeName>Id 的属性将被配置为主键。short、int、long 或 Guid 类型的键值由数据存储控制。数值创建为标识列（SQL Server）。 |
| 关系 | 当两个实体类之间存在导航属性时，会创建表之间的关系。 |
| 外键 | 名为 <OtherClassName>Id 的属性是类型为 <OtherClassName> 的导航属性的外键。 |

之前的导航属性示例都利用 EF Core 约定来构建表之间的关系。

## 将属性映射到列

按照约定，公共读写属性映射到同名列。数据类型与数据存储中属性的 CLR 数据类型的等效类型匹配。不可为 null 的属性在数据存储中设置为 not null，可空属性设置为允许 null。EF Core 支持 C# 8 中引入的可空引用类型。

对于后备字段，EF Core 期望后备字段使用以下约定之一命名（按优先级顺序）：

- `_ <camel-cased property name>`
- `_ <property name>`
- `m_ <camel-cased property name>`
- `m_ <property name>`

如果 Car 类的 Color 属性更新为使用后备字段，那么该后备字段（按照约定）需要命名为 _color、_Color、m_color 或 m_Color 之一，如下所示：

```
private string _color = "Gold";
public string Color
{
    get => _color;
    set => _color = value;
}
```

## Entity Framework 数据注解

数据注解是 C# 特性，用于进一步塑造实体。表 22-8 列出了一些最常用的数据注解，用于定义实体类和属性如何映射到数据库表和字段。数据注解会覆盖任何冲突的约定。还有更多注解可用于优化模型中的实体，正如你将在本章和本书其余部分看到的那样。

### 表 22-8：Entity Framework Core 支持的一些数据注解（*EF Core 5 中的新特性）

| 数据注解 | 作用 |
|---|---|
| Table | 定义实体的架构和表名。 |
| Keyless* | 表示实体没有键（例如，表示数据库视图）。 |
| Column | 定义实体属性的列名。 |
| BackingField* | 指定属性的 C# 后备字段。 |
| Key | 定义实体的主键。键字段隐式也是 [Required]。 |
| Index* | 放置在类上以指定单列或多列索引。允许指定索引是唯一的。 |
| Owned | 声明该类将由另一个实体类拥有。 |
| Required | 声明该属性在数据库中不可为空。 |
| ForeignKey | 声明用作导航属性外键的属性。 |
| InverseProperty | 声明关系另一端的导航属性。 |
| StringLength | 指定字符串属性的最大长度。 |
| TimeStamp | 在 SQL Server 中声明类型为 rowversion，并为涉及该实体的数据库操作添加并发检查。 |
| ConcurrencyCheck | 标记字段在执行更新和删除时用于并发检查。 |
| DatabaseGenerated | 指定字段是否由数据库生成。接受 DatabaseGeneratedOption 值：Computed、Identity 或 None。 |
| DataType | 提供比固有数据类型更具体的字段定义。 |
| NotMapped | 将属性或类排除在数据库字段和表之外。 |

以下代码显示了 BaseEntity 类，其中包含将 Id 字段声明为主键的注解。Id 属性上的第二个数据注解表示它是 SQL Server 中的标识列。TimeStamp 属性将是 SQL Server 的 timestamp/rowversion 属性（用于并发检查，本章稍后介绍）。

```
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;
public abstract class BaseEntity
{
    [Key, DatabaseGenerated(DatabaseGeneratedOption.Identity)]
    public int Id { get; set; }
    [Timestamp]
    public byte[] TimeStamp { get; set; }
}
```

以下是 Car 类及在数据库中定义其结构的数据注解：

```
using System.Collections.Generic;
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;
using Microsoft.EntityFrameworkCore;

[Table("Inventory", Schema="dbo")]
[Index(nameof(MakeId), Name = "IX_Inventory_MakeId")]
public class Car : BaseEntity
{
    [Required, StringLength(50)]
    public string Color { get; set; }
    [Required, StringLength(50)]
    public string PetName { get; set; }
    public int MakeId { get; set; }
    [ForeignKey(nameof(MakeId))]
    public Make MakeNavigation { get; set; }
    [InverseProperty(nameof(Driver.Cars))]
    public IEnumerable<Driver> Drivers { get; set; }
}
```

Table 特性将 Car 类映射到 dbo 架构中的 Inventory 表（Column 特性用于更改列名或数据类型）。Index 特性在外键 MakeId 上创建索引。两个文本字段设置为 Required，且最大 StringLength 为 50 个字符。InverseProperty 和 ForeignKey 特性将在下一节中解释。

与 EF Core 约定相比的更改如下：

- 将表从 Cars 重命名为 Inventory
- 将 TimeStamp 列从 varbinary(max) 更改为 SQL Server timestamp 数据类型
- 将 Color 和 PetName 列的数据类型和可空性从 nvarchar(max)/null 设置为 nvarchar(50)/not null
- 重命名 MakeId 上的索引

其余使用的注解与 EF Core 约定定义的配置匹配。

如果你创建迁移并尝试应用它，迁移将失败。SQL Server 不允许将现有列从另一种数据类型更改为 timestamp 数据类型。必须删除并重新创建该列。不幸的是，迁移基础设施不会删除并重新创建。它会尝试更改列。

解决这个问题的最简单方法是注释掉基实体上的 TimeStamp 属性，创建并应用迁移，然后取消注释 TimeStamp 并创建和应用另一个迁移。

注释掉 TimeStamp 属性和数据注解，然后执行以下命令：

```
dotnet ef migrations add RemoveTimeStamp -o Migrations -c AutoLot.Samples.ApplicationDbContext
dotnet ef database update RemoveTimeStamp  -c AutoLot.Samples.ApplicationDbContext
```

取消注释 TimeStamp 属性和数据注解，然后运行以下命令，将该属性作为 timestamp 列添加到每个表中：

```
dotnet ef migrations add ReplaceTimeStamp -o Migrations -c AutoLot.Samples.ApplicationDbContext
dotnet ef database update ReplaceTimeStamp  -c AutoLot.Samples.ApplicationDbContext
```

现在你的数据库与模型匹配了。

## 注解和导航属性

ForeignKey 注解让 EF Core 知道哪个属性是导航属性的后备字段。按照约定，<TypeName>Id 会自动设置为外键属性，但在前面的示例中是显式设置的。这支持不同的命名风格，以及同一个表有多个外键的情况。而且（依我愚见）它提高了代码的可读性。

InverseProperty 通过指示其他实体上导航回此实体的导航属性，告知 EF Core 表之间如何关联。当一个实体与另一个实体存在多种关系时，InverseProperty 是必需的，而且（再次声明，依我愚见）它使代码更具可读性。

## Fluent API

Fluent API 通过 C# 代码配置应用程序实体。这些方法由 DbContext 的 OnModelCreating() 方法中可用的 ModelBuilder 实例公开。Fluent API 是最强大的配置方法，并覆盖任何冲突的约定或数据注解。某些配置选项只能使用 Fluent API，例如设置导航属性的默认值和级联行为。

## 类和属性映射

以下代码显示前面的 Car 示例，其中 Fluent API 等同于所使用的数据注解（省略了导航属性，接下来将介绍）。

```
modelBuilder.Entity<Car>(entity =>
{
    entity.ToTable("Inventory", "dbo");
    entity.HasKey(e=>e.Id);
    entity.HasIndex(e => e.MakeId, "IX_Inventory_MakeId");
    entity.Property(e => e.Color)
        .IsRequired()
        .HasMaxLength(50);
    entity.Property(e => e.PetName)
        .IsRequired()
        .HasMaxLength(50);
}
```



```
entity.Property(e => e.TimeStamp)
    .IsRowVersion()
    .IsConcurrencyToken();
});
```

如果现在创建并运行迁移，你会发现自 Fluent API 中的命令与约定和数据注释定义的当前配置一致以来，没有任何变化。

## 默认值

Fluent API 提供了用于设置列默认值的方法。默认值可以是值类型，也可以是 SQL 字符串。例如，要将新 Car 的默认 Color 设置为 Black，请使用以下代码：

```
modelBuilder.Entity<Car>(entity =>
{
    ...
    entity.Property(e => e.Color)
        .HasColumnName("CarColor")
        .IsRequired()
        .HasMaxLength(50)
        .HasDefaultValue("Black");
});
```

若要将值设置为数据库函数（例如 getdate()）的结果，请使用 HasDefaultValueSql() 方法。假设已向 Car 类添加了一个名为 DateBuilt 的 DateTime 属性，并且希望使用 SQL Server 的 getdate() 方法将默认值设置为当前日期。列的配置如下：

```
modelBuilder.Entity<Car>(entity =>
{
    ...
    entity.Property(e => e.DateBuilt)
        .HasDefaultValueSql("getdate()");
});
```

与使用 SQL 插入记录一样，如果映射到具有默认值的列的属性在 EF Core 插入记录时有值，则会使用该属性值，而不是默认值。如果属性值为 null，则使用列的默认值。

当属性的数据类型本身具有默认值时，会出现一个问题。回想一下，数字的默认值为零，而 Boolean 的默认值为 false。如果将数值属性设置为零，或将 Boolean 属性设置为 false，然后插入该实体，EF Core 会将该属性视为未设置值。如果该属性映射到具有默认值的列，则会使用列定义中的默认值。

例如，向 Car 类添加一个名为 IsDrivable 的 bool 属性，并将该属性对应列的默认值设置为 true。

```
//Car.cs
public class Car : BaseEntity
{
    ...
    public bool IsDrivable { get; set; }
}
```

```
//ApplicationDbContext
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Car>(entity =>
    {
        ...
        entity.Property(e => e.IsDrivable).HasDefaultValue(true);
    });
```

当你保存一条 IsDrivable = false 的新记录时，该值会被忽略（因为它是 Boolean 的默认值），数据库默认值将被使用。这意味着 IsDrivable 的值将始终为 true！一种解决方案是将公共属性（以及对应的列）设置为可空，但这可能不符合业务需求。

另一个解决方案由 EF Core 及其对后备字段的支持提供。回想一下，如果存在后备字段（并且通过约定、数据注释或 Fluent API 将其标识为该属性的后备字段），那么 EF Core 将在执行读写操作时使用后备字段，而不是公共属性。

如果将 IsDrivable 更新为使用可空后备字段（但保持属性不可空），EF Core 将从后备字段而不是属性中进行读写。可空 Boolean 的默认值是 null，而不是 false。此更改可以使该属性按预期工作。

```
public class Car
{
    ...
    private bool? _isDrivable;
    public bool IsDrivable
    {
        get => _isDrivable ?? true;
        set => _isDrivable = value;
    }
```

使用 Fluent API 告知 EF Core 该后备字段。

```
modelBuilder.Entity<Car>(entity =>
{
    entity.Property(p => p.IsDrivable)
        .HasField("_isDrivable")
        .HasDefaultValue(true);
});
```

> 注意：在此示例中，HasField() 方法并不是必需的，因为后备字段的名称遵循命名约定。我将其包含在示例中，是为了展示如何使用 Fluent API 设置后备字段。

EF Core 会将该字段转换为以下 SQL 定义：

```
CREATE TABLE [dbo].[Inventory](
    ...
    [IsDrivable] [BIT] NOT NULL,
    ...
GO
```

```
ALTER TABLE [dbo].[Inventory] ADD DEFAULT (CONVERT([BIT],(1))) FOR [IsDrivable]
GO
```

## 计算列

根据数据存储的功能，还可以将列设置为计算列。对于 SQL Server，有两种计算方式：根据同一条记录中其他字段的值计算，或者使用标量函数。例如，要在 Inventory 表中创建一个计算列，将 PetName 和 Color 的值组合起来生成 DisplayName，可以使用 HasComputedColumnSql() 函数。

```
modelBuilder.Entity<Car>(entity =>
{
    entity.Property(p => p.FullName)
        .HasComputedColumnSql("[PetName] + ' (' + [Color] + ')'");
});
```

EF Core 5 新增了持久化计算值的功能，因此只有在创建或更新行时才会计算该值。虽然 SQL Server 支持此功能，但并非所有数据存储都支持，因此请查阅数据库提供程序的文档。

```
modelBuilder.Entity<Car>(entity =>
{
    entity.Property(p => p.FullName)
        .HasComputedColumnSql("[PetName] + ' (' + [Color] + ')'", stored:true);
});
```

## 一对多关系

要使用 Fluent API 定义一对多关系，请选择其中一个实体进行配置。导航链的两端都在同一段代码中设置。

```
modelBuilder.Entity<Car>(entity =>
{
    ...
    entity.HasOne(d => d.MakeNavigation)
        .WithMany(p => p.Cars)
        .HasForeignKey(d => d.MakeId)
        .OnDelete(DeleteBehavior.ClientSetNull)
        .HasConstraintName("FK_Inventory_Makes_MakeId");
});
```

如果选择主体实体作为导航属性配置的基础，代码如下：

```
modelBuilder.Entity<Make>(entity =>
{
    ...
    entity.HasMany(e=>e.Cars)
        .WithOne(c=>c.MakeNavigation)
        .HasForeignKey(c=>c.MakeId)
```

## 一对一关系

一对一关系的配置方式相同，只是使用 WithOne() Fluent API 方法，而不是 WithMany()。依赖实体上会添加唯一索引。以下是使用依赖实体（Radio）配置 Car 和 Radio 实体之间关系的代码：

```
modelBuilder.Entity<Radio>(entity =>
{
    entity.HasIndex(e => e.CarId, "IX_Radios_CarId")
        .IsUnique();

    entity.HasOne(d => d.CarNavigation)
        .WithOne(p => p.RadioNavigation)
        .HasForeignKey<Radio>(d => d.CarId);
});
```

如果在主体实体上定义该关系，依赖实体上仍会添加唯一索引。以下是使用主体实体配置 Car 和 Radio 实体之间关系的代码：

```
modelBuilder.Entity<Radio>(entity =>
{
    entity.HasIndex(e => e.CarId, "IX_Radios_CarId")
        .IsUnique();
});
```

```
modelBuilder.Entity<Car>(entity =>
{
    entity.HasOne(d => d.RadioNavigation)
        .WithOne(p => p.CarNavigation)
        .HasForeignKey<Radio>(d => d.CarId);
});
```

## 多对多关系

使用 Fluent API 可以更加灵活地配置多对多关系。外键字段名称、索引名称和级联行为都可以在定义关系的语句中设置。以下是使用 Fluent API 重现前面多对多关系的示例（对键和列名称进行了修改，使其更易读）：

```
modelBuilder.Entity<Car>()
    .HasMany(p => p.Drivers)
    .WithMany(p => p.Cars)
    .UsingEntity<Dictionary<string, object>>(
    "CarDriver",
    j => j
```

```
.HasOne<Driver>()
.WithMany()
.HasForeignKey("DriverId")
.HasConstraintName("FK_CarDriver_Drivers_DriverId")
.OnDelete(DeleteBehavior.Cascade),
j => j
.HasOne<Car>()
.WithMany()
.HasForeignKey("CarId")
.HasConstraintName("FK_CarDriver_Cars_CarId")
.OnDelete(DeleteBehavior.ClientCascade);
```

## 约定、注释和 Fluent API，真是让人眼花缭乱！

在本章此处，你可能会想知道应该使用这三种选项中的哪一种来配置实体、实体之间的关系以及实体与数据存储之间的关系。答案是：三者都可以使用。约定始终处于启用状态（除非通过数据注释或 Fluent API 覆盖它们）。数据注释几乎可以完成 Fluent API 方法能够完成的所有操作，并且会将信息保存在实体类本身中，这可以提高代码的可读性和可维护性。Fluent API 是三者中功能最强大的，但相关代码隐藏在 DbContext 类中。无论使用数据注释还是 Fluent API，都需要知道：数据注释会覆盖内置约定，而 Fluent API 的方法会覆盖其他所有配置。

## 查询执行

数据检索查询通过针对 DbSet<T> 属性编写 LINQ 查询来创建。数据库提供程序的 LINQ 转换引擎会将 LINQ 查询转换为特定于数据库的语言（例如 T-SQL），然后在服务器端执行。多记录（或可能返回多条记录）的 LINQ 查询不会立即执行，只有在遍历查询（例如使用 foreach）或将其绑定到用于显示的控件（例如数据网格）时才会执行。这种延迟执行机制允许在代码中逐步构建查询，而不会因频繁与数据库通信而产生性能问题。

例如，要从数据库中获取所有黄色的 Car 记录，请执行以下查询：

```
var cars = Context.Cars.Where(x=>x.Color == "Yellow");
```

采用延迟执行时，在遍历结果之前，实际上不会查询数据库。若要立即执行查询，请使用 ToList()。

```
var cars = Context.Cars.Where(x=>x.Color == "Yellow").ToList();
```

由于查询只有在被触发时才会执行，因此可以通过多行代码逐步构建查询。下面的代码示例与前一个示例执行的结果相同：

```
var query = Context.Cars.AsQueryable();
query = query.Where(x=>x.Color == "Yellow");
var cars = query.ToList();
```

单记录查询（例如使用 First()/FirstOrDefault() 时）会在调用相应操作（例如 FirstOrDefault()）时立即执行；而创建、更新和删除语句会在执行 DbContext.SaveChanges() 方法时立即执行。





## 混合客户端-服务器评估

EF Core 的早期版本引入了混合服务器端和客户端执行的功能。这意味着可以在 LINQ 语句中间使用 C# 函数，从而基本抵消我在上一段中所描述的内容。C# 函数之前的部分会在服务器端执行，但随后该时刻查询产生的所有结果都会被带回客户端，然后查询的其余部分会作为 LINQ to Objects 执行。这最终带来的问题多于它解决的问题，因此随着 EF Core 3.1 的发布，此功能发生了改变。现在，只有 LINQ 语句的最终节点可以在客户端执行。

## 跟踪查询与非跟踪查询

当数据从数据库读取到 `DbSet<T>` 实例中时，实体默认会由更改跟踪器进行跟踪。这通常正是应用程序所需要的。一旦某个实例被更改跟踪器跟踪，后续针对同一项数据（基于主键）向数据库发出的调用将导致更新该项，而不会创建重复项。

不过，有时你可能需要从数据库获取一些数据，但不希望这些数据被更改跟踪器跟踪。原因可能是性能问题（对于大量记录跟踪原始值和当前值可能会增加内存压力），或者你明确知道需要这些数据的应用程序部分永远不会修改这些记录。

若要将数据加载到 `DbSet<T>` 实例中，同时不将数据添加到 `ChangeTracker`，请在 LINQ 语句中加入 `AsNoTracking()`。这会通知 EF Core 获取数据，但不要将其添加到 `ChangeTracker` 中。例如，若要加载一条 Car 记录，同时不将其添加到 `ChangeTracker`，请执行以下代码：

```csharp
public virtual Car? FindAsNoTracking(int id)
    => Table.AsNoTracking().FirstOrDefault(x => x.Id == id);
```

这样做的好处是不会增加潜在的内存压力，但缺点是：后续获取同一辆 Car 的调用会创建该记录的额外副本。虽然需要使用更多内存，并且执行时间会略有增加，但可以修改查询，以确保未映射的 Car 只有一个实例。

```csharp
public virtual Car? FindAsNoTracking(int id)
    => Table.AsNoTrackingWithIdentityResolution().FirstOrDefault(x => x.Id == id);
```

## 值得注意的 EF Core 功能

EF Core 复制了 EF 6 中的许多功能，并且每个版本都会增加更多功能。其中许多功能在 EF Core 中都得到了大幅改进，无论是功能性还是性能方面。除了复制 EF 6 中的功能之外，EF Core 还包含许多上一版本中没有的新功能。以下是 EF Core 中一些较为重要的功能（不分先后顺序）。

> **注意：**本节中的代码示例直接取自已完成的 AutoLot 数据访问库，你将在下一章构建该库。

## 处理数据库生成的值

除了更改跟踪以及根据 LINQ 生成 SQL 查询之外，相较于直接使用 ADO.NET，使用 EF Core 的一个显著优势是可以无缝处理数据库生成的值。添加或更新实体后，EF Core 会查询数据库生成的数据，并自动使用正确的值更新实体。在原始 ADO.NET 中，则需要自行完成这些操作。

例如，Inventory 表有一个整数主键，该主键在 SQL Server 中定义为 Identity 列。添加记录时，Identity 列由 SQL Server 使用序列中的唯一编号填充；在正常更新期间不允许更新该列（启用 Identity insert 的特殊情况除外）。此外，Inventory 表还有一个用于并发检查的 Timestamp 列。下一节将介绍并发检查，但现在只需知道，Timestamp 列由 SQL Server 维护，并且会在添加或编辑操作时更新。

以向 Inventory 表添加一辆新的 Car 为例。以下代码创建一个新的 Car 实例，将其添加到派生 `DbContext` 上的 `DbSet<Car>` 实例中，然后调用 `SaveChanges()` 持久化数据：

```csharp
var car = new Car
{
    Color = "Yellow",
    MakeId = 1,
    PetName = "Herbie"
};
Context.Cars.Add(car);
Context.SaveChanges();
```

执行 `SaveChanges` 时，新记录会插入表中，然后 Id 和 Timestamp 值会从表返回到 EF Core，实体的相应属性也会随之更新。

```sql
INSERT INTO [dbo].[Inventory] ([Color], [MakeId], [PetName])
VALUES (N'Yellow', 1, N'Herbie');
SELECT [Id], [TimeStamp]
FROM [dbo].[Inventory]
WHERE @@ROWCOUNT = 1 AND [Id] = scope_identity();
```

> **注意：**EF Core 实际执行的是参数化查询，但为了便于阅读，我简化了所有示例。

向数据库添加多个项时同样有效。EF Core 知道如何将这些值关联到正确的实体。更新记录时，主键值已经已知，因此在我们的 Car 示例中，只会查询并返回更新后的 Timestamp 值。

## 并发检查

当两个独立的进程（用户或系统）大致同时尝试更新同一条记录时，就会出现并发问题。例如，用户 1 和用户 2 都获取了客户 A 的数据。用户 1 更新地址并保存更改。用户 2 更新信用评级，并尝试保存同一条记录。如果用户 2 的保存操作成功，由于地址是在用户 2 获取记录之后被更改的，用户 1 的更改将会被还原。另一种选择是让用户 2 的保存操作失败；这样，用户 1 的更改会被持久化，但用户 2 的更改不会被保存。

如何处理这种情况取决于应用程序的需求。解决方案范围从什么都不做（第二次更新覆盖第一次更新），到使用乐观并发（第二次更新失败），再到更复杂的解决方案，例如检查各个字段。除了选择什么都不做（这被普遍认为是不良的编程实践）之外，开发人员需要知道并发问题何时发生，以便采取适当的处理方式。

幸运的是，许多现代数据库都提供了帮助开发团队处理并发问题的工具。SQL Server 有一种名为 `timestamp` 的内置数据类型，它是 `rowversion` 的同义词。如果某列定义为 `timestamp` 数据类型，那么添加记录到数据库时，该列的值由 SQL Server 创建；更新记录时，该列的值也会随之更新。该值几乎可以保证是唯一的，并且由 SQL Server 控制。

EF Core 可以通过在实体上实现 `Timestamp` 属性来利用 SQL Server 的 `timestamp` 数据类型（在 C# 中表示为 `byte[]`）。使用 `Timestamp` 特性或 Fluent API 配置定义的实体属性，在更新或删除记录时会被添加到 `WHERE` 子句中。生成的 SQL 不再仅使用主键值，而是将 Timestamp 属性的值也添加到 `WHERE` 子句中。这样，结果就会被限制为主键值和 timestamp 值都匹配的记录。如果另一位用户（或系统）已经更新了该记录，timestamp 值就不会匹配，此时 `UPDATE` 或 `DELETE` 语句不会更新该记录。以下是一个使用 Timestamp 列的更新查询示例：

```sql
UPDATE [dbo].[Inventory] SET [Color] = N'Yellow'
WHERE [Id] = 1 AND [TimeStamp] = 0x000000000000081F;
```

当数据存储报告的受影响记录数与 `ChangeTracker` 预期更改的记录数不同时，EF Core 会抛出 `DbUpdateConcurrencyException`，并回滚整个事务。`DbUpdateConcurrencyException` 包含所有未能持久化的记录的信息，包括原始值（实体从数据库加载时的值）和当前值（用户或系统更新后的值）。此外，还有一种方法可以获取当前数据库值（这需要再次调用服务器）。借助这些丰富的信息，开发人员就可以根据应用程序的要求处理并发错误。以下代码展示了这一过程：

```csharp
try
{
    // 获取一条 Car 记录（哪一条都可以）
    var car = Context.Cars.First();
    // 在上下文之外更新数据库
    Context.Database.ExecuteSqlInterpolated($"Update dbo.Inventory set Color='Pink' where Id = {car.Id}");
    // 更新更改跟踪器中的 Car 记录，然后尝试保存更改
    car.Color = "Yellow";
    Context.SaveChanges();
}
catch (DbUpdateConcurrencyException ex)
{
    // 获取更新失败的实体
    var entry = ex.Entries[0];
    // 获取原始值（实体加载时的值）
    PropertyValues originalProps = entry.OriginalValues;
    // 获取当前值（由此代码路径更新后的值）
    PropertyValues currentProps = entry.CurrentValues;
    // 从数据存储中获取当前值
    // 注意：这需要再次调用数据库
    // PropertyValues databaseProps = entry.GetDatabaseValues();
}
```





## 连接弹性

瞬时错误很难调试，更难以重现。幸运的是，许多数据库提供程序都内置了针对数据库系统故障（如 tempdb 问题、用户限制等）的重试机制，EF Core 可以利用这些机制。对于 SQL Server，SqlServerRetryingExecutionStrategy 会捕获瞬时错误（由 SQL Server 团队定义），如果通过 DbContextOptions 在派生的 DbContext 上启用，EF Core 会自动重试该操作，直到达到最大重试限制。

对于 SQL Server，有一个快捷方法可用于在默认设置下启用 SqlServerRetryingExecutionStrategy。与 SqlServerOptions 一起使用的方法是 EnableRetryOnFailure()，如下所示：

```csharp
public ApplicationDbContext CreateDbContext(string[] args)
{
    var optionsBuilder = new DbContextOptionsBuilder<ApplicationDbContext>();
    var connectionString = @"server=.,5433;Database=AutoLot50;User Id=sa;Password=P@ssw0rd;";
    optionsBuilder.UseSqlServer(connectionString, options => options.EnableRetryOnFailure());
    return new ApplicationDbContext(optionsBuilder.Options);
}
```

最大重试次数和重试之间的时间限制可以根据应用程序的需求进行配置。如果在操作未完成的情况下达到重试限制，EF Core 将通过抛出 RetryLimitExceededException 来通知应用程序连接问题。开发人员处理此异常时，可以将相关信息传递给用户，从而提供更好的体验。

```csharp
try
{
    Context.SaveChanges();
}
catch (RetryLimitExceededException ex)
{
    //A retry limit error occurred
    //Should handle intelligently
    Console.WriteLine($"Retry limit exceeded! {ex.Message}");
}
```

对于没有提供内置执行策略的数据库提供程序，也可以创建自定义执行策略。有关更多信息，请参阅 EF Core 文档：https://docs.microsoft.com/en-us/ef/core/miscellaneous/connection-resiliency。

## 关联数据

实体导航属性用于加载实体的关联数据。关联数据可以预先加载（一个 LINQ 语句，一个 SQL 查询）、通过拆分查询预先加载（一个 LINQ 语句，多个 SQL 查询）、显式加载（多个 LINQ 调用，多个 SQL 查询）或延迟加载（一个 LINQ 语句，多个按需 SQL 查询）。

除了可以使用导航属性加载关联数据之外，EF Core 还会在实体加载到更改跟踪器时自动修复实体。例如，假设所有 Make 记录都已加载到 DbSet<Make> 中。接下来，所有 Car 记录都已加载到 DbSet<Car> 中。即使这些记录是分别加载的，它们也可以通过导航属性相互访问。

## 预先加载

*预先加载*是指在一个数据库调用中从多个表加载关联记录的术语。这类似于在 T-SQL 中创建使用连接将两个或多个表关联起来的查询。当实体具有导航属性，并且这些属性在 LINQ 查询中使用时，翻译引擎会使用连接从相关表中获取数据，并加载相应的实体。这通常比先执行一个查询从一张表获取数据，然后再为每个相关表执行额外查询要高效得多。当使用单个查询效率较低时，EF Core 5 引入了查询拆分，接下来会介绍。

Include() 和 ThenInclude()（用于后续导航属性）方法用于在 LINQ 查询中遍历导航属性。如果关系是必需的，LINQ 翻译引擎将创建内连接。如果关系是可选的，翻译引擎将创建左连接。

例如，要加载所有 Car 记录及其相关的 Make 信息，请执行以下 LINQ 查询：

```csharp
var queryable = Context.Cars.IgnoreQueryFilters().Include(c => c.MakeNavigation).ToList();
```

上述 LINQ 会对数据库执行以下查询：

```sql
SELECT [i].[Id], [i].[Color], [i].[MakeId], [i].[PetName], [i].[TimeStamp],
    [m].[Id], [m].[Name], [m].[TimeStamp]
FROM [dbo].[Inventory] AS [i]
INNER JOIN [dbo].[Makes] AS [m] ON [i].[MakeId] = [m].[Id]
```

在同一个查询中可以使用多个 Include() 语句，将多个实体连接到原始实体。要遍历导航属性树，请在 Include() 之后使用 ThenInclude()。例如，要获取所有 Cars 记录及其相关的 Make 和 Order 信息，以及与 Order 相关的 Customer 信息，请使用以下语句：

```csharp
var cars = Context.Cars.Where(c => c.Orders.Any())
    .Include(c => c.MakeNavigation)
    .Include(c => c.Orders).ThenInclude(o => o.CustomerNavigation).ToList();
```

## 筛选 Include

EF Core 5 中的新功能，可以对包含的数据进行筛选和排序。集合导航上允许的操作有 Where()、OrderBy()、OrderByDescending()、ThenBy()、ThenByDescending()、Skip() 和 Take()。例如，如果要获取所有 Make 记录，但只获取颜色为黄色的相关 Car 记录，可以在 lambda 表达式中筛选导航属性，如下所示：

```csharp
var query = Context.Makes
    .Include(x => x.Cars.Where(x => x.Color == "Yellow")).ToList();
```

执行的查询如下：

```sql
SELECT [m].[Id], [m].[Name], [m].[TimeStamp], [t].[Id], [t].[Color],
    [t].[MakeId], [t].[PetName], [t].[TimeStamp]
FROM [dbo].[Makes] AS [m]
LEFT JOIN (
    SELECT [i].[Id], [i].[Color], [i].[MakeId], [i].[PetName], [i].[TimeStamp]
    FROM [dbo].[Inventory] AS [i]
    WHERE [i].[Color] = N'Yellow' ) AS [t] ON [m].[Id] = [t].[MakeId]
ORDER BY [m].[Id], [t].[Id]
```

## 使用拆分查询进行预先加载

当 LINQ 查询包含大量 Include 时，可能会对性能产生负面影响。为了解决这种情况，EF Core 5 引入了拆分查询。EF Core 不会执行单个查询，而是将 LINQ 查询拆分为多个 SQL 查询，然后关联所有相关数据。例如，可以通过在 LINQ 查询中添加 AsSplitQuery() 来将之前的查询按多个 SQL 查询执行，如下所示：

```csharp
var query = Context.Makes.AsSplitQuery()
    .Include(x => x.Cars.Where(x => x.Color == "Yellow")).ToList();
```

执行的查询如下所示：

```sql
SELECT [m].[Id], [m].[Name], [m].[TimeStamp]
FROM [dbo].[Makes] AS [m]
ORDER BY [m].[Id]
```

```sql
SELECT [t].[Id], [t].[Color], [t].[MakeId], [t].[PetName], [t].[TimeStamp], [m].[Id]
FROM [dbo].[Makes] AS [m]
INNER JOIN (
    SELECT [i].[Id], [i].[Color], [i].[MakeId], [i].[PetName], [i].[TimeStamp]
    FROM [dbo].[Inventory] AS [i]
    WHERE [i].[Color] = N'Yellow'
) AS [t] ON [m].[Id] = [t].[MakeId]
ORDER BY [m].[Id]
```

使用拆分查询有一个缺点：如果在执行各查询期间数据发生了变化，则返回的数据将不一致。

## 显式加载

显式加载是在核心对象已经加载后，沿着导航属性加载数据。此过程涉及执行额外的数据库调用来获取相关数据。如果应用程序需要根据某些用户操作有选择地获取相关记录，而不是拉取所有相关记录，这将非常有用。

该过程从一个已加载的实体开始，并在派生的 DbContext 上使用 Entry() 方法。当查询引用导航属性（例如，获取汽车的 Make 信息）时，请使用 Reference() 方法。当查询集合导航属性时，请使用 Collection() 方法。查询会延迟到执行 Load()、ToList() 或聚合函数（如 Count()、Max()）时才执行。

以下示例演示如何获取 Car 记录的相关 Make 数据以及任何 Orders：

```csharp
//Get the Car record
var car = Context.Cars.First(x => x.Id == 1);
//Get the Make information
Context.Entry(car).Reference(c => c.MakeNavigation).Load();
//Get any orders the Car is related to
Context.Entry(car).Collection(c => c.Orders).Query().IgnoreQueryFilters().Load();
```





## 延迟加载

延迟加载是指在导航属性被用来访问尚未加载到内存中的相关记录时，按需加载记录。延迟加载是 EF 6 的一项功能，在 EF Core 2.1 版本中被重新加入。虽然听起来开启它是个好主意，但启用延迟加载会通过进行可能不必要的数据库往返而在应用程序中造成性能问题。出于这个原因，延迟加载在 EF Core 中默认是关闭的（在 EF 6 中默认是启用的）。

延迟加载在智能客户端（WPF、WinForms）应用程序中可能很有用，但在 Web 或服务应用程序中不建议使用。出于这个原因，我在本文中不涉及延迟加载。如果你想了解有关延迟加载的更多信息以及如何将其与 EF Core 一起使用，请查阅此处的文档：https://docs.microsoft.com/en-us/ef/core/querying/related-data/lazy。

## 全局查询过滤器

全局查询过滤器允许为特定实体添加 where 子句到所有 LINQ 查询中。例如，一个常见的数据库设计模式是使用软删除而不是硬删除。向表中添加一个字段来指示记录的删除状态。如果记录被“删除”，则值设置为 true（或 1），但不会从数据库中移除。这称为软删除。为了从正常操作中过滤掉软删除的记录，每个 where 子句都必须检查此字段的值。记住在每个查询中包含此过滤器可能耗时，甚至会造成问题。

EF Core 允许向实体添加全局查询过滤器，然后将其应用于涉及该实体的每个查询。对于前面描述的软删除示例，你在实体类上设置一个过滤器以排除软删除的记录。EF Core 创建的任何涉及具有全局查询过滤器的实体的查询都将应用其过滤器。不再需要在每个查询中记住包含 where 子句。

沿用本书的汽车主题，假设所有不可行驶的 Car 记录都应从正常查询中过滤掉。使用 Fluent API，你可以像这样添加全局查询过滤器：

```
modelBuilder.Entity<Car>(entity =>
{
    entity.HasQueryFilter(c => c.IsDrivable == true);
    entity.Property(p => p.IsDrivable).HasField("_isDrivable").HasDefaultValue(true);
});
```

设置全局查询过滤器后，涉及 Car 实体的查询将自动过滤掉不可行驶的汽车。例如，执行此 LINQ 查询：

```
var cars = Context.Cars.ToList();
```

执行以下 SQL：

```
SELECT [i].[Id], [i].[Color], [i].[IsDrivable], [i].[MakeId], [i].[PetName], [i].[TimeStamp]
FROM [Dbo].[Inventory] AS [i]
WHERE [i].[IsDrivable] = CAST(1 AS bit)
```

如果需要查看被过滤的记录，请在 LINQ 查询中添加 IgnoreQueryFilters()，这将禁用 LINQ 查询中每个实体的全局查询过滤器。执行此 LINQ 查询：

```
var cars = Context.Cars.IgnoreQueryFilters().ToList();
```

执行以下 SQL：

```
SELECT [i].[Id], [i].[Color], [i].[IsDrivable], [i].[MakeId], [i].[PetName], [i].[TimeStamp]
FROM [Dbo].[Inventory] AS [i]
```

需要注意的是，调用 IgnoreQueryFilters() 会删除 LINQ 查询中每个实体的查询过滤器，包括参与 Include() 或 ThenInclude() 语句的实体。

### 导航属性上的全局查询过滤器

全局查询过滤器也可以设置在导航属性上。假设你想过滤掉包含不可行驶汽车的任何订单。查询过滤器创建在 Order 实体的 CarNavigation 导航属性上，如下所示：

```
modelBuilder.Entity<Order>().HasQueryFilter(e => e.CarNavigation.IsDrivable);
```

执行标准 LINQ 查询时，包含不可行驶汽车的任何订单都将被排除在结果之外。以下是 LINQ 语句和生成的 SQL 语句：

```
//C# Code
var orders = Context.Orders.ToList();

/* Generated SQL query */
SELECT [o].[Id], [o].[CarId], [o].[CustomerId], [o].[TimeStamp]
FROM [Dbo].[Orders] AS [o]
INNER JOIN (SELECT [i].[Id], [i].[IsDrivable]
    FROM [Dbo].[Inventory] AS [i]
    WHERE [i].[IsDrivable] = CAST(1 AS bit)) AS [t]
    ON [o].[CarId] = [t].[Id]
WHERE [t].[IsDrivable] = CAST(1 AS bit)
```

要移除查询过滤器，请使用 IgnoreQueryFilters()。以下是更新后的 LINQ 语句和随后生成的 SQL：

```
//C# Code
var orders = Context.Orders.IgnoreQueryFilters().ToList();

/* Generated SQL query */
SELECT [o].[Id], [o].[CarId], [o].[CustomerId], [o].[TimeStamp]
FROM [Dbo].[Orders] AS [o]
```

在此处需要谨慎：EF Core 不会检测循环全局查询过滤器，因此在向导航属性添加查询过滤器时要小心。

### 使用全局查询过滤器的显式加载

全局查询过滤器在显式加载相关数据时也生效。例如，如果你想为 Make 加载 Car 记录，IsDrivable 过滤器将防止不可行驶的汽车被加载到内存中。以以下代码片段为例：

```
var make = Context.Makes.First(x => x.Id == makeId);
Context.Entry(make).Collection(c=>c.Cars).Load();
```

到现在为止，生成的 SQL 查询包含不可行驶汽车的过滤器应该不会让人感到意外。

```
SELECT [i].[Id], [i].[Color], [i].[IsDrivable],
    [i].[MakeId], [i].[PetName], [i].[TimeStamp]
FROM [dbo].[Inventory] AS [i]
WHERE ([i].[IsDrivable] = CAST(1 AS bit)) AND ([i].[MakeId] = 1
```

在显式加载数据时忽略查询过滤器有一个小问题。Collection() 方法返回的类型是 CollectionEntry<Make, Car>，并且没有显式实现 IQueryable<T> 接口。要调用 IgnoreQueryFilters()，你必须先调用 Query()，它返回一个 IQueryable<Car>。

```
var make = Context.Makes.First(x => x.Id == makeId);
Context.Entry(make).Collection(c=>c.Cars).Query().IgnoreQueryFilters().Load();
```

使用 Reference() 方法从引用导航属性检索数据时适用相同的过程。

### 使用 LINQ 的原始 SQL 查询

有时为复杂查询获取正确的 LINQ 语句比直接编写 SQL 更困难。幸运的是，EF Core 提供了一种机制，允许在 DbSet<T> 上执行原始 SQL 语句。FromSqlRaw() 和 FromSqlRawInterpolated() 方法接收一个字符串作为 LINQ 查询的基础。此查询在服务器端执行。

如果原始 SQL 语句是非终止的（例如，既不是存储过程也不是用户定义函数，也不是使用公用表表达式或以分号结尾的语句），则可以向查询添加额外的 LINQ 语句。诸如 Include()、OrderBy() 或 Where() 等额外的 LINQ 语句将与原始的原始 SQL 调用和任何全局查询过滤器组合，并且整个查询在服务器端执行。

使用 FromSql 变体之一时，查询必须使用数据存储架构和表名编写，而不是实体名称。FromSqlRaw() 会原样发送字符串。FromSqlInterpolated() 使用 C# 字符串插值，每个插值字符串都会在 SQL 参数中进行转换。每当你使用变量时，都应使用插值版本以获得参数化查询固有的额外保护。

假设全局查询过滤器已设置在 Car 实体上，以下 LINQ 语句将获取 Id 为一的第一条库存记录，包含相关的 Make 数据，并过滤掉不可行驶的汽车：

```
var car = Context.Cars
    .FromSqlInterpolated($"Select * from dbo.Inventory where Id = {carId}")
    .Include(x => x.MakeNavigation)
    .First();
```

LINQ 到 SQL 翻译引擎将原始 SQL 语句与剩余的 LINQ 语句组合并执行以下查询：

```
SELECT TOP(1) [c].[Id], [c].[Color], [c].[IsDrivable], [c].[MakeId],
    [c].[PetName], [c].[TimeStamp],
    [m].[Id], [m].[Name], [m].[TimeStamp]
FROM (Select * from dbo.Inventory where Id = 1) AS [c]
INNER JOIN [dbo].[Makes] AS [m] ON [c].[MakeId] = [m].[Id]
WHERE [c].[IsDrivable] = CAST(1 AS bit)
```





知道在使用 LINQ 时使用原始 SQL 时必须遵守几条规则。

- SQL 查询必须为实体类型的每个属性返回数据。
- 列名必须与它们映射到的属性相匹配（相对于 EF 6 的改进，其中映射被忽略）。
- SQL 查询不能包含相关数据。

## 语句批处理

EF Core 通过将语句执行为一个或多个批来显著提高了保存数据库更改时的性能。这减少了应用程序与数据库之间的往返次数，提高了性能，并可能降低成本（例如，对于按事务计费的云数据库）。

EF Core 使用表值参数批处理创建、更新和删除语句。EF 批处理的语句数量取决于数据库提供程序。例如，对于 SQL Server，批处理在语句少于 4 条和多于 40 条时效率低下。无论批次数多少，所有语句仍在事务中执行。批大小也可以通过 DbContextOptions 配置，但建议在大多数（如果不是全部）情况下让 EF Core 计算批大小。

如果你在一个事务中像这样插入四辆汽车：

```
var cars = new List<Car>
{
    new Car { Color = "Yellow", MakeId = 1, PetName = "Herbie" },
    new Car { Color = "White", MakeId = 2, PetName = "Mach 5" },
    new Car { Color = "Pink", MakeId = 3, PetName = "Avon" },
    new Car { Color = "Blue", MakeId = 4, PetName = "Blueberry" },
};
Context.Cars.AddRange(cars);
Context.SaveChanges();
```

EF Core 会将语句批处理为一次调用。生成的查询如下所示：

```
exec sp_executesql N'SET NOCOUNT ON;
DECLARE @inserted0 TABLE ([Id] int, [_Position] [int]);
MERGE [dbo].[Inventory] USING (
VALUES (@p0, @p1, @p2, 0),
(@p3, @p4, @p5, 1),
(@p6, @p7, @p8, 2),
(@p9, @p10, @p11, 3)) AS i ([Color], [MakeId], [PetName], _Position) ON 1=0
WHEN NOT MATCHED THEN
INSERT ([Color], [MakeId], [PetName])
VALUES (i.[Color], i.[MakeId], i.[PetName])
OUTPUT INSERTED.[Id], i._Position
INTO @inserted0;

SELECT [t].[Id], [t].[IsDrivable], [t].[TimeStamp] FROM [dbo].[Inventory] t
INNER JOIN @inserted0 i ON ([t].[Id] = [i].[Id])
ORDER BY [i].[_Position];
```

```
' ,N'@p0 nvarchar(50),@p1 int,@p2 nvarchar(50),@p3 nvarchar(50),@p4 int,@p5 nvarchar(50),@p6 nvarchar(50),@p7 int,@p8 nvarchar(50),@p9 nvarchar(50),@p10 int,@p11 nvarchar(50)',@p0=N'Yellow',@p1=1,@p2=N'Herbie ',@p3=N'White',@p4=2,@p5=N'Mach 5 ',@p6=N'Pink',@p7=3,@p8=N'Avon',@p9=N'Blue',@p10=4,@p11=N'Blueberry'
```

## 所有者实体类型

使用 C# 类作为实体的属性来定义另一个实体的属性集合，最初是在版本 2.0 中引入并不断更新的。当标记有 [Owned] 属性（或使用 Fluent API 配置）的类型被添加为实体的属性时，EF Core 会将 [Owned] 实体类中的所有属性添加到拥有实体中。这增加了 C# 代码重用的可能性。

在幕后，EF Core 将其视为一对一关系。拥有类是主实体，拥有类是主实体。拥有类即使被视为实体，也不能没有拥有实体而存在。来自拥有类型的默认列名将格式化为 NavigationPropertyName_OwnedEntityPropertyName（例如，PersonalNavigation_FirstName）。默认名称可以使用 Fluent API 更改。

请看这个 Person 类（注意 Owned 属性）：

```
[Owned]
public class Person
{
    [Required, StringLength(50)]
    public string FirstName { get; set; } = "New";
    [Required, StringLength(50)]
    public string LastName { get; set; } = "Customer";
}
```

它被 Customer 类使用：

```
[Table("Customers", Schema = "dbo")]
public partial class Customer : BaseEntity
{
    public Person PersonalInformation { get; set; } = new Person();
    [JsonIgnore]
    [InverseProperty(nameof(CreditRisk.CustomerNavigation))]
    public IEnumerable<CreditRisk> CreditRisks { get; set; } = new List<CreditRisk>();
    [JsonIgnore]
    [InverseProperty(nameof(Order.CustomerNavigation))]
    public IEnumerable<Order> Orders { get; set; } = new List<Order>();
}
```

默认情况下，两个 Person 属性映射到名为 PersonalInformation_FirstName 和 PersonalInformation_LastName 的列。要更改此设置，请将以下 Fluent API 代码添加到 OnConfiguring() 方法中：

```
modelBuilder.Entity<Customer>(entity =>
{
    entity.OwnsOne(o => o.PersonalInformation,
        pd =>
        {

```

```
pd.Property<string>(nameof(Person.FirstName))
    .HasColumnName(nameof(Person.FirstName))
    .HasColumnType("nvarchar(50)");
pd.Property<string>(nameof(Person.LastName))
    .HasColumnName(nameof(Person.LastName))
    .HasColumnType("nvarchar(50)");
});
});
```

生成的表创建如下（请注意，FirstName 和 LastName 列的可空性与 Person 拥有实体上的数据注释不匹配）：

```
CREATE TABLE [dbo].[Customers](
    [Id] [int] IDENTITY(1,1) NOT NULL,
    [FirstName] [nvarchar](50) NULL,
    [LastName] [nvarchar](50) NULL,
    [TimeStamp] [timestamp] NULL,
    [FullName] AS ([LastName]+', '+[FirstName]),
    CONSTRAINT [PK_Customers] PRIMARY KEY CLUSTERED
    (
        [Id] ASC
    )WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON, OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF) ON [PRIMARY]
) ON [PRIMARY]
GO
```

EF Core 5 修复了拥有实体的问题，该问题可能不会对你造成影响，但可能是一个重大问题。请注意，Person 类在其两个属性上都有 Required 数据注释，但 SQL Server 列都设置为 NULL。这是因为迁移系统在将拥有实体与可选关系一起使用时翻译拥有实体的方式存在问题。修复方法是使关系成为必需的。

要纠正此问题，有几个选项。第一个是启用 C# 可空性（在项目级别或在类中）。这使 PersonalInformation 导航属性变为不可空，EF Core 会尊重这一点，进而 EF Core 会适当地配置拥有实体中的列。另一个选项是添加 Fluent API 语句以使导航属性成为必需的。

```
modelBuilder.Entity<Customer>(entity =>
{
    entity.OwnsOne(o => o.PersonalInformation,
        pd =>
        {
            pd.Property<string>(nameof(Person.FirstName))
                .HasColumnName(nameof(Person.FirstName))
                .HasColumnType("nvarchar(50)");
            pd.Property<string>(nameof(Person.LastName))
                .HasColumnName(nameof(Person.LastName))
                .HasColumnType("nvarchar(50)");
        });
    entity.Navigation(c => c.PersonalInformation).IsRequired(true);
});
```

拥有实体还有其他选项可供探索，包括集合、表拆分和嵌套。这些都超出了本书的范围。要了解更多信息，请在此处查阅 EF Core 关于拥有实体的文档：https://docs.microsoft.com/en-us/ef/core/modeling/owned-entities。

## 数据库函数映射

SQL Server 函数可以映射到 C# 方法并包含在 LINQ 语句中。C# 方法只是一个占位符，因为服务器函数被折叠到查询生成的 SQL 中。表值函数映射的支持已添加到 EF Core 中，已有的标量函数映射支持中。有关数据库函数映射的更多信息，请查阅文档：https://docs.microsoft.com/en-us/ef/core/querying/user-defined-function-mapping。

## EF Core 全局工具 CLI 命令

dotnet-ef 全局 CLI 工具 EF Core 工具包含将现有数据库 scaffolding 为代码、创建/删除数据库迁移以及对数据库进行操作（更新、删除等）所需的命令。在你可以使用 dotnet-ef 全局工具之前，必须使用以下命令安装它（如果你在本书本章前面跟着做，你已经完成了此操作）：

```
dotnet tool install --global dotnet-ef --version 5.0.1
```

> 注意 因为 EF Core 5 不是长期支持版本，要使用 EF Core 5 全局工具，必须指定版本。

要测试安装，请打开命令提示符并输入以下命令：

```
dotnet ef
```

如果工具成功安装，你将获得 EF Core 独角兽（团队的吉祥物）和可用命令列表，如下所示（独角兽在屏幕上看起来更好）：

```
      __/\__
   ---==/   \\
      | . )   |\|
[ ] [ ] | )   ///
[ ] [ ] \_/   //\\
|_ |_| |   /   \\\///

```

```
Entity Framework Core .NET Command-line Tools 5.0.1
```

```
Usage: dotnet ef [options] [command]
```

- Options:
- --version      Show version information
- -h|--help      Show help information
- -v|--verbose   Show verbose output.
- --no-color     Don't colorize output.
- --prefix-output Prefix output with level.





Commands:
  database    用于管理数据库的命令。
  dbcontext   用于管理 DbContext 类型的命令。
  migrations  用于管理迁移的命令。

使用 "dotnet ef [command] --help" 可获取有关命令的更多信息。

表 22-9 描述了 EF Core 全局工具中的三个主要命令。每个主要命令都有额外的子命令。与所有 .NET Core 命令一样，每个命令都有丰富的帮助系统，可通过在命令后输入 -h 来访问。

## Table 22-9. EF Core Tooling Commands

| Command | Meaning in Life |
|---|---|
| Database | 用于管理数据库的命令。子命令包括 drop 和 update。 |
| DbContext | 用于管理 DbContext 类型的命令。子命令包括 scaffold、list 和 info。 |
| Migrations | 用于管理迁移的命令。子命令包括 add、list、remove 和 script。 |

EF Core 命令在 .NET Core 项目文件上执行（而不是解决方案文件）。目标项目需要引用 EF Core 工具 NuGet 包 Microsoft.EntityFrameworkCore.Design。命令在命令运行的同一目录中的项目文件上操作，或通过命令行选项引用的其他目录中的项目文件。

对于需要派生 DbContext 类实例的 EF Core CLI 命令（Database 和 Migrations），如果项目中只有一个，则会使用它。如果有多个，则需要在命令行选项中指定 DbContext。派生的 DbContext 类将使用实现 IDesignTimeDbContextFactory<TContext> 接口的类的实例进行实例化，如果能找到的话。如果工具找不到，则派生的 DbContext 将使用无参数构造函数进行实例化。如果两者都不存在，命令将失败。请注意，无参数构造函数选项需要 OnConfiguring 重写存在，这并不是一种好的做法。最好的（也是唯一）选择是为应用程序中拥有的每个派生 DbContext 都创建一个 IDesignTimeDbContextFactory<TContext>。

EF Core 命令的通用选项在表 22-10 中显示。许多命令还有额外的选项或参数。

## Table 22-10. EF Core Command Options

| Option (Shorthand || Longhand) | Meaning in Life |
|---|---|
| --c || --context <DBCONTEXT> | 要使用的完全限定派生 DbContext 类。如果项目中存在多个派生 DbContext，则此为必填选项。 |
| -p || --project <PROJECT> | 要使用的项目（放置文件的位置）。默认为当前工作目录。 |
| -s || --startup-project <PROJECT> | 要使用的启动项目（包含派生 DbContext）。默认为当前工作目录。 |
| -h || --help | 显示帮助和所有选项。 |
| -v || --verbose | 显示详细输出。 |

要列出命令的所有参数和选项，请在命令窗口中输入 dotnet ef <command> -h，如下所示：

```
dotnet ef migrations add -h
```

> Note 请注意 CLI 命令不是 C# 命令，因此转义斜杠和引号的规则不适用。

## The Migrations Commands

迁移命令用于添加、删除、列出和脚本化迁移。随着迁移被应用到基表，会在 __EFMigrationsHistory 表中创建一条记录。表 22-11 描述了这些命令。以下各节将详细介绍这些命令。

## Table 22-11. EF Core Migrations Commands

| Command | Meaning in Life |
|---|---|
| Add | 基于与上一次迁移的更改创建新的迁移 |
| Remove | 检查项目中最后一次迁移是否已应用到数据库，如果未应用，则删除迁移文件（及其设计器）然后将快照类回滚到上一次迁移 |
| List | 列出派生 DbContext 的所有迁移及其状态（已应用或待处理） |
| Script | 为所有、一个或一系列迁移创建 SQL 脚本 |

## The Add Command

add 命令基于当前对象模型创建新的数据库迁移。该过程检查派生 DbContext 上 DbSet<T> 属性的每个实体（以及可通过导航属性从这些实体到达的每个实体），并确定是否需要对数据库进行任何更改。如果有更改，则会生成适当的代码来更新数据库。你将在稍后了解更多相关信息。

Add 命令需要一个 name 参数，用于为迁移的创建类和文件命名。除了通用选项外，选项 -o <PATH> 或 -output-dir <PATH> 指示迁移文件应放置的位置。默认目录名为 Migrations，相对于当前路径。

每次添加的迁移都会创建两个文件，它们是同一类的部分类。两个文件的名称都以时间戳和作为 add 命令参数使用的迁移名称开头。第一个文件名为 <YYYYMMDDHHMMSS>_<MigrationName>.cs，第二个文件名为 <YYYYMMDDHHMMSS>_<MigrationName>.Designer.cs。时间戳基于文件创建的时间，对于两个文件完全相同。第一个文件代表此迁移中数据库更改生成的代码，设计器文件代表基于截至此迁移为止的所有迁移创建和更新数据库的代码。

主文件包含两个方法，Up() 和 Down()。Up() 方法包含使用此迁移的更改更新数据库的代码，Down() 方法包含回滚此迁移更改的代码。本章前面初始迁移的部分列表（One2Many 迁移）如下所示：

```
public partial class One2Many : Migration
{
    protected override void Up(MigrationBuilder migrationBuilder)
    {
        migrationBuilder.CreateTable(
            name: "Make",
            columns: table => new
            {
                Id = table.Column<int>(type: "int", nullable: false)
                    .Annotation("SqlServer:Identity", "1, 1"),
                Name = table.Column<string>(type: "nvarchar(max)", nullable: true),
                TimeStamp = table.Column<byte[]>(type: "varbinary(max)", nullable: true)
            },
            constraints: table =>
            {
                table.PrimaryKey("PK_Make", x => x.Id);
            });
        ...
        migrationBuilder.CreateIndex(
            name: "IX_Cars_MakeId",
            table: "Cars",
            column: "MakeId");
    }

    protected override void Down(MigrationBuilder migrationBuilder)
    {
        migrationBuilder.DropTable(name: "Cars");
        migrationBuilder.DropTable(name: "Make");
    }
}
```

正如你所见，Up() 方法正在创建表、列、索引等。Down() 方法正在删除创建的项目。迁移引擎会根据需要发出 alter、add 和 drop 语句，以确保数据库与模型匹配。

设计器文件包含两个属性，将这些部分类与文件名和派生 DbContext 绑定。属性如下所示，其中包含设计类部分列表：

```
[DbContext(typeof(ApplicationDbContext))]
[Migration("20201230020509_One2Many")]
partial class One2Many
{
    protected override void BuildTargetModel(ModelBuilder modelBuilder)
    {
    ...
    }
}
```

第一次迁移会在目标目录中创建另一个文件，其名称为派生 DbContext，格式为 <DerivedDbContextName>ModelSnapshot.cs。此文件的格式与设计器部分相同，包含所有迁移的总和代码。当添加或删除迁移时，此文件会自动更新以匹配更改。

Note 极其重要的是不要手动删除迁移文件。这会导致 <DerivedDbContext>ModelSnapshot.cs 与你的迁移不同步，从本质上破坏它们。如果你要手动删除它们，请全部删除并重新开始。要删除迁移，请使用稍后介绍的 remove 命令。

## Excluding Tables from Migrations

如果一个实体在多个 DbContext 之间共享，每个 DbContext 都会在迁移文件中为该实体的任何更改创建代码。这会导致问题，因为第二个迁移脚本如果更改已存在于数据库中将会失败。在 EF Core 5 之前，唯一的解决方案是手动编辑其中一个迁移文件以删除这些更改。

在 EF Core 5 中，DbContext 可以将实体标记为从迁移中排除，让另一个 DbContext 成为该实体的记录系统。以下代码显示了实体被排除在迁移之外的示例：

```
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<LogEntry>().ToTable("Logs", t => t.ExcludeFromMigrations());
}
```

## The Remove Command

remove 命令用于从项目中删除迁移，并且始终操作最后一次迁移（基于迁移的时间戳）。删除迁移时，EF Core 会通过检查数据库中的 __EFMigrationsHistory 表来确保它尚未被应用。如果迁移已被应用，则过程失败。如果迁移尚未应用或已被回滚，则迁移将被删除，并且模型快照文件将被更新。

remove 命令不接受任何参数（因为它始终作用于最后一次迁移），并使用与 add 命令相同的选项。有一个额外的选项，即 force 选项（-f || --force）。这将回滚最后一次迁移然后将其删除，一步完成。

## The List Command

list 命令用于显示派生 DbContext 的所有迁移。默认情况下，它将列出所有迁移并查询数据库以确定它们是否已被应用。如果未应用，它们将被列为待处理。有选项可以传入特定的连接字符串，还有另一个选项可以完全不连接到数据库，而是仅列出迁移。表 22-12 显示了这些选项。

## Table 22-12. Additional Options for EF Core Migrations List Command

| Option (Shorthand || Longhand) | Meaning in Life |
|---|---|
| --connection <CONNECTION> | 数据库的连接字符串。默认为 IDesignTimeDbContextFactory 实例或 DbContext 的 OnConfiguring 方法中指定的连接字符串。 |
| --no-connect | 指示命令跳过数据库检查。 |





## Script 命令

script 命令基于一个或多个迁移创建 SQL 脚本。该命令有两个可选参数，分别表示起始迁移和结束迁移。如果两者均未指定，则会为所有迁移生成脚本。表 22-13 描述了这些参数。

### 表 22-13. EF Core 迁移 Script 命令的参数

| 参数 | 含义 |
|---|---|
| `<FROM>` | 起始迁移。默认为 0（零），即初始迁移。 |
| `<TO>` | 目标迁移。默认为最后一个迁移。 |

如果未指定迁移名称，生成的脚本将包含所有迁移的累计内容。如果指定了迁移名称，脚本将包含这两个迁移之间（含两者）的更改。每个迁移都封装在一个事务中。如果执行脚本的数据库中不存在 `__EFMigrationsHistory` 表，则会创建该表。该表也会更新，以反映已执行的迁移。以下是一些示例：

```
// 为所有迁移生成脚本
dotnet ef migrations script

// 从初始迁移开始，生成到 Many2Many 迁移为止的脚本
dotnet ef migrations script 0 Many2Many
```

还有一些其他选项，如表 22-14 所示。`-o` 选项允许指定脚本文件（目录路径相对于命令的执行位置），`-i` 则会创建一个幂等脚本。这意味着脚本会检查某个迁移是否已应用；如果已应用，则跳过该迁移。`--no-transaction` 选项会禁用通常添加到脚本中的事务。

### 表 22-14. EF Core 迁移 Script 命令的其他选项

| 选项（简写 \|\| 全称） | 含义 |
|---|---|
| `-o` \|\| `--output <FILE>` | 写入生成脚本的文件 |
| `-i` \|\| `--idempotent` | 生成脚本，在应用迁移前检查该迁移是否已应用 |
| `--no-transactions` | 不将每个迁移封装在事务中 |

## 数据库命令

数据库命令有两个：drop 和 update。drop 命令会删除数据库（如果数据库存在）。update 命令使用迁移更新数据库。

## Drop 命令

drop 命令会删除 `DbContext` 的 `OnConfiguring` 方法中上下文工厂所指定连接字符串对应的数据库。使用 force 选项时，命令不会请求确认，并会强制关闭所有连接。参见表 22-15。

### 表 22-15. EF Core 数据库 Drop 选项

| 选项（简写 \|\| 全称） | 含义 |
|---|---|
| `-f` \|\| `--force` | 不询问确认，强制关闭所有连接。 |
| `--dry-run` | 显示将要删除的数据库，但不实际删除。 |

## Database Update 命令

update 命令接受一个参数（迁移名称）以及常用选项。该命令还有一个额外选项：`--connection <CONNECTION>`。它允许使用未在设计时工厂或 `DbContext` 中配置的连接字符串。

如果执行命令时未指定迁移名称，命令会将数据库更新到最新迁移；必要时还会创建数据库。如果指定了迁移名称，数据库将更新到该迁移。所有尚未应用的先前迁移也会一并应用。应用迁移时，其名称会存储在 `__EFMigrationsHistory` 表中。

如果指定迁移的时间戳早于其他已应用的迁移，则所有时间戳更晚的迁移都会回滚。如果指定迁移为 0（零），则会还原所有迁移，数据库将变为空数据库（但 `__EFMigrationsHistory` 表仍会保留）。

## DbContext 命令

DbContext 命令共有四个。其中三个（list、info、script）用于项目中的派生 `DbContext` 类。scaffold 命令则根据现有数据库创建派生 `DbContext` 和实体。表 22-16 列出了这四个命令。

### 表 22-16. DbContext 命令

| 命令 | 含义 |
|---|---|
| Info | 获取有关 `DbContext` 类型的信息 |
| List | 列出可用的 `DbContext` 类型 |
| Scaffold | 为数据库生成 `DbContext` 和实体类型 |
| Script | 根据对象模型从 `DbContext` 生成 SQL 脚本，不使用任何迁移 |

list 和 info 命令支持常用选项。list 命令会列出目标项目中的派生 `DbContext` 类。info 命令会提供指定派生 `DbContext` 类的详细信息，包括连接字符串、提供程序名称、数据库名称和数据源。script 命令会根据对象模型创建 SQL 脚本来创建数据库，并忽略可能存在的任何迁移。scaffold 命令用于对现有数据库进行反向工程，下一节将对此进行介绍。

## DbContext Scaffold 命令

scaffold 命令根据现有数据库创建 C# 类（派生 `DbContext` 和实体），并可按需包含数据注释和 Fluent API 命令。该命令有两个必需参数：数据库连接字符串和提供程序的完全限定名称（例如 `Microsoft.EntityFrameworkCore.SqlServer`）。表 22-17 描述了这些参数。

### 表 22-17. DbContext Scaffold 参数

| 参数 | 含义 |
|---|---|
| Connection | 数据库连接字符串 |
| Provider | 要使用的 EF Core 数据库提供程序（例如 `Microsoft.EntityFrameworkCore.SqlServer`） |

可用选项包括：选择特定架构和表、指定所创建上下文类的名称和命名空间、指定生成的实体类的输出目录和命名空间，等等。此外，还可以使用标准选项。扩展选项列于表 22-18，后文将对此进行讨论。

### 表 22-18. DbContext Scaffold 选项

| 选项（简写 \|\| 全称） | 含义 |
|---|---|
| `-d` \|\| `--data-annotations` | 尽可能使用特性配置模型。如果省略，则只使用 Fluent API。 |
| `-c` \|\| `--context <NAME>` | 要创建的派生 `DbContext` 的名称。 |
| `--context-dir <PATH>` | 放置派生 `DbContext` 的目录，相对于项目目录。默认为数据库名称。 |
| `-f` \|\| `--force` | 替换目标目录中的任何现有文件。 |
| `-o` \|\| `--output-dir <PATH>` | 放置生成的实体类的目录，相对于项目目录。 |
| `--schema <SCHEMA_NAME>...` | 要为其生成实体类型的表所在的架构。 |
| `-t` \|\| `--table <TABLE_NAME>...` | 要为其生成实体类型的表。 |
| `--use-database-names` | 直接使用数据库中的表名和列名。 |
| `-n` \|\| `--namespaces <NAMESPACE>` | 生成的实体类的命名空间。默认与目录匹配。 |
| `--context-namespace <NAMESPACE>` | 生成的派生 `DbContext` 类的命名空间。默认与目录匹配。 |
| `--no-onconfiguring` | 不生成 `OnConfiguring` 方法。 |
| `--no-pluralize` | 不使用复数化器。 |

在 EF Core 5.0 中，scaffold 命令变得更加强大。如你所见，可选项非常丰富。如果选择数据注释（`-d`）选项，EF Core 会在可行的情况下使用数据注释，并通过 Fluent API 补充两者之间的差异。如果不选择该选项，则整个配置（与约定不同时）都会通过 Fluent API 编写。你可以指定生成的实体和派生 `DbContext` 文件的命名空间、架构和位置。如果不想为整个数据库生成代码，可以选择特定的架构和表。`--no-onconfiguring` 选项会从生成的类中移除 `OnConfiguring()` 方法；`--no-pluralize` 选项则会关闭复数化器。复数化器在创建迁移时会将单数实体（Car）转换为复数表（Cars），在生成实体时则会将复数表转换为单数实体。

## 总结

本章开启了对 Entity Framework Core 的探索。本章介绍了 EF Core 的基础知识、查询的执行方式以及更改跟踪。你了解了如何塑造模型、EF Core 约定、数据注释和 Fluent API，以及它们如何影响数据库设计。最后一节介绍了 EF Core 命令行界面和全局工具的强大功能。

本章涵盖了大量理论知识和一些代码，而下一章几乎全是代码，只包含少量理论内容。完成第 23 章后，你将拥有完整的 AutoLot 数据访问层。




