

将类设为 public 且 static，如下所示：

```
namespace AutoLot.Dal.Initialization
{
    public static class SampleDataInitializer
    {
    }
}
```

创建一个名为 DropAndCreateDatabase 的方法，该方法接受 ApplicationDbContext 实例作为唯一参数。此方法使用 ApplicationDbContext 的 Database 属性首先删除数据库（使用 EnsureDeleted() 方法），然后创建数据库（使用 Migrate() 方法）。

```
public static void DropAndCreateDatabase(ApplicationDbContext context)
{
    context.Database.EnsureDeleted();
    context.Database.Migrate();
}
```

再创建一个名为 ClearData() 的方法，该方法删除数据库中的所有数据并为每个表的每个主键重置标识值。该方法遍历域实体的列表，并使用 DbContext Model 属性获取每个实体映射到的架构和表名。然后它执行删除语句并使用 DbContext Database 属性上的 ExecuteSqlRaw() 方法为每个表重置标识。

```
internal static void ClearData(ApplicationDbContext context)
{
    var entities = new[]
    {
        typeof(Order).FullName,
        typeof(Customer).FullName,
        typeof(Car).FullName,
        typeof(Make).FullName,
        typeof(CreditRisk).FullName
    };
    foreach (var entityName in entities)
    {
        var entity = context.Model.FindEntityType(entityName);
        var tableName = entity.GetTableName();
        var schemaName = entity.GetSchema();
        context.Database.ExecuteSqlRaw($"DELETE FROM {schemaName}.{tableName}");
        context.Database.ExecuteSqlRaw($"DBCC CHECKIDENT (\"{schemaName}.{tableName}\", RESEED, 1);");
    }
}
```

注意 ExecuteSqlRaw() 方法的数据库外观应谨慎使用，以防止潜在的 SQL 注入攻击。

既然你已经可以删除并创建数据库以及清除数据，现在是时候创建将添加示例数据的那些方法了。

## 数据初始化

我们将构建自己的数据播种系统，可以按需运行。第一步是创建示例数据，然后将方法添加到 SampleDataInitializer 中以将示例数据加载到数据库中。

## 创建示例数据

在 Initialization 文件夹中添加一个名为 SampleData.cs 的新文件。使类为 public 且 static 并将 using 语句更新为以下内容：

```
using System.Collections.Generic;
using AutoLot.Dal.Entities;
using AutoLot.Dal.Entities.Owned;

namespace AutoLot.Dal.Initialization
{
    public static class SampleData
    {
    }
}
```

该文件由五个静态方法组成，用于创建示例数据。

```
public static List<Customer> Customers => new()
{
    new() {Id = 1, PersonalInformation = new() {FirstName = "Dave", LastName = "Brenner"}},
    new() {Id = 2, PersonalInformation = new() {FirstName = "Matt", LastName = "Walton"}},
    new() {Id = 3, PersonalInformation = new() {FirstName = "Steve", LastName = "Hagen"}},
    new() {Id = 4, PersonalInformation = new() {FirstName = "Pat", LastName = "Walton"}},
    new() {Id = 5, PersonalInformation = new() {FirstName = "Bad", LastName = "Customer"}},
};
```

```
public static List<Make> Makes => new()
{
    new() {Id = 1, Name = "VW"},
    new() {Id = 2, Name = "Ford"},
    new() {Id = 3, Name = "Saab"},
    new() {Id = 4, Name = "Yugo"},
    new() {Id = 5, Name = "BMW"},
    new() {Id = 6, Name = "Pinto"},
};
```

```
public static List<Car> Inventory => new()
{
    new() {Id = 1, MakeId = 1, Color = "Black", PetName = "Zippy"},
    new() {Id = 2, MakeId = 2, Color = "Rust", PetName = "Rusty"},
    new() {Id = 3, MakeId = 3, Color = "Black", PetName = "Mel"},
    new() {Id = 4, MakeId = 4, Color = "Yellow", PetName = "Clunker"},
    new() {Id = 5, MakeId = 5, Color = "Black", PetName = "Bimmer"},
    new() {Id = 6, MakeId = 5, Color = "Green", PetName = "Hank"},
    new() {Id = 7, MakeId = 5, Color = "Pink", PetName = "Pinky"},
    new() {Id = 8, MakeId = 6, Color = "Black", PetName = "Pete"},
    new() {Id = 9, MakeId = 4, Color = "Brown", PetName = "Brownie"},
    new() {Id = 10, MakeId = 1, Color = "Rust", PetName = "Lemon", IsDrivable = false},
};
```

```
public static List<Order> Orders => new()
{
    new() {Id = 1, CustomerId = 1, CarId = 5},
    new() {Id = 2, CustomerId = 2, CarId = 1},
    new() {Id = 3, CustomerId = 3, CarId = 4},
    new() {Id = 4, CustomerId = 4, CarId = 7},
    new() {Id = 5, CustomerId = 5, CarId = 10},
};
```

```
public static List<CreditRisk> CreditRisks => new()
{
new()
{
    Id = 1,
    CustomerId = Customers[4].Id,
    PersonalInformation = new()
    {
        FirstName = Customers[4].PersonalInformation.FirstName,
        LastName = Customers[4].PersonalInformation.LastName
    }
}
};
```

