

## 创建 DbContext 设计时工厂

正如你在第 22 章中回忆的那样，IDesignTimeDbContextFactory 由 EF Core 命令行工具用来创建派生的 DbContext 类的实例。在 AutoLot.Dal 项目的 EfStructures 目录中创建一个名为 ApplicationDbContextFactory.cs 的新类文件。向该类添加以下命名空间：

```csharp
using System;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Design;
```

工厂的细节已在上一章中介绍过，所以我只在这里列出代码。额外的 Console.WriteLine() 调用会将连接字符串输出到控制台。这仅用于信息目的。请确保更新你的连接字符串以匹配你的环境。

```csharp
namespace AutoLot.Dal.EfStructures
{
    public class ApplicationDbContextFactory : IDesignTimeDbContextFactory<ApplicationDbContext>
    {
        public ApplicationDbContext CreateDbContext(string[] args)
        {
            var optionsBuilder = new DbContextOptionsBuilder<ApplicationDbContext>();
            var connectionString = @"server=.,5433;Database=AutoLot;User Id=sa;Password=P@ssw0rd;";
            optionsBuilder.UseSqlServer(connectionString);
            Console.WriteLine(connectionString);
            return new ApplicationDbContext(optionsBuilder.Options);
        }
    }
}
```

## 创建初始迁移

请记住，第一次迁移会创建三个文件：迁移 partial 类的两个文件，以及第三个文件是完整的模型快照。在 AutoLot.Dal 目录的命令提示符中输入以下内容，以创建一个名为 Initial 的新迁移（使用刚刚生成的 ApplicationDbContext 实例）并将迁移文件放在 AutoLot.Dal 项目的 EfStructures\Migrations 文件夹中：

```bash
dotnet ef migrations add Initial -o EfStructures\Migrations -c AutoLot.Dal.EfStructures.ApplicationDbContext
```

> **注意** 在第一次迁移创建并应用之前，务必确保不对生成的任何文件或数据库进行更改。任一方面的更改都会导致代码和数据库不同步。一旦应用，所有对数据库的更改都必须通过 EF Core 迁移完成。

要确认迁移已创建并等待应用，请执行 list 命令。

```bash
dotnet ef migrations list -c AutoLot.Dal.EfStructures.ApplicationDbContext
```

结果将显示 Initial 迁移处于待处理状态（你的时间戳会不同）。由于 CreateDbContext() 方法中的 Console.WriteLine()，连接字符串会显示在输出中。

```text
Build started...
Build succeeded.
server=.,5433;Database=AutoLot;User Id=sa;Password=P@ssw0rd;
20201231203939_Initial (Pending)
```

## 应用迁移

将迁移应用到数据库的最简单方法是删除数据库并重新创建它。如果这是可行的选项，你可以输入以下命令，然后继续下一节：

```bash
dotnet ef database drop -f
dotnet ef database update Initial -c AutoLot.Dal.EfStructures.ApplicationDbContext
```

如果删除并重新创建数据库不是一个选项（例如，它是 Azure SQL 数据库），那么 EF Core 需要认为迁移已被应用。幸运的是，有 EF Core 完成所有工作，这很简单。首先通过使用以下命令从迁移创建 SQL 脚本来开始：

```bash
dotnet ef migrations script --idempotent -o FirstMigration.sql
```

此脚本的相关部分是创建 `__EFMigrationsHistory` 表的部分，然后将迁移记录添加到表中以表明它已被应用。将这些片段复制到 Azure Data Studio 或 SQL Server Management Studio 中的新查询中。以下是你需要的 SQL 代码（你的时间戳会不同）：

```sql
IF OBJECT_ID(N'[__EFMigrationsHistory]') IS NULL
BEGIN
    CREATE TABLE [__EFMigrationsHistory] (
        [MigrationId] nvarchar(150) NOT NULL,
        [ProductVersion] nvarchar(32) NOT NULL,
        CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
    );
END;
GO

INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
VALUES (N'20201231203939_Initial', N'5.0.1');
```

现在如果运行 list 命令，它将不再显示 Initial 迁移为待处理。初始迁移应用后，项目和数据库保持同步，开发将继续代码优先。

## 更新模型

本节将所有当前实体更新到其最终版本并添加一个日志实体。请注意，直到本节完成，你的项目将无法编译。

## 实体

在 AutoLot.Models 项目的 Entities 目录中，你会找到五个文件，每个数据库表一个文件。请注意名称是单数而不是复数（数据库中是复数）。这是 EF Core 5 中的一个更改，从数据库脚手架实体时复数化器默认开启。

你将对实体进行的更改包括添加基类、创建拥有的 Person 实体、修复导航属性名称，并添加一些附加属性。你还将添加一个新的日志实体（将在 ASP.NET Core 章节中使用）。上一章深入介绍了 EF Core 约定、数据注释和 Fluent API，因此本节大部分内容将是代码列表和简要说明。

## BaseEntity 类

BaseEntity 类将保存每个实体上都有的 Id 和 TimeStamp 列。在 AutoLot.Models 项目的 Entities 目录中创建一个名为 Base 的新目录。在此目录中，创建一个名为 BaseEntity.cs 的新文件。将代码更新为与以下内容匹配：

```csharp
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

namespace AutoLot.Models.Entities.Base
{
    public abstract class BaseEntity
    {
        [Key, DatabaseGenerated(DatabaseGeneratedOption.Identity)]
        public int Id { get; set; }

        [Timestamp]
        public byte[]? TimeStamp { get; set; }
    }
}
```

从 AutoLot 数据库脚手架生成的所有实体都将更新为使用此基类。

## 拥有的 Person 实体

Customer 和 CreditRisk 实体都具有 FirstName 和 LastName 属性。每个实体中具有完全相同属性的实体可以通过将这些属性移动到拥有的类中受益。虽然两个属性是一个简单的示例，但拥有实体有助于减少代码重复并提高一致性。除了类中的两个属性外，还添加了一个新属性，该属性将映射到 SQL Server 计算列。

在 AutoLot.Models 项目的 Entities 目录中创建一个名为 Owned 的新目录。在此新目录中，创建一个名为 Person.cs 的新文件。将代码更新为与以下内容匹配：

```csharp
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;
using Microsoft.EntityFrameworkCore;

namespace AutoLot.Models.Entities.Owned
{
    [Owned]
    public class Person
    {
        [Required, StringLength(50)]
        public string FirstName { get; set; } = "New";

        [Required, StringLength(50)]
        public string LastName { get; set; } = "Customer";

        [DatabaseGenerated(DatabaseGeneratedOption.Computed)]
        public string? FullName { get; set; }
    }
}
```

FullName 属性是可空的，因为新实体在保存到数据库之前不会设置该值。FullName 属性的最终配置将使用 Fluent API 添加。

## Car（库存）实体

Inventory 表被脚手架为名为 Inventory 的实体类。我们更喜欢使用名称 Car。这很容易修复：将文件名更改为 Car.cs 并将类名更改为 Car。Table 属性已正确应用，只需添加 dbo 架构。请注意，schema 参数是可选的，因为 SQL Server 默认使用 dbo，但我为了完整性而包含它。

```csharp
[Table("Inventory", Schema = "dbo")]
[Index(nameof(MakeId), Name = "IX_Inventory_MakeId")]
public partial class Car : BaseEntity
{
    ...
}
```

将 using 语句更新为与以下内容匹配：

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;
using System.Text.Json.Serialization;
using AutoLot.Models.Entities.Base;
using Microsoft.EntityFrameworkCore;
```