## 加载示例数据

SampleDataInitializer 类中的 internal SeedData() 方法将 SampleData 方法中的数据添加到 ApplicationDbContext 实例中，然后将数据持久化到数据库。

```
internal static void SeedData(ApplicationDbContext context)
{
try
{
    ProcessInsert(context, context.Customers!, SampleData.Customers);
    ProcessInsert(context, context.Makes!, SampleData.Makes);
    ProcessInsert(context, context.Cars!, SampleData.Inventory);
    ProcessInsert(context, context.Orders!, SampleData.Orders);
    ProcessInsert(context, context.CreditRisks!, SampleData.CreditRisks);
}
catch (Exception ex)
{
    Console.WriteLine(ex);
}
```

```
//Set a break point here to determine what the issues is
throw;
}
static void ProcessInsert<TEntity>(
    ApplicationDbContext context,
    DbSet<TEntity> table,
    List<TEntity> records) where TEntity : BaseEntity
{
    if (table.Any())
    {
        return;
    }
    IExecutionStrategy strategy = context.Database.CreateExecutionStrategy();
    strategy.Execute(() =>
    {
        using var transaction = context.Database.BeginTransaction();
        try
        {
            var metaData = context.Model.FindEntityType(typeof(TEntity).FullName);
            context.Database.ExecuteSqlRaw(
                $"SET IDENTITY_INSERT {metaData.GetSchema()}.{metaData.GetTableName()} ON");
            table.AddRange(records);
            context.SaveChanges();
            context.Database.ExecuteSqlRaw(
                $"SET IDENTITY_INSERT {metaData.GetSchema()}.{metaData.GetTableName()} OFF");
            transaction.Commit();
        }
        catch (Exception)
        {
            transaction.Rollback();
        }
    });
}
```

SeedData() 方法使用局部函数来处理数据。它首先检查表是否有任何记录，如果没有，则继续处理示例数据。从数据库外观创建 ExecutionStrategy，并使用它来创建显式事务，这对于打开和关闭标识插入是必需的。添加记录，如果一切成功，则提交事务；否则回滚。

这两个方法是 public 并用于重置数据库。InitializeData() 在播种之前先删除并重新创建数据库，而 ClearDatabase() 方法只是删除所有记录、重置标识，然后播种数据。

```
public static void InitializeData(ApplicationDbContext context)
{
    DropAndCreateDatabase(context);
    SeedData(context);
}
```

```
public static void ClearAndReseedDatabase(ApplicationDbContext context)
{
    ClearData(context);
    SeedData(context);
}
```

## 设置测试驱动

与其创建一个客户端应用程序来测试驱动完成的 AutoLot 数据访问层，我们将使用自动化集成测试。测试将演示对数据库的创建、读取、更新和删除调用。这使我们能够在不创建另一个应用程序的开销的情况下检查代码。本节中的每个测试都将执行一个查询（创建、读取、更新或删除），然后有一个或多个 Assert 语句来验证结果是否符合预期。

## 创建项目

为了开始，我们将使用 xUnit，一个 .NET Core 兼容的测试框架来设置集成测试平台。首先添加一个名为 AutoLot.Dal.Tests 的新 xUnit 测试项目。在 Visual Studio 中，此项目类型名为 xUnit Test Project (.NET Core)。

> **注意** 单元测试旨在测试单个代码单元。我们在本章中将要做的技术上是创建 *集成* 测试，因为我们正在测试 C# 代码 *和* EF Core 一直到数据库并返回。

从命令行界面执行以下命令：

```
dotnet new xunit -lang c# -n AutoLot.Dal.Tests -o ./AutoLot.Dal.Tests -f net5.0
dotnet sln ./Chapter23_AllProjects.sln add AutoLot.Dal.Tests
```

向 AutoLot.Dal.Tests 项目添加以下 NuGet 包：

- Microsoft.EntityFrameworkCore
- Microsoft.EntityFrameworkCore.SqlServer
- Microsoft.Extensions.Configuration.Json

由于随 xUnit 项目模板提供的 Microsoft.NET.Test.Sdk 包的版本通常落后于当前可用版本，请使用 NuGet 包管理器更新所有 NuGet 包。接下来，添加对 AutoLot.Models 和 AutoLot.Dal 的项目引用。

如果你使用 CLI，请执行以下命令（注意命令会先移除再添加 Microsoft.NET.Test.Sdk 以确保引用最新版本）：

```
dotnet add AutoLot.Dal.Tests package Microsoft.EntityFrameworkCore
dotnet add AutoLot.Dal.Tests package Microsoft.EntityFrameworkCore.SqlServer
dotnet add AutoLot.Dal.Tests package Microsoft.Extensions.Configuration.Json
dotnet remove AutoLot.Dal.Tests package Microsoft.NET.Test.Sdk
dotnet add AutoLot.Dal.Tests package Microsoft.NET.Test.Sdk
dotnet add AutoLot.Dal.Tests reference AutoLot.Dal
dotnet add AutoLot.Dal.Tests reference AutoLot.Models
```
