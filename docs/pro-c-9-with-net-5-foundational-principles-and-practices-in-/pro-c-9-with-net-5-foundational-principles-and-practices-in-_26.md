## 第23章

## 使用Entity Framework Core构建数据访问层

上一章介绍了EF Core的细节及其功能。本章重点是将你学到的EF Core知识应用到构建AutoLot数据访问层上。首先，你将从上一章的数据库搭建实体和派生的DbContext。然后，将项目从数据库优先改为代码优先，将实体更新到最终版本，并使用EF Core迁移应用到数据库。对数据库的最后一次更改是重新创建GetPetName存储过程，并新建一个数据库视图（配有相应的视图模型），所有这些都通过迁移来完成。

下一步是创建仓库，为数据库提供隔离的创建、读取、更新和删除（CRUD）访问。然后将包含示例数据的数据初始化代码添加到项目中，用于测试。本章剩余部分通过自动化集成测试来测试AutoLot数据访问层。

## 代码优先还是数据库优先

在开始构建数据访问层之前，我们先花点时间讨论使用EF Core和数据库的两种不同方式：代码优先和数据库优先。这两种方式都是使用EF Core的有效方式，选择哪种方法主要取决于你的开发团队。

代码优先意味着你在代码中创建并配置实体类和派生的DbContext，然后使用迁移来更新数据库。这是大多数绿地项目或新项目的开发方式。其优势在于，当你构建应用程序时，实体会根据应用程序的需求而演变。迁移保持数据库同步，因此数据库设计会随着应用程序一起演变。这种涌现式设计过程在敏捷开发团队中很受欢迎，因为你在正确的时间构建正确的部分。

如果你已经有一个数据库，或者更喜欢让数据库设计驱动应用程序，这被称为数据库优先。你不必手动创建派生的DbContext和所有实体，而是从数据库搭建这些类。当数据库发生变化时，你需要重新搭建类以保持代码与数据库同步。实体或派生DbContext中的任何自定义代码都必须放在分部类中，这样在重新搭建类时不会被覆盖。幸运的是，搭建过程正是出于这个原因而创建分部类。

无论你选择哪种方法——代码优先还是数据库优先——都要知道这是一个承诺。如果你使用代码优先，所有更改都针对实体和上下文类，并使用迁移更新数据库。如果你使用数据库优先，所有更改都必须在数据库中进行，然后重新搭建类。通过一些努力和规划，你可以从数据库优先切换到代码优先（反之亦然），但你不应该同时手动修改代码和数据库。

## 创建AutoLot.Dal和AutoLot.Models项目

AutoLot数据访问层由两个项目组成：一个用于存放EF Core特定的代码（派生的DbContext、上下文工厂、仓库、迁移等），另一个用于存放实体和视图模型。创建一个名为Chapter23_AllProjects的新解决方案，并将一个名为AutoLot.Models的.NET Core类库添加到解决方案中。删除模板创建的默认类，并向项目添加以下NuGet包：

```text
Microsoft.EntityFrameworkCore.Abstractions
System.Text.Json
```

Microsoft.EntityFrameworkCore.Abstractions包提供对许多EF Core构造（如数据注解）的访问，并且比Microsoft.EntityFrameworkCore包更轻量。

将另一个名为AutoLot.Dal的.NET Core类库项目添加到解决方案中。删除模板创建的默认类，添加对AutoLot.Models项目的引用，并向项目添加以下NuGet包：

```text
Microsoft.EntityFrameworkCore
Microsoft.EntityFrameworkCore.SqlServer
Microsoft.EntityFrameworkCore.Design
```

Microsoft.EntityFrameworkCore包提供EF Core的通用功能。Microsoft.EntityFrameworkCore.SqlServer包提供SQL Server数据提供程序，而Microsoft.EntityFrameworkCore.Design包是EF Core命令行工具所必需的。

要使用命令行完成所有这些步骤，请使用以下命令（在你希望创建解决方案的目录中）：

```text
dotnet new sln -n Chapter23_AllProjects

dotnet new classlib -lang c# -n AutoLot.Models -o .\AutoLot.Models -f net5.0
dotnet sln .\Chapter23_AllProjects.sln add .\AutoLot.Models
dotnet add AutoLot.Models package Microsoft.EntityFrameworkCore.Abstractions
dotnet add AutoLot.Models package System.Text.Json

dotnet new classlib -lang c# -n AutoLot.Dal -o .\AutoLot.Dal -f net5.0
dotnet sln .\Chapter23_AllProjects.sln add .\AutoLot.Dal
dotnet add AutoLot.Dal reference AutoLot.Models
dotnet add AutoLot.Dal package Microsoft.EntityFrameworkCore
dotnet add AutoLot.Dal package Microsoft.EntityFrameworkCore.Design
dotnet add AutoLot.Dal package Microsoft.EntityFrameworkCore.SqlServer
dotnet add AutoLot.Dal package Microsoft.EntityFrameworkCore.Tools
```

> **注意：** 如果你使用的不是基于Windows的机器，请根据你的操作系统调整目录分隔符。本章中的所有CLI命令都需要这样做。

创建项目后，更新每个 *.csproj 文件以启用C# 8可空引用类型。更新内容以粗体显示：

```xml
<PropertyGroup>
    <TargetFramework>net5.0</TargetFramework>
    <Nullable>enable</Nullable>
</PropertyGroup>
```

## 搭建DbContext和实体

下一步是使用EF Core命令行工具从第21章搭建AutoLot数据库。在命令提示符或Visual Studio的程序包管理器控制台中导航到AutoLot.Dal项目目录。

> **注意：** 第21章的仓库文件夹中包含适用于Windows和Docker的数据库备份。如果需要还原数据库，请参阅第21章中的说明。

使用EF Core CLI工具将AutoLot数据库搭建为实体和DbContext派生类，命令如下（全部在一行中）：

```text
dotnet ef dbcontext scaffold "server=.,5433;Database=AutoLot;User Id=sa;Password=P@ssw0rd;" Microsoft.EntityFrameworkCore.SqlServer -d -c ApplicationDbContext --context-namespace AutoLot.Dal.EfStructures --context-dir EfStructures --no-onconfiguring -n AutoLot.Models.Entities -o ../AutoLot.Models\Entities
```

上述命令使用SQL Server数据库提供程序搭建位于所提供连接字符串处的数据库（这是第21章中Docker容器使用的连接字符串）。-d标志表示尽可能优先使用数据注解（而非Fluent API）。-c用于命名上下文，--context-namespace指定上下文的命名空间，--context-dir指示上下文所在的目录（相对于当前项目），--no-onconfiguring阻止搭建OnConfiguring方法，-o是实体的输出目录（相对于项目目录），-n指定实体的命名空间。此命令将所有实体放入AutoLot.Models项目的Entities文件夹中，并将ApplicationDbContext放入AutoLot.Dal项目的EfStructures文件夹中。

如果你一直跟着本章操作，会注意到存储过程没有被搭建。如果数据库中有任何视图，它们会被搭建为无键实体。由于EF Core没有直接映射到存储过程的构造，因此没有什么可将存储过程搭建进去。存储过程和其他SQL对象可以使用EF Core创建，但目前只有表和视图会被搭建。

## 切换到代码优先

现在你已将数据库搭建为实体，是时候从数据库优先切换到代码优先了。要切换，必须创建一个上下文工厂，并根据项目的当前状态创建迁移。接下来，通过删除并重新创建数据库来应用迁移，或者通过“欺骗”EF Core来假应用迁移。





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





接下来，继承自 BaseEntity，并移除 Id 和 TimeStamp 属性、构造函数以及 pragma #nullable disable。进行这些更改后类的代码如下：

```
namespace AutoLot.Models.Entities
{
    [Table("Inventory", Schema = "dbo")]
    [Index(nameof(MakeId), Name = "IX_Inventory_MakeId")]
    public partial class Car : BaseEntity
    {
        public int MakeId { get; set; }
        [Required]
        [StringLength(50)]
        public string Color { get; set; }
        [Required]
        [StringLength(50)]
        public string PetName { get; set; }
        [ForeignKey(nameof(MakeId))]
        [InverseProperty("Inventories")]
        public virtual Make Make { get; set; }
        [InverseProperty(nameof(Order.Car))]
        public virtual ICollection<Order> Orders { get; set; }
    }
}
```

此代码仍有一些问题需要修复，并且需要添加新的属性。
Color 和 PetName 属性被设置为不可空，但构造函数中未设置值，也未通过属性定义进行初始化。这通过为每个属性分配初始化器来解决。为 PetName 属性添加 DisplayName 属性以获得更好的可读名称。更新属性以匹配以下内容（更改以粗体显示）：

```
[Required]
[StringLength(50)]
public string Color { get; set; } = "Gold";

[Required]
[StringLength(50)]
[DisplayName("Pet Name")]
public string PetName { get; set; } = "My Precious";
```

> 注意 DisplayName 属性由 ASP.NET Core 使用，将在第 8 部分介绍。

Make 导航属性需要重命名为 MakeNavigation 并设为可空，并且 inverse property 使用了魔术字符串而不是 C# 的 nameof() 方法。最后的更改是移除 virtual 修饰符。以下是更新后的属性：

```
[ForeignKey(nameof(MakeId))]
[InverseProperty(nameof(Make.Cars))]
public Make? MakeNavigation { get; set;}
```

注意 virtual 修饰符对于延迟加载是必需的。由于本书中的示例均未使用延迟加载，数据访问层中的所有属性都将移除 virtual 修饰符。

Orders 导航属性需要 JsonIgnore 属性，以防止在序列化对象模型时出现循环 JSON 引用。生成的代码确实在 inverse property 中使用了 nameof() 方法，但需要更新，因为所有引用导航属性都将添加 Navigation 后缀。最后的更改是将属性的类型从 ICollection<Order> 改为 IEnumerable<Order> 并使用 new List<Order>() 初始化。这不是必需的更改，因为 ICollection<Order> 也能工作。我更喜欢在集合导航属性上使用较低级别的 IEnumerable<T>（因为 IQueryable<T> 和 ICollection<T> 都派生自 IEnumerable<T>）。将代码更新为匹配以下内容：

```
[JsonIgnore]
[InverseProperty(nameof(Order.CarNavigation))]
public IEnumerable<Order> Orders { get; set; } = new List<Order>();
```

接下来，添加一个 NotMapped 属性来显示 Car 的 Make 值。这消除了第 21 章中 CarViewModel 的需求。如果在从数据库检索 Car 记录时检索了相关的 Make 信息，则会显示 Make 名称。如果未检索相关数据，则属性显示 "Unknown"。作为提醒，NotMapped 属性不是数据库的一部分，仅存在于实体上。添加以下内容：

```
[NotMapped]
public string MakeName => MakeNavigation?.Name ?? "Unknown";
```

重写 ToString() 以显示车辆信息。

```
public override string ToString()
{
    // Since the PetName column could be empty, supply
    // the default name of **No Name**.
    return $"{PetName ?? "**No Name**"} is a {Color} {MakeNavigation?.Name} with ID {Id}.";
}
```

为 MakeId 添加 Required 和 DisplayName 属性。即使 EF Core 认为 MakeId 属性是必需的，因为它是不可空的，ASP.NET Core 验证引擎仍需要 Required 属性。将代码更新为匹配以下内容：

```
[Required]
[DisplayName("Make")]
public int MakeId { get; set; }
```

最后的更改是添加不可空的 bool IsDrivable 属性，具有可空的 backing field 和显示名称。

```
private bool? _isDrivable;

[DisplayName("Is Drivable")]
public bool IsDrivable
{
    get => _isDrivable ?? false;
    set => _isDrivable = value;
}
```

至此完成了更新后的 Car 实体。

## The Customer Entity

Customers 表被生成为名为 Customer 的实体类。将 using 语句更新为匹配以下内容：

```
using System;
using System.Collections.Generic;
using System.ComponentModel.DataAnnotations.Schema;
using System.Text.Json.Serialization;
using AutoLot.Models.Entities.Base;
using AutoLot.Models.Entities.Owned;
```

接下来，继承自 BaseEntity 并移除 Id 和 TimeStamp 属性。删除构造函数和 pragma #nullable disable 并添加带有 schema 的 Table 属性。移除 FirstName 和 LastName 属性，因为它们将被 Person 拥有实体替换。这就是此时类的代码状态：

```
namespace AutoLot.Models.Entities
{
    [Table("Customers", Schema = "dbo")]
    public partial class Customer : BaseEntity
    {
        [InverseProperty(nameof(CreditRisk.Customer))]
        public virtual ICollection<CreditRisk> CreditRisks { get; set; }
        [InverseProperty(nameof(Order.Customer))]
        public virtual ICollection<Order> Orders { get; set; }
    }
}
```

与 Car 实体类似，此代码仍有一些问题需要修复，并且必须添加拥有实体。导航属性需要 JsonIgnore 属性，inverse property 属性需要使用 Navigation 后缀进行更新，类型更改为已初始化的 IEnumerable<T>，并移除 virtual 修饰符。将代码更新为匹配以下内容：

```
[JsonIgnore]
[InverseProperty(nameof(CreditRisk.CustomerNavigation))]
public IEnumerable<CreditRisk> CreditRisks { get; set; } = new List<CreditRisk>();

[JsonIgnore]
[InverseProperty(nameof(Order.CustomerNavigation))]
public IEnumerable<Order> Orders { get; set; } = new List<Order>();
```

最后的更改是添加拥有属性。关系将在 Fluent API 中进一步配置。

```
public Person PersonalInformation { get; set; } = new Person();
```

至此完成了更新后的 Customer 实体。

## The Make Entity

Makes 表被生成为名为 Make 的实体类。将 using 语句更新为匹配以下内容：

```
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;
using System.Text.Json.Serialization;
using AutoLot.Models.Entities.Base;
using Microsoft.EntityFrameworkCore;
```

继承自 BaseEntity 并移除 Id 和 TimeStamp 属性。删除构造函数和 pragma #nullable disable 并添加带有 schema 的 Table 属性。以下是实体的当前状态：

```
namespace AutoLot.Models.Entities
{
    [Table("Makes", Schema = "dbo")]
    public partial class Make : BaseEntity
    {
        [Required]
        [StringLength(50)]
        public string Name { get; set; }
        [InverseProperty(nameof(Inventory.Make))]
        public virtual ICollection<Inventory> Inventories { get; set; }
    }
}
```

以下代码显示了不可空的 Name 属性已初始化，并且 Cars 导航属性已更正（注意 nameof 方法中从 Inventory 到 Car 的更改）：

```
[Required]
[StringLength(50)]
public string Name { get; set; } = "Ford";

[JsonIgnore]
[InverseProperty(nameof((Car.MakeNavigation)))]
public IEnumerable<Car> Cars { get; set; } = new List<Car>();
```

至此完成了 Make 实体。

## The CreditRisk Entity

CreditRisks 表被生成为名为 CreditRisk 的实体类。将 using 语句更新为匹配以下内容：

```
using System.ComponentModel.DataAnnotations.Schema;
using AutoLot.Models.Entities.Base;
using AutoLot.Models.Entities.Owned;
```

继承自 BaseEntity 并移除 Id 和 TimeStamp 属性。删除构造函数和 pragma #nullable disable 并添加带有 schema 的 Table 属性。移除 FirstName 和 LastName 属性，因为它们将被 Person 拥有实体替换。以下是更新后的类代码：

```
namespace AutoLot.Models.Entities
{
    [Table("CreditRisks", Schema = "dbo")]
    public partial class CreditRisk : BaseEntity
    {
        public Person PersonalInformation { get; set; } = new Person();
        public int CustomerId { get; set; }

        [ForeignKey(nameof(CustomerId))]
        [InverseProperty("CreditRisks")]
        public virtual Customer Customer { get; set; }
    }
}
```

修复导航属性，移除 virtual 修饰符，在 InverseProperty 属性中使用 nameof() 方法，并为属性名称添加 Navigation 后缀。

```
[ForeignKey(nameof(CustomerId))]
[InverseProperty(nameof(Customer.CreditRisks))]
public Customer? CustomerNavigation { get; set; }
```

最后的更改是添加拥有属性。关系将在 Fluent API 中进一步配置。

```
public Person PersonalInformation { get; set; } = new Person();
```

至此完成了 CreditRisk 实体。

## The Order Entity

Orders 表被生成为名为 Order 的实体类。将 using 语句更新为匹配以下内容：

```
using System;
using System.ComponentModel.DataAnnotations.Schema;
using AutoLot.Models.Entities.Base;
using Microsoft.EntityFrameworkCore;
```




继承自 BaseEntity 并删除 Id 和 TimeStamp 属性。删除构造函数和 `#nullable disable` pragma，并添加带有 schema 的 Table 特性。以下是当前代码：

```
namespace AutoLot.Models.Entities
{
    [Table("Orders", Schema = "dbo")]
    [Index(nameof(CarId), Name = "IX_Orders_CarId")]
    [Index(nameof(CustomerId), nameof(CarId), Name = "IX_Orders_CustomerId_CarId", IsUnique = true)]
    public partial class Order : BaseEntity
    {
        public int CustomerId { get; set; }
        public int CarId { get; set; }
        [ForeignKey(nameof(CarId))]
        [InverseProperty(nameof(Inventory.Orders))]
        public virtual Inventory Car { get; set; }
        [ForeignKey(nameof(CustomerId))]
        [InverseProperty("Orders")]
        public virtual Customer { get; set; }
    }
}
```

Car 和 Customer 导航属性需要在属性名称中添加 Navigation 后缀。Car 导航属性的类型需要从 Inventory 更正为 Car。反向属性需要 `nameof()` 方法使用 Car.Orders 而不是 Inventory.Orders。Customer 导航属性需要在 InverseProperty 中使用 `nameof()` 方法。两个属性都需要变为可空，并移除 virtual 修饰符。

```
[ForeignKey(nameof(CarId))]
[InverseProperty(nameof(Car.Orders))]
public Car? CarNavigation { get; set; }

[ForeignKey(nameof(CustomerId))]
[InverseProperty(nameof(Customer.Orders))]
public Customer? CustomerNavigation { get; set; }
```

这就完成了 Order 实体。

> 注意 此时，AutoLot.Models 项目应该能够正常构建。AutoLot.Dal 项目要等到 ApplicationDbContext 类更新后才能构建。

## SeriLogEntry 实体

数据库需要一个额外的表来保存日志记录。第 8 部分中的 ASP.NET Core 项目将使用 SeriLog 日志框架，其中一种选择是将日志记录写入 SQL Server 表。我们现在就添加该表，虽然知道它会在几章之后才被使用。

该表不关联任何其他表，也不使用 BaseEntity 类。在 Entities 文件夹中添加一个名为 SeriLogEntry.cs 的新类文件。此处完整列出代码：

```
using System;
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;
using System.Xml.Linq;

namespace AutoLot.Models.Entities
{
    [Table("Serilogs", Schema = "Logging")]
    public class SeriLogEntry
    {
        [Key, DatabaseGenerated(DatabaseGeneratedOption.Identity)]
        public int Id { get; set; }
        public string? Message { get; set; }
        public string? MessageTemplate { get; set; }
        [MaxLength(128)]
        public string? Level { get; set; }
        [DataType(DataType.DateTime)]
        public DateTime? TimeStamp { get; set; }
        public string? Exception { get; set; }
        public string? Properties { get; set; }
        public string? LogEvent { get; set; }
        public string? SourceContext { get; set; }
        public string? RequestPath { get; set; }
        public string? ActionName { get; set; }
        public string? ApplicationName { get; set; }
        public string? MachineName { get; set; }
        public string? FilePath { get; set; }
        public string? MemberName { get; set; }
        public int? LineNumber { get; set; }
        [NotMapped]
        public XElement? PropertiesXml => (Properties != null)? XElement.Parse(Properties):null;
    }
}
```

这就完成了 SeriLogEntry 实体。

> 注意 此实体中的 TimeStamp 属性与 BaseEntity 类中的 TimeStamp 属性不相同。名称相同，但在此表中，它保存的是条目被记录时的日期和时间（这将配置为 SQL Server 默认值），而不是其他实体中的 rowversion。

## ApplicationDbContext

是时候更新 ApplicationDbContext.cs 了。首先更新 using 语句，使其与以下内容匹配：

```
using System;
using System.Collections;
using System.Collections.Generic;
using AutoLot.Models.Entities;
using AutoLot.Models.Entities.Owned;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Storage;
using Microsoft.EntityFrameworkCore.ChangeTracking;
using AutoLot.Dal.Exceptions;
```

该文件以无参数构造函数开始。删除它，因为我们不需要它。下一个构造函数接受 DbContextOptions 对象的实例，暂时保持不变。DbContext 和 ChangeTracker 的事件钩子将在本章稍后添加。

DbSet<T> 属性需要更新为可空，修正名称，并移除 virtual 修饰符。需要添加新的日志实体。导航到 DbSet<T> 属性并将它们更新为以下内容：

```
public DbSet<SeriLogEntry>? LogEntries { get; set; }
public DbSet<CreditRisk>? CreditRisks { get; set; }
public DbSet<Customer>? Customers { get; set; }
public DbSet<Make>? Makes { get; set; }
public DbSet<Car>? Cars { get; set; }
public DbSet<Order>? Orders { get; set; }
```

### 更新 Fluent API 代码

OnModelCreating 覆盖是 Fluent API 代码所在的位置，它使用 ModelBuilder 类的实例来更新模型。

### SeriLog 实体

对该方法的第一个更改是为 SeriLogEntry 实体的配置添加 Fluent API 代码。Properties 属性是 SQL Server XML 列，TimeStamp 属性映射到 SQL Server 中的 datetime2 列，默认值设置为 getdate() SQL Server 函数。在 OnModelCreating 方法中，添加以下代码：

```
modelBuilder.Entity<SeriLogEntry>(entity =>
{
    entity.Property(e => e.Properties).HasColumnType("Xml");
    entity.Property(e => e.TimeStamp).HasDefaultValueSql("GetDate()");
});
```

### CreditRisk 实体

接下来要更新的代码是 CreditRisk 实体。TimeStamp 列的配置块将被移除，因为它已在 BaseEntity 中配置。导航配置必须使用新的名称更新。我们还断言导航属性不为 null。另一个更改是配置拥有实体的 FirstName 和 LastName 属性到列名称的映射，并为 FullName 属性添加计算值。以下是 CreditRisk 实体的更新块，更改处以粗体突出显示：

```
modelBuilder.Entity<CreditRisk>(entity =>
{
    entity.HasOne(d => d.CustomerNavigation)
        .WithMany(p => p!.CreditRisks)
        .HasForeignKey(d => d.CustomerId)
        .HasConstraintName("FK_CreditRisks_Customers");

    entity.OwnsOne(o => o.PersonalInformation,
        pd =>
        {
            pd.Property<string>(nameof(Person.FirstName))
                .HasColumnName(nameof(Person.FirstName))
                .HasColumnType("nvarchar(50)");
            pd.Property<string>(nameof(Person.LastName))
                .HasColumnName(nameof(Person.LastName))
                .HasColumnType("nvarchar(50)");
            pd.Property(p => p.FullName)
                .HasColumnName(nameof(Person.FullName))
                .HasComputedColumnSql("[LastName] + ', ' + [FirstName]");
        });
});
```

### Customer 实体

接下来要更新的代码是 Customer 实体。TimeStamp 代码被移除，并配置了拥有实体的属性。

```
modelBuilder.Entity<Customer>(entity =>
{
    entity.OwnsOne(o => o.PersonalInformation,
        pd =>
        {
            pd.Property(p => p.FirstName).HasColumnName(nameof(Person.FirstName));
            pd.Property(p => p.LastName).HasColumnName(nameof(Person.LastName));
            pd.Property(p => p.FullName)
                .HasColumnName(nameof(Person.FullName))
                .HasComputedColumnSql("[LastName] + ', ' + [FirstName]");
        });
});
```

### Make 实体

对于 Make 实体，更新配置块以移除 TimeStamp，并添加限制删除具有依赖实体的实体的代码。

```
modelBuilder.Entity<Make>(entity =>
{
    entity.HasMany(e => e.Cars)
        .WithOne(c => c.MakeNavigation!)
        .HasForeignKey(k => k.MakeId)
        .OnDelete(DeleteBehavior.Restrict)
        .HasConstraintName("FK_Make_Inventory");
});
```

### Order 实体

对于 Order 实体，更新导航属性名称并断言反向属性不为 null。Customer 到 Orders 的关系设置为级联删除，而不是限制删除。

```
modelBuilder.Entity<Order>(entity =>
{
    entity.HasOne(d => d.CarNavigation)
        .WithMany(p => p!.Orders)
        .HasForeignKey(d => d.CarId)
        .OnDelete(DeleteBehavior.ClientSetNull)
        .HasConstraintName("FK_Orders_Inventory");

    entity.HasOne(d => d.CustomerNavigation)
        .WithMany(p => p!.Orders)
        .HasForeignKey(d => d.CustomerId)
        .OnDelete(DeleteBehavior.Cascade)
        .HasConstraintName("FK_Orders_Customers");
});
```

在 Order 表的 CarNavigation 属性上设置一个查询过滤器，以过滤掉不可驾驶的汽车。请注意，此代码与前面的代码不在同一个块中。将配置放在单独的块中没有技术原因；这是一种替代语法。

```
modelBuilder.Entity<Order>().HasQueryFilter(e => e.CarNavigation!.IsDrivable);
```

### Car 实体

脚手架生成的类包含 Inventory 类的配置。需要将其更改为 Car 类。TimeStamp 可以被删除，导航属性配置随 MakeNavigation 和 Cars 属性名称的更新而保留。该实体获得一个查询过滤器，默认仅显示可驾驶的汽车，并将 IsDrivable 属性的默认值设置为 true。更新代码以匹配以下内容：

```
modelBuilder.Entity<Car>(entity =>
{
    entity.HasQueryFilter(c => c.IsDrivable);
    entity.Property(p => p.IsDrivable).HasField("_isDrivable").HasDefaultValue(true);
});
```




entity.HasOne(d => d.MakeNavigation)
    .WithMany(p => p.Cars)
    .HasForeignKey(d => d.MakeId)
    .OnDelete(DeleteBehavior.ClientSetNull)
    .HasConstraintName("FK_Make_Inventory");
});

## 自定义异常

异常处理中常见的模式是捕获系统异常（以及本示例中的 EF Core 异常），记录异常，然后抛出自定义异常。如果在上游方法中捕获到自定义异常，开发人员就知道异常已经被记录下来，只需要在代码中对其做出适当响应即可。

在 AutoLot.Dal 项目中创建一个名为 Exceptions 的新目录。在该目录中，创建四个新的类文件：CustomException.cs、CustomConcurrencyException.cs、CustomDbUpdateException.cs 和 CustomRetryLimitExceededException.cs。以下列表中显示了全部四个文件：

```csharp
//CustomException.cs
using System;
namespace AutoLot.Dal.Exceptions
{
    public class CustomException : Exception
    {
        public CustomException() {}
        public CustomException(string message) : base(message) { }
        public CustomException(string message, Exception innerException)
            : base(message, innerException) { }
    }
}
```

```csharp
//CustomConcurrencyException.cs
using Microsoft.EntityFrameworkCore;
namespace AutoLot.Dal.Exceptions
{
    public class CustomConcurrencyException : CustomException
    {
        public CustomConcurrencyException() { }
        public CustomConcurrencyException(string message) : base(message) { }
        public CustomConcurrencyException(
            string message, DbUpdateConcurrencyException innerException)
            : base(message, innerException) { }
    }
}
```

```csharp
//CustomDbUpdateException.cs
using Microsoft.EntityFrameworkCore;
namespace AutoLot.Dal.Exceptions
{
    public class CustomDbUpdateException : CustomException
    {
        public CustomDbUpdateException() { }
        public CustomDbUpdateException(string message) : base(message) { }
        public CustomDbUpdateException(
            string message, DbUpdateException innerException)
            : base(message, innerException) { }
    }
}
```

```csharp
//CustomRetryLimitExceededException.cs
using System;
using Microsoft.EntityFrameworkCore.Storage;

namespace AutoLot.Dal.Exceptions
{
    public class CustomRetryLimitExceededException : CustomException
    {
        public CustomRetryLimitExceededException() { }
        public CustomRetryLimitExceededException(string message)
            : base(message) { }
        public CustomRetryLimitExceededException(
            string message, RetryLimitExceededException innerException)
            : base(message, innerException) { }
    }
}
```

> 注 自定义异常处理已在第 7 章中详细讲解。

## 重写 SaveChanges 方法

如上一章所述，基类 DbContext 上的 SaveChanges() 方法会将数据更改、新增和删除持久化到数据库。重写该方法可以使异常处理封装在一个位置。添加自定义异常后，将 AutoLot.Dal.Exceptions using 语句添加到 ApplicationDbContext 类的顶部。接下来，添加以下 SaveChanges() 方法的重写：

```csharp
public override int SaveChanges()
{
    try
    {
        return base.SaveChanges();
    }
    catch (DbUpdateConcurrencyException ex)
    {
        //A concurrency error occurred
        //Should log and handle intelligently
        throw new CustomConcurrencyException("A concurrency error happened.", ex);
    }
    catch (RetryLimitExceededException ex)
    {
        //DbResiliency retry limit exceeded
        //Should log and handle intelligently
        throw new CustomRetryLimitExceededException("There is a problem with SQL Server.", ex);
    }
    catch (DbUpdateException ex)
    {
        //Should log and handle intelligently
        throw new CustomDbUpdateException("An error occurred updating the database", ex);
    }
    catch (Exception ex)
    {
        //Should log and handle intelligently
        throw new CustomException("An error occurred updating the database", ex);
    }
}
```

## 处理 DbContext 和 ChangeTracker 事件

导航到 ApplicationDbContext 的构造函数并添加上一章中讨论的三个 DbContext 事件。

```csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
    base.SavingChanges += (sender, args) =>
    {
        Console.WriteLine($"Saving changes for {((ApplicationDbContext)sender)!.Database!.GetConnectionString()}");
    };
    base.SavedChanges += (sender, args) =>
    {
        Console.WriteLine($"Saved {args!.EntitiesSavedCount} changes for {((ApplicationDbContext)sender)!.Database!.GetConnectionString()}");
    };
    base.SaveChangesFailed += (sender, args) =>
    {
        Console.WriteLine($"An exception occurred! {args.Exception.Message} entities");
    };
}
```

接下来，为 ChangeTracker 的 StateChanged 和 Tracked 事件添加处理程序。

```csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
    ...
    ChangeTracker.Tracked += ChangeTracker_Tracked;
    ChangeTracker.StateChanged += ChangeTracker_StateChanged;
}
```

Tracked 事件参数保存了触发事件的实体的引用，以及它是否来自查询（从数据库加载）或以编程方式添加。在 ApplicationDbContext 中添加以下事件处理程序：

```csharp
private void ChangeTracker_Tracked(object? sender, EntityTrackedEventArgs e)
{
    var source = (e.FromQuery) ? "Database" : "Code";
    if (e.Entry.Entity is Car c)
    {
        Console.WriteLine($"Car entry {c.PetName} was added from {source}");
    }
}
```

StateChanged 事件在跟踪实体的状态发生变化时触发。此事件的一个用途是审计。在以下事件处理程序中，如果实体的 NewState 为 Unchanged，则会检查 OldState 以查看实体是被添加还是被修改。将以下事件处理程序添加到 ApplicationDbContext：

```csharp
private void ChangeTracker_StateChanged(object? sender, EntityStateChangedEventArgs e)
{
    if (e.Entry.Entity is not Car c)
    {
        return;
    }
    var action = string.Empty;
    Console.WriteLine($"Car {c.PetName} was {e.OldState} before the state changed to {e.NewState}");
    switch (e.NewState)
    {
        case EntityState.Unchanged:
            action = e.OldState switch
            {
                EntityState.Added => "Added",
                EntityState.Modified => "Edited",
                _ => action
            };
            Console.WriteLine($"The object was {action}");
            break;
    }
}
```

## 创建迁移并更新数据库

到本章的这一点，两个项目都已编译，我们准备好创建另一个迁移来更新数据库。在 AutoLot.Dal 项目目录中输入以下命令（每个命令必须在一行中输入）：

```bash
dotnet ef migrations add UpdatedEntities -o EfStructures\Migrations -c AutoLot.Dal.EfStructures.ApplicationDbContext
```

```bash
dotnet ef database update UpdatedEntities -c AutoLot.Dal.EfStructures.ApplicationDbContext
```

## 添加数据库视图和存储过程

数据库还剩下两个更改。第一个是添加第 21 章中的 GetPetName 存储过程，第二个是添加一个数据库视图，将 Orders 表与 Customer、Car 和 Make 详细信息组合起来。

## 添加 MigrationHelpers 类

我们使用迁移创建存储过程和视图，这需要手动编码迁移。这样做的原因（而不是直接打开 Azure Data Studio 并运行 T-SQL 代码）是为了将所有数据库配置放在一个单一流程中。当所有内容都包含在迁移中时，对 `dotnet ef database update` 的一次调用即可确保数据库是最新的，包括 EF Core 配置和自定义 SQL。

在没有模型更改时调用 `dotnet migrations add` 命令仍会创建带有空 `Up()` 和 `Down()` 方法的正确时间戳迁移文件。执行以下命令创建空迁移（但不要应用迁移）：

```bash
dotnet ef migrations add SQL -o EfStructures\Migrations -c AutoLot.Dal.EfStructures.ApplicationDbContext
```

现在，在 AutoLot.Dal 项目的 EfStructures 文件夹中添加一个名为 MigrationHelpers.cs 的新文件。添加对 Microsoft.EntityFrameworkCore.Migrations 的 using 语句，使类为 public 且 static，并添加以下方法，这些方法使用 MigrationBuilder 对数据库执行 SQL 语句：

```csharp
namespace AutoLot.Dal.EfStructures
{
    public static class MigrationHelpers
    {
        public static void CreateSproc(MigrationBuilder migrationBuilder)
        {
            migrationBuilder.Sql($@"
                exec (N'
                CREATE PROCEDURE [dbo].[GetPetName]
                    @carID int,
                    @petName nvarchar(50) output
                AS
                    SELECT @petName = PetName from dbo.Inventory where Id = @carID
                ')");
        }
        public static void DropSproc(MigrationBuilder migrationBuilder)
        {
            migrationBuilder.Sql("DROP PROCEDURE [dbo].[GetPetName]");
        }

        public static void CreateCustomerOrderView(MigrationBuilder migrationBuilder)
        {
            migrationBuilder.Sql($@"
                exec (N'
                CREATE VIEW [dbo].[CustomerOrderView]
                AS
```



```
SELECT dbo.Customers.FirstName, dbo.Customers.LastName,
    dbo.Inventory.Color, dbo.Inventory.PetName, dbo.Inventory.IsDrivable,
    dbo.Makes.Name AS Make
FROM  dbo.Orders
INNER JOIN dbo.Customers ON dbo.Orders.CustomerId = dbo.Customers.Id
INNER JOIN dbo.Inventory ON dbo.Orders.CarId = dbo.Inventory.Id
INNER JOIN dbo.Makes ON dbo.Makes.Id = dbo.Inventory.MakeId
')');
}
public static void DropCustomerOrderView(MigrationBuilder migrationBuilder)
{
    migrationBuilder.Sql("EXEC (N' DROP VIEW [dbo].[CustomerOrderView] '");
}
}
}
```

## 更新并应用迁移

MigrationHelpers 类为每个 SQL Server 对象提供了两种方法：一种用于创建对象，另一种用于删除对象。回想一下，当迁移被应用时，Up() 方法会被执行，而当迁移被回滚时，Down() 方法会被执行。创建静态方法进入迁移的 Up() 方法中，而删除方法进入迁移的 Down() 方法中。当此迁移被应用时，两个 SQL Server 对象将被创建；当迁移被回滚时，两个 SQL Server 对象将被删除。以下是更新后的迁移代码清单：

```
namespace AutoLot.Dal.EfStructures.Migrations
{
    public partial class SQL : Migration
    {
        protected override void Up(MigrationBuilder migrationBuilder)
        {
            MigrationHelpers.CreateSproc(migrationBuilder);
            MigrationHelpers.CreateCustomerOrderView(migrationBuilder);
        }

        protected override void Down(MigrationBuilder migrationBuilder)
        {
            MigrationHelpers.DropSproc(migrationBuilder);
            MigrationHelpers.DropCustomerOrderView(migrationBuilder);
        }
    }
}
```

如果您为了运行初始迁移而删除了数据库，您可以应用此迁移并继续。执行以下命令来应用迁移：

```
dotnet ef database update -c AutoLot.Dal.EfStructures.ApplicationDbContext
```

如果您没有为第一次迁移删除数据库，则该存储过程已存在且无法创建。简单的解决方法是在 Up() 方法中注释掉创建存储过程的调用，如下所示：

```
protected override void Up(MigrationBuilder migrationBuilder)
{
// MigrationHelpers.CreateSproc(migrationBuilder);
MigrationHelpers.CreateCustomerOrderView(migrationBuilder);
}
```

在您第一次应用此迁移后，取消注释该行，一切将正常处理。当然，另一个选择是从数据库中删除存储过程然后应用迁移。这破坏了“更新一处”的范式，但它是数据库优先到代码优先过渡的一部分。

> 注 您也可以编写代码先检查对象是否存在，如果已存在则删除它，但我认为对于可能永远不会发生的问题来说这有点过度。

## 添加视图模型

现在 SQL Server 视图已就位，是时候创建将用于显示视图数据的视图模型了。视图模型将作为无键 DbSet<T> 添加。这样做的优点是，可以使用适用于所有 DbSet<T> 集合的常规 LINQ 过程来查询数据。

## 添加视图模型

在 AutoLot.Models 项目中添加一个名为 ViewModels 的新文件夹。在此文件夹中，添加一个名为 CustomerOrderViewModel.cs 的类，并在文件中添加以下 using 语句：

```
using System.ComponentModel.DataAnnotations.Schema;
using Microsoft.EntityFrameworkCore;
```

接下来，将代码更新为如下所示：

```
namespace AutoLot.Models.ViewModels
{
[Keyless]
public class CustomerOrderViewModel
{
public string? FirstName { get; set; }
public string? LastName { get; set; }
public string? Color { get; set; }
public string? PetName { get; set; }
public string? Make { get; set; }
public bool? IsDrivable { get;set; }
[NotMapped]
public string FullDetail =>
$"{FirstName} {LastName} ordered a {Color} {Make} named {PetName}";
```

```
public override string ToString() => FullDetail;
}
```

KeyLess 数据注释表示这是一个与没有主键的数据一起工作且可被优化为只读数据（从数据库角度来看）的实体。前五个属性代表来自视图的数据。FullDetail 属性用 NotMapped 数据注释进行装饰。这告知 EF Core 该属性不应包含在数据库中，也不是由于查询操作而来自数据库。ToString() 重写也会被 EF Core 忽略。

## 将视图模型添加到 ApplicationDbContext

最后一步是在 ApplicationDbContext 中注册并配置 CustomerOrderViewModel。向 ApplicationDbContext 添加 AutoLot.Models.ViewModels 的 using 语句，然后添加 DbSet<T> 属性。

```
public virtual DbSet<CustomerOrderViewModel>? CustomerOrderViewModels { get; set; }
```

除了添加 DbSet<T> 实例之外，Fluent API 还将视图模型映射到 SQL Server 视图。HasNoKey() Fluent API 方法和 Keyless 数据注释实现相同的功能，其中 Fluent API 方法会覆盖数据注释。我更喜欢保留数据注释以提高清晰度。将以下内容添加到 OnModelCreating() 方法中：

```
modelBuilder.Entity<CustomerOrderViewModel>(entity =>
{
    entity.HasNoKey().ToView("CustomerOrderView","dbo");
});
```

## 添加存储库

常见的数据访问设计模式是存储库模式。如 Martin Fowler 所描述的 [www.martinfowler.com/eaaCatalog/repository.html](http://www.martinfowler.com/eaaCatalog/repository.html)，该模式的核心是充当领域和数据映射层之间的中介。拥有一个包含通用数据访问代码的通用基础存储库有助于消除代码重复。拥有从基础存储库派生的特定存储库和接口也与 ASP.NET Core 中的依赖注入框架配合良好。

AutoLot 数据访问层中的每个领域实体都将拥有一个强类型的存储库来封装所有数据访问工作。首先，在 AutoLot.DAL 项目中创建一个名为 Repos 的文件夹来存放所有类。

> 注 本节并非旨在也不声称是 Mr. Fowler 设计模式的字面解释。如果您对促成此版本的原始模式感兴趣，可以在 www.martinfowler.com/eaaCatalog/repository.html 上找到有关存储库模式的更多信息。

## 添加 IRepo 基础接口

IRepo 基础接口公开了数据访问中使用的许多通用方法。在 AutoLot.Dal 项目中添加一个名为 Repos 的新文件夹，并在该文件夹中创建一个名为 Base 的新文件夹。在 Repos\Base 文件夹中添加一个名为 IRepo 的新接口。将 using 语句更新为如下所示：

```
using System;
using System.Collections.Generic;
```

完整的接口如下所示：

```
namespace AutoLot.Dal.Repos.Base
{
    public interface IRepo<T>: IDisposable
    {
        int Add(T entity, bool persist = true);
        int AddRange(IEnumerable<T> entities, bool persist = true);
        int Update(T entity, bool persist = true);
        int UpdateRange(IEnumerable<T> entities, bool persist = true);
        int Delete(int id, byte[] timeStamp, bool persist = true);
        int Delete(T entity, bool persist = true);
        int DeleteRange(IEnumerable<T> entities, bool persist = true);
        T? Find(int? id);
        T? FindAsNoTracking(int id);
        T? FindIgnoreQueryFilters(int id);
        IEnumerable<T> GetAll();
        IEnumerable<T> GetAllIgnoreQueryFilters();
        void ExecuteQuery(string sql, object[] sqlParametersObjects);
        int SaveChanges();
    }
}
```

## 添加 BaseRepo

接下来，在 Repos\Base 目录中添加一个名为 BaseRepo 的类。此类将实现 IRepo 接口并为类型特定的存储库提供核心功能（即将到来）。将 using 语句更新为如下所示：

```
using System;
using System.Collections.Generic;
using System.Linq;
using AutoLot.Dal.EfStructures;
using AutoLot.Dal.Exceptions;
using AutoLot.Models.Entities.Base;
using Microsoft.EntityFrameworkCore;
```

使类泛型化，类型为 T，并将类型约束为 BaseEntity 和 new()，这将限制类型为具有无参数构造函数的类。实现 IRepo<T> 接口，如下所示：

```
public abstract class BaseRepo<T> : IRepo<T> where T : BaseEntity, new()
```





The repo needs an instance of the ApplicationDbContext injected into a constructor. When used with the ASP.NET Core DI container, the container will handle the lifetime of the context. A second constructor will accept DbContextOptions and need to create an instance of the ApplicationDbContext. That context will need to be disposed. Because this class is abstract, both constructors are protected. Add the following code for the public ApplicationDbContext, the two constructors, and the Dispose pattern:

```
private readonly bool _disposeContext;
public ApplicationDbContext Context { get; }

protected BaseRepo(ApplicationDbContext context)
{
    Context = context;
    _disposeContext = false;
}

protected BaseRepo(DbContextOptions<ApplicationDbContext> options) : this(new ApplicationDbContext(options))
{
    _disposeContext = true;
}

public void Dispose()
{
    Dispose(true);
    GC.SuppressFinalize(this);
}
private bool _isDisposed;
protected virtual void Dispose(bool disposing)
{
    if (_isDisposed)
    {
        return;
    }

    if (disposing)
    {
        if (_disposeContext)
        {
            Context.Dispose();
        }
    }
    _isDisposed = true;
}

~BaseRepo()
{
    Dispose(false);
}
```

The DbSet<T> properties of ApplicationDbContext can be referenced by using the Context.Set<T>() method. Create a public property named Table of type DbSet<T> and set the value in the initial constructor, like this:

```
public DbSet<T> Table { get; }
protected BaseRepo(ApplicationDbContext context)
{
    Context = context;
    Table = Context.Set<T>();
    _disposeContext = false;
}
```

## Implement the SaveChanges Method

The BaseRepo has a SaveChanges() that calls into the overridden SaveChanges() method that demonstrates the custom exception pattern. Add the following code to the BaseRepo class:

```
public int SaveChanges()
{
    try
    {
        return Context.SaveChanges();
    }
    catch (CustomException ex)
    {
        //Should handle intelligently - already logged
        throw;
    }
    catch (Exception ex)
    {
        //Should log and handle intelligently
        throw new CustomException("An error occurred updating the database", ex);
    }
}
```

## Implement the Common Read Methods

The next series of methods returns records using LINQ statements. The Find() method takes the primary key value(s) and searches the ChangeTracker first. If the entity is already being tracked, the tracked instance is returned. If not, the record is retrieved from the database.

```
public virtual T? Find(int? id) => Table.Find(id);
```

The two additional Find() methods extend the Find() base method. The next method demonstrates retrieving a record but not adding it to the ChangeTracker using AsNoTrackingWithIdentityResolution(). Add the following code to the class:

```
public virtual T? FindAsNoTracking(int id) =>
    Table.AsNoTrackingWithIdentityResolution().FirstOrDefault(x => x.Id == id);
```

The next variation removes the query filters from the entity and then uses the shorthand version (skipping the Where() method) to get FirstOrDefault(). Add the following to the class:

```
public T? FindIgnoreQueryFilters(int id) =>
    Table.IgnoreQueryFilters().FirstOrDefault(x => x.Id == id);
```

The GetAll() methods return all of the records from the table. The first retrieves them in database order, and the second takes turns of any query filters.

```
public virtual IEnumerable<T> GetAll() => Table;
public virtual IEnumerable<T> GetAllIgnoreQueryFilters()
    => Table.IgnoreQueryFilters();
```

The ExecuteQuery() method is there to execute stored procedures:

```
public void ExecuteQuery(string sql, object[] sqlParametersObjects)
    => Context.Database.ExecuteSqlRaw(sql, sqlParametersObjects);
```

## The Add, Update, and Delete Methods

The next block of code to be added wraps the matching Add(), Update(), and Remove() methods on the specific DbSet<T> property. The persist parameter determines whether the repo executes SaveChanges() immediately when the Add()/Update()/Remove() repository methods are called. All of the methods are marked virtual to allow for downstream overriding. Add the following code to your class:

```
public virtual int Add(T entity, bool persist = true)
{
    Table.Add(entity);
    return persist ? SaveChanges() : 0;
}
public virtual int AddRange(IEnumerable<T> entities, bool persist = true)
{
    Table.AddRange(entities);
    return persist ? SaveChanges() : 0;
}
public virtual int Update(T entity, bool persist = true)
{
    Table.Update(entity);
    return persist ? SaveChanges() : 0;
}
public virtual int UpdateRange(IEnumerable<T> entities, bool persist = true)
{
    Table.UpdateRange(entities);
    return persist ? SaveChanges() : 0;
}
public virtual int Delete(T entity, bool persist = true)
{
    Table.Remove(entity);
    return persist ? SaveChanges() : 0;
}
public virtual int DeleteRange(IEnumerable<T> entities, bool persist = true)
{
    Table.RemoveRange(entities);
    return persist ? SaveChanges() : 0;
}
```

There is one more Delete() method that doesn’t follow the same pattern. This method uses EntityState to conduct the delete operation, which is used fairly often in ASP.NET Core operations to cut down on the network traffic. It is listed here:

```
public int Delete(int id, byte[] timeStamp, bool persist = true)
{
    var entity = new T {Id = id, TimeStamp = timeStamp};
    Context.Entry(entity).State = EntityState.Deleted;
    return persist ? SaveChanges() : 0;
}
```

This concludes the BaseRepo class, and now it’s time to build the entity-specific repos.

## Entity-Specific Repo Interfaces

Each entity will have a strongly typed repository derived from BaseRepo<T> and an interface that implements IRepo<T>. Add a new folder named Interfaces under the Repos directory in the AutoLot.Dal project. In this new directory, add five interfaces.

- ICarRepo.cs
- ICreditRiskRepo.cs
- ICustomerRepo.cs
- IMakeIRepo.cs
- IOrderRepo.cs

The next sections complete the interfaces.

## The Car Repository Interface

Open the ICarRepo.cs interface. Add the following using statements to the top of the file:

```
using System.Collections.Generic;
using AutoLot.Models.Entities;
using AutoLot.Dal.Repos.Base;
```

Change the interface to public and implement IRepo<Category> as follows:

```
namespace AutoLot.Dal.Repos.Interfaces
{
    public interface ICarRepo : IRepo<Car>
    {
        IEnumerable<Car> GetAllBy(int makeId);
        string GetPetName(int id);
    }
}
```

## The Credit Risk Interface

Open the ICreditRiskRepo.cs interface. This interface doesn’t add any functionality beyond what is provided in the BaseRepo. Update the code to the following:

```
using AutoLot.Models.Entities;
using AutoLot.Dal.Repos.Base;
namespace AutoLot.Dal.Repos.Interfaces
{
    public interface ICreditRiskRepo : IRepo<CreditRisk>
    {
    }
}
```

## The Customer Repository Interface

Open the ICustomerRepo.cs interface. This interface doesn’t add any functionality beyond what is provided in the BaseRepo. Update the code to the following:

```
using AutoLot.Models.Entities;
using AutoLot.Dal.Repos.Base;
namespace AutoLot.Dal.Repos.Interfaces
{
    public interface ICustomerRepo : IRepo<Customer>
    {
    }
}
```

## The Make Repository Interface

Open the IMakeRepo.cs interface. This interface doesn’t add any functionality beyond what is provided in the BaseRepo. Update the code to the following:

```
using AutoLot.Models.Entities;
using AutoLot.Dal.Repos.Base;
namespace AutoLot.Dal.Repos.Interfaces
{
    public interface IMakeRepo : IRepo<Make>
    {
    }
}
```

## The Order Repository Interface

Open the IOrderRepo.cs interface. Add the following using statements to the top of the file:

```
using System.Collections.Generic;
using System.Linq;
using AutoLot.Models.Entities;
using AutoLot.Dal.Repos.Base;
using AutoLot.Models.ViewModels;
```

Change the interface to public and implement IRepo<Order> as follows:

```
namespace AutoLot.Dal.Repos.Interfaces
{
    public interface IOrderRepo : IRepo<Order>
    {
        IQueryable<CustomerOrderViewModel> GetOrdersViewModel();
    }
}
```

This completes the interface as all of the necessary API endpoints are covered in the base class.

## Implement the Entity-Specific Repositories

The implemented repositories gain most of their functionality from the base class. This section covers the functionality added to or overridden from the base repository. In the Repos directory of the AutoLot.Dal project, add the five repo classes.

- CarRepo.cs
- CreditRiskRepo.cs
- CustomerRepo.cs
- MakeRepo.cs
- OrderRepo.cs

The next sections complete the repositories.

## The Car Repository

Open the CarRepo.cs class and add the following using statements to the top of the file:

```
using System.Collections.Generic;
using System.Data;
using System.Linq;
using AutoLot.Dal.EfStructures;
using AutoLot.Models.Entities;
using AutoLot.Dal.Repos.Base;
using AutoLot.Dal.Repos.Interfaces;
using Microsoft.Data.SqlClient;
using Microsoft.EntityFrameworkCore;
```

Change the class to public, inherit from BaseRepo<Car>, and implement ICarRepo.

```
namespace AutoLot.Dal.Repos
{
    public class CarRepo : BaseRepo<Car>, ICarRepo
    {
    }
}
```




每个仓库都必须实现 BaseRepo 中的两个构造函数。

```
public CarRepo(ApplicationDbContext context) : base(context)
{
}
internal CarRepo(DbContextOptions<ApplicationDbContext> options) : base(options)
{
}
```

为 GetAll() 和 GetAllIgnoreQueryFilters() 添加重写，以包含 MakeNavigation 属性并按 PetName 值排序。

```
public override IEnumerable<Car> GetAll()
    => Table
        .Include(c => c.MakeNavigation)
        .OrderBy(o => o.PetName);
```

```
public override IEnumerable<Car> GetAllIgnoreQueryFilters()
    => Table
        .Include(c => c.MakeNavigation)
        .OrderBy(o => o.PetName)
        .IgnoreQueryFilters();
```

实现 GetAllBy() 方法。此方法必须在执行前在上下文上设置查询过滤器。包含 Make 导航属性并按 PetName 值排序。

```
public IEnumerable<Car> GetAllBy(int makeId)
{
    return Table
        .Where(x => x.MakeId == makeId)
        .Include(c => c.MakeNavigation)
        .OrderBy(c => c.PetName);
}
```

为 Find() 添加重写以包含 MakeNavigation 属性并忽略查询过滤器。

```
public override Car? Find(int? id)
    => Table
        .IgnoreQueryFilters()
        .Where(x => x.Id == id)
        .Include(m => m.MakeNavigation)
        .FirstOrDefault();
```

添加使用存储过程获取汽车 PetName 值的方法。

```
public string GetPetName(int id)
{
    var parameterId = new SqlParameter
    {
        ParameterName = "@carId",
        SqlDbType = SqlDbType.Int,
        Value = id,
    };
```

```
var parameterName = new SqlParameter
{
    ParameterName = "@petName",
    SqlDbType = SqlDbType.NVarChar,
    Size = 50,
    Direction = ParameterDirection.Output
};

_ = Context.Database
    .ExecuteSqlRaw("EXEC [dbo].[GetPetName] @carId, @petName OUTPUT",parameterId,
        parameterName);
return (string)parameterName.Value;
}
```

## CreditRisk 仓库

打开 CreditRiskRepo.cs 类并在文件顶部添加以下 using 语句：

```
using AutoLot.Dal.EfStructures;
using AutoLot.Dal.Models.Entities;
using AutoLot.Dal.Repos.Base;
using AutoLot.Dal.Repos.Interfaces;
using Microsoft.EntityFrameworkCore;
```

将类改为 public，从 BaseRepo<CreditRisk> 继承，实现 ICreditRiskRepo，并添加两个必需的构造函数。

```
namespace AutoLot.Dal.Repos
{
    public class CreditRiskRepo : BaseRepo<CreditRisk>, ICreditRiskRepo
    {
        public CreditRiskRepo(ApplicationDbContext context) : base(context)
        {
        }
        internal CreditRiskRepo(
            DbContextOptions<ApplicationDbContext> options)
        : base(options)
        {
        }
    }
}
```

## Customer 仓库

打开 CustomerRepo.cs 类并在文件顶部添加以下 using 语句：

```
using System.Collections.Generic;
using System.Linq;
using AutoLot.Dal.EfStructures;
using AutoLot.Dal.Models.Entities;
using AutoLot.Dal.Repos.Base;
```

```
using AutoLot.Dal.Repos.Interfaces;
using Microsoft.EntityFrameworkCore;
```

将类改为 public，从 BaseRepo<Customer> 继承，实现 ICustomerRepo，并添加两个必需的构造函数。

```
namespace AutoLot.Dal.Repos
{
    public class CustomerRepo : BaseRepo<Customer>, ICustomerRepo
    {
        public CustomerRepo(ApplicationDbContext context)
            : base(context)
        {
        }
        internal CustomerRepo(
            DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }
    }
}
```

最后一步是添加返回所有 Customer 记录及其订单并按 LastName 排序的方法。将以下方法添加到类中：

```
public override IEnumerable<Customer> GetAll()
    => Table
        .Include(c => c.Orders)
        .OrderBy(o => o.PersonalInformation.LastName);
```

## Make 仓库

打开 MakeRepo.cs 类并在文件顶部添加以下 using 语句：

```
using System.Collections.Generic;
using System.Linq;
using AutoLot.Dal.EfStructures;
using AutoLot.Dal.Models.Entities;
using AutoLot.Dal.Repos.Base;
using AutoLot.Dal.Repos.Interfaces;
using Microsoft.EntityFrameworkCore;
```

将类改为 public，从 BaseRepo<Make> 继承，实现 IMakeRepo，并添加两个必需的构造函数。

```
namespace AutoLot.Dal.Repos
{
    public class MakeRepo : BaseRepo<Make>, IMakeRepo
    {
        public MakeRepo(ApplicationDbContext context)
            : base(context)
```

```
{
}

internal MakeRepo(
    DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
}
```

最后需要重写的是 GetAll() 方法，按名称对 Make 值排序。

```
public override IEnumerable<Make> GetAll()
    => Table.OrderBy(m => m.Name);
public override IEnumerable<Make> GetAllIgnoreQueryFilters()
    => Table.IgnoreQueryFilters().OrderBy(m => m.Name);
```

## Order 仓库

打开 OrderRepo.cs 类并在文件顶部添加以下 using 语句：

```
using AutoLot.Dal.EfStructures;
using AutoLot.Dal.Models.Entities;
using AutoLot.Dal.Repos.Base;
using AutoLot.Dal.Repos.Interfaces;
using Microsoft.EntityFrameworkCore;
```

将类改为 public，从 BaseRepo<Order> 继承，并实现 IOrderRepo。

```
namespace AutoLot.Dal.Repos
{
    public class OrderRepo : BaseRepo<Order>, IOrderRepo
    {
        public OrderRepo(ApplicationDbContext context)
            : base(context)
        {
        }

        internal OrderRepo(
            DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }
    }
}
```

最后要实现的方法是 GetOrderViewModel() 方法，它从数据库视图返回 IQueryable<CustomerOrderViewModel>。

```
public IQueryable<CustomerOrderViewModel> GetOrdersViewModel()
{
    return Context.CustomerOrderViewModels!.AsQueryable();
}
```

至此所有仓库都已完成。下一节将创建用于删除、创建和填充数据库的代码。

## 程序化数据库和迁移处理

DbContext 的 Database 属性提供了程序化方法来删除和创建数据库以及运行所有迁移。表 23-1 描述了与这些操作相关的成员。

表 23-1. 程序化操作数据库

| Database 的成员 | 意义 |
|---|---|
| EnsureDeleted | 如果数据库存在则删除它。如果不存在则不执行任何操作。 |
| EnsureCreated | 如果数据库不存在则创建它。如果已存在则不执行任何操作。基于可从 DbSet<T> 属性访问的类创建表和列。不会应用任何迁移。注意：此方法不应与迁移一起使用。 |
| Migrate | 如果数据库不存在则创建它。向数据库应用所有迁移。 |

如表中所述，EnsureCreated() 方法会在数据库不存在时创建数据库，然后根据实体模型创建表、列和索引。它不会应用任何迁移。如果您使用迁移（如我们所做的），这将在操作数据库时导致错误，您将不得不欺骗 EF Core（像我们之前做的那样）使其相信迁移已被应用。您还必须手动将任何自定义 SQL 对象应用到数据库。当您使用迁移时，始终使用 Migrate() 方法以程序化方式创建数据库，而不是 EnsureCreated() 方法。

## 删除、创建和清理数据库

在开发期间，删除并重新创建开发数据库然后用示例数据填充它是有益的。这创建了一个环境，可以在其中执行测试（手动或自动）而不用担心由于更改数据而破坏其他测试。在 AutoLot.Dal 项目中创建一个名为 Initialization 的新文件夹。在此文件夹中，创建一个名为 SampleDataInitializer.cs 的新类。在文件顶部，将 using 语句更新为以下内容：

```
using System;
using System.Collections.Generic;
using System.Linq;
using AutoLot.Dal.EfStructures;
using AutoLot.Models.Entities;
using AutoLot.Models.Entities.Base;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Storage;
```



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




## 配置项目

为了在运行时获取连接字符串，我们将使用 .NET Core 的配置功能，通过 JSON 文件进行配置。在项目中添加一个名为 `appsettings.json` 的 JSON 文件，并按以下格式将连接字符串信息添加到文件中（根据需要修改此处列出的连接字符串）：

```json
{
    "ConnectionStrings": {
        "AutoLot": "server=.,5433;Database=AutoLotFinal;User Id=sa;Password=P@ssw0rd;"
    }
}
```

更新项目文件，使设置文件在每次生成时都复制到输出文件夹。为此，请将以下 `ItemGroup` 添加到 `AutoLot.Dal.Tests.csproj` 文件中：

```xml
<ItemGroup>
    <None Update="appsettings.json">
        <CopyToOutputDirectory>Always</CopyToOutputDirectory>
    </None>
</ItemGroup>
```

## 创建测试辅助类

`TestHelper` 类负责处理应用程序配置，并创建 `ApplicationDbContext` 的新实例。在项目根目录中添加一个名为 `TestHelpers.cs` 的新公共静态类。将 using 语句更新为以下内容：

```csharp
using System.IO;
using AutoLot.Dal.EfStructures;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Storage;
using Microsoft.Extensions.Configuration;

namespace AutoLot.Dal.Tests
{
    public static class TestHelpers
    {
    }
}
```

添加两个公共静态方法，分别用于创建 `IConfiguration` 和 `ApplicationDbContext` 类的实例。向类中添加以下代码：

```csharp
public static IConfiguration GetConfiguration() =>
    new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json", true, true)
        .Build();
```

```csharp
public static ApplicationDbContext GetContext(IConfiguration configuration)
{
    var optionsBuilder = new DbContextOptionsBuilder<ApplicationDbContext>();
    var connectionString = configuration.GetConnectionString("AutoLot");
    optionsBuilder.UseSqlServer(connectionString, sqlOptions => sqlOptions.EnableRetryOnFailure());
    return new ApplicationDbContext(optionsBuilder.Options);
}
```

请注意对 `EnableRetryOnFailure()` 的调用（原文中为粗体）。提醒一下，该调用启用了 SQL Server 重试执行策略，会自动重试因暂时性错误而失败的操作。

再添加一个静态方法，使用与传入的原始实例相同的连接和事务创建新的 `ApplicationDbContext` 实例。此方法展示了如何基于现有实例创建 `ApplicationDbContext`，以共享连接和事务。

```csharp
public static ApplicationDbContext GetSecondContext(
    ApplicationDbContext oldContext,
    IDbContextTransaction trans)
{
    var optionsBuilder = new DbContextOptionsBuilder<ApplicationDbContext>();
    optionsBuilder.UseSqlServer(
        oldContext.Database.GetDbConnection(),
        sqlServerOptions => sqlServerOptions.EnableRetryOnFailure());
    var context = new ApplicationDbContext(optionsBuilder.Options);
    context.Database.UseTransaction(trans.GetDbTransaction());
    return context;
}
```

## 添加 BaseTest 类

现在，在项目中添加一个名为 `Base` 的新文件夹，并在该文件夹中添加一个名为 `BaseTest.cs` 的新类文件。将 using 语句更新为以下内容：

```csharp
using System;
using System.Data;
using AutoLot.Dal.EfStructures;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Storage;
using Microsoft.Extensions.Configuration;
```

将该类声明为抽象类并实现 `IDisposable`。添加两个受保护的只读属性，用于保存 `IConfiguration` 和 `ApplicationDbContext` 实例，并在虚拟 `Dispose()` 方法中释放 `ApplicationDbContext` 实例。

```csharp
namespace AutoLot.Dal.Tests.Base
{
    public abstract class BaseTest : IDisposable
    {
        protected readonly IConfiguration Configuration;
        protected readonly ApplicationDbContext Context;

        public virtual void Dispose()
        {
            Context.Dispose();
        }
    }
}
```

xUnit 测试框架提供了一种机制，可在每次测试执行前后运行代码。实现 `IDisposable` 接口的测试类（称为 fixture）会在每次测试运行前执行类构造函数中的代码（本例中包括基类和派生类的构造函数），这也称为测试准备；每次测试运行后，则会执行 `Dispose` 方法中的代码（包括派生类和基类中的实现），这也称为测试清理。

添加一个受保护的构造函数，用于创建 `IConfiguration` 实例并将其赋给受保护的类变量。使用该配置，通过 `TestHelpers` 类创建 `ApplicationDbContext` 实例，并同样将其赋给受保护的类变量。

```csharp
protected BaseTest()
{
    Configuration = TestHelpers.GetConfiguration();
    Context = TestHelpers.GetContext(Configuration);
}
```

## 添加事务测试执行辅助方法

`BaseTest` 类中的最后两个方法用于在事务中运行测试方法。这些方法接收一个 `Action` 委托作为唯一参数，创建显式事务（或加入现有事务），执行 `Action` 委托，然后回滚事务。这样，任何创建、更新或删除测试执行后，数据库都能恢复到测试运行前的状态。由于 `ApplicationDbContext` 配置为在发生暂时性错误时启用重试，因此整个过程必须通过 `ApplicationDbContext` 的执行策略运行。

`ExecuteInATransaction()` 使用单个 `ApplicationDbContext` 实例执行操作。`ExecuteInASharedTransaction()` 方法允许多个 `ApplicationDbContext` 实例共享一个事务。本章稍后会详细介绍这些方法。现在，先将以下代码添加到 `BaseTest` 类中：

```csharp
protected void ExecuteInATransaction(Action actionToExecute)
{
    var strategy = Context.Database.CreateExecutionStrategy();
    strategy.Execute(() =>
    {
        using var trans = Context.Database.BeginTransaction();
        actionToExecute();
        trans.Rollback();
    });
}
```

```csharp
protected void ExecuteInASharedTransaction(Action<IDbContextTransaction> actionToExecute)
{
    var strategy = Context.Database.CreateExecutionStrategy();
    strategy.Execute(() =>
    {
        using IDbContextTransaction trans =
            Context.Database.BeginTransaction(IsolationLevel.ReadUncommitted);
        actionToExecute(trans);
        trans.Rollback();
    });
}
```

## 添加 EnsureAutoLotDatabase 测试 fixture 类

xUnit 测试框架提供了一种机制，可在所有测试运行前运行代码（称为 fixture 初始化），并在所有测试运行后运行代码（称为 fixture 清理）。通常不建议采用这种做法，但在本例中，我们希望确保数据库在任何测试运行前就已创建并加载数据，而不是在每次测试运行前都执行此操作。实现 `IClassFixture<T>`（其中 `T: TestFixtureClass`）的测试类会在任何测试运行前执行 `T`（即 `TestFixtureClass`）的构造函数代码，并在所有测试完成后执行 `Dispose()` 代码。

在 `Base` 目录中添加一个名为 `EnsureAutoLotDatabaseTestFixture.cs` 的新类，并实现 `IDisposable`。将该类声明为公共密封类，并添加以下 using 语句：

```csharp
using System;
using AutoLot.Dal.Initialization;
```

```csharp
namespace AutoLot.Dal.Tests.Base
{
    public sealed class EnsureAutoLotDatabaseTestFixture : IDisposable
    {
    }
}
```

构造函数代码会创建一个 `IConfiguration` 实例，然后使用该实例创建 `ApplicationDbContext`。接下来，它会调用 `SampleDataInitializer` 中的 `ClearAndReseedDatabase()` 方法。最后一行会释放上下文实例。在我们的示例中，`Dispose()` 方法不需要执行任何操作（但必须保留该方法以满足 `IDisposable` 接口的要求）。下面列出了构造函数和 `Dispose()` 方法：

```csharp
public EnsureAutoLotDatabaseTestFixture()
{
    var configuration = TestHelpers.GetConfiguration();
    var context = TestHelpers.GetContext(configuration);
    SampleDataInitializer.ClearAndReseedDatabase(context);
    context.Dispose();
}
```

```csharp
public void Dispose()
{
}
```





# 添加集成测试类

下一步是添加将保存自动化测试的类。这些类被称为测试夹具。在 AutoLot.DAL.Tests 文件夹中添加一个名为 IntegrationTests 的新文件夹，并在该文件夹中添加四个文件 CarTests.cs、CustomerTests.cs、MakeTests.cs 和 OrderTests.cs。

根据测试运行器的功能，xUnit 测试在测试夹具（类）内按串行运行，但在所有测试夹具（类）之间并行运行。当执行与数据库交互的集成测试时，这可能会有问题，因为测试正在与单个数据库交互。可以通过将它们放入同一个测试集合中来将执行更改为在测试夹具之间串行。测试集合使用 Collection 属性在类上按名称定义。将以下 Collection 属性添加到所有四个类的顶部：

```
[Collection("Integration Tests")]
```

接下来，从 BaseTest 继承并在两个类中实现 IClassFixture 接口。更新每个类的 using 语句以匹配以下内容：

```
//CarTests.cs
using System.Collections.Generic;
using System.Linq;
using AutoLot.Dal.Exceptions;
using AutoLot.Dal.Repos;
using AutoLot.Dal.Tests.Base;
using AutoLot.Models.Entities;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.ChangeTracking;
using Microsoft.EntityFrameworkCore.Query;
using Microsoft.EntityFrameworkCore.Storage;
using Xunit;
namespace AutoLot.Dal.Tests.IntegrationTests
{
    [Collection("Integration Tests")]
    public class CarTests : BaseTest, IClassFixture<EnsureAutoLotDatabaseTestFixture>
    {
    }
}
```

```
//CustomerTests.cs
using System.Collections.Generic;
using System;
using System.Linq;
using System.Linq.Expressions;
using AutoLot.Dal.Tests.Base;
using AutoLot.Models.Entities;
using Microsoft.EntityFrameworkCore;
using Xunit;
namespace AutoLot.Dal.Tests.IntegrationTests
{
    [Collection("Integration Tests")]
    public class CustomerTests : BaseTest, IClassFixture<EnsureAutoLotDatabaseTestFixture>
    {
    }
}
```

```
//MakeTests.cs
using System.Linq;
using AutoLot.Dal.Repos;
using AutoLot.Dal.Repos.Interfaces;
using AutoLot.Dal.Tests.Base;
using AutoLot.Models.Entities;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.ChangeTracking;
using Xunit;
namespace AutoLot.Dal.Tests.IntegrationTests
{
    [Collection("Integration Tests")]
    public class MakeTests : BaseTest, IClassFixture<EnsureAutoLotDatabaseTestFixture>
    {
    }
}
```

```
//OrderTests.cs
using System.Linq;
using AutoLot.Dal.Repos;
using AutoLot.Dal.Repos.Interfaces;
using AutoLot.Dal.Tests.Base;
using Microsoft.EntityFrameworkCore;
using Xunit;
namespace AutoLot.Dal.Tests.IntegrationTests
{
    [Collection("Integration Tests")]
    public class OrderTests : BaseTest, IClassFixture<EnsureAutoLotDatabaseTestFixture>
    {
    }
}
```

对于 MakeTests 类，添加一个构造函数来创建 MakeRepo 的实例并将其分配给私有只读类级变量。重写 Dispose() 方法，并在该方法中释放 repo。

```
[Collection("Integration Tests")]
public class MakeTests : BaseTest, IClassFixture<EnsureAutoLotDatabaseTestFixture>
{
    private readonly IMakeRepo _repo;
    public MakeTests()
    {
        _repo = new MakeRepo(Context);
    }
    public override void Dispose()
    {
        _repo.Dispose();
    }
    ...
}
```

对 OrderTests 类重复此操作，使用 OrderRepo 代替 MakeRepo。

```
[Collection("Integration Tests")]
public class OrderTests : BaseTest, IClassFixture<EnsureAutoLotDatabaseTestFixture>
{
    private readonly IOrderRepo _repo;
    public OrderTests()
    {
        _repo = new OrderRepo(Context);
    }
    public override void Dispose()
    {
        _repo.Dispose();
    }
    ...
}
```

# Fact 和 Theory 测试方法

无参数的测试方法被称为事实（并使用 Fact 属性）。带参数的测试被称为理论（并使用 Theory 属性），并且可以使用不同的值传递给测试方法作为参数来运行多次迭代。为了演示这些测试类型，在 AutoLot.Dal.Tests 项目中创建一个名为 SampleTests.cs 的新类。将 using 语句更新为以下内容：

```
using Xunit;

namespace AutoLot.Dal.Tests
{
    public class SampleTests
    {
    }
}
```

要创建的第一个测试是 Fact 测试。对于 Fact 测试，所有值都包含在测试方法中。以下（平凡的）示例测试 3+2=5：

```
[Fact]
public void SimpleFactTest()
{
    Assert.Equal(5,3+2);
}
```

在使用 Theory 类型测试时，测试的值会传递给测试方法。值可以来自 InlineData 属性、方法或类。对于我们的目的，我们将只使用 InlineData 属性。创建以下提供不同加数和预期结果的测试：

```
[Theory]
[InlineData(3,2,5)]
[InlineData(1,-1,0)]
```

```
public void SimpleTheoryTest(int addend1, int addend2, int expectedResult)
{
    Assert.Equal(expectedResult, addend1+addend2);
}
```

注意 有关 xUnit 测试框架的更多信息，请参阅位于 https://xunit.net/ 的文档。

# 执行测试

虽然 xUnit 测试可以从命令行执行（使用 dotnet test），但在我看来，使用 Visual Studio 执行测试是一种更好的开发体验。从 Test 菜单启动 Test Explorer 以访问运行和调试所有或所选测试。

# 查询数据库

请记住，从数据库数据创建实体实例通常涉及对 DbSet<T> 属性执行 LINQ 语句。LINQ 语句由数据库提供程序和 LINQ 翻译引擎转换为 SQL，并从数据库读取适当的数据。数据也可以使用 FromSqlRaw() 或 FromSqlInterpolated() 方法使用原始 SQL 字符串加载。默认情况下加载到 DbSet<T> 集合中的实体会被添加到 ChangeTracker 中，但也可以不跟踪加载。加载到无键 DbSet<T> 集合中的数据永远不会被跟踪。

如果相关实体已加载到 DbSet<T> 中，EF Core 将沿导航属性连接新实例。例如，如果 Cars 已加载到 DbSet<Car> 集合中，然后相关的 Orders 在同一个 ApplicationDbContext 实例的 DbSet<Order> 中加载，则 Car.Orders 导航属性将返回相关的 Order 实体，而无需重新查询数据库。

这里演示的许多方法都有可用的异步版本。LINQ 查询的语法在结构上是相同的，因此我只演示非异步版本。

# 实体状态

当实体通过从数据库读取数据创建时，EntityState 值设置为 Unchanged。

# LINQ 查询

DbSet<T> 集合类型实现了（除其他接口外）IQueryable<T>。这允许使用 C# LINQ 命令创建查询以从数据库获取数据。虽然可以使用 DbSet<T> 集合类型使用所有 C# LINQ 语句，但某些 LINQ 语句可能不被数据库提供程序支持，并且 EF Core 添加了额外的 LINQ 语句。无法转换为数据库提供程序查询语言的不受支持的 LINQ 语句将在运行时引发异常，除非该语句是 LINQ 链的最后一个语句。如果不支持的 LINQ 语句是 LINQ 链中的最后一个语句，它将在客户端（在 C# 中）执行。

> 注意 本书不是完整的 LINQ 参考，而是只显示几个示例。有关 LINQ 查询的更多示例，微软已在 https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b 发布了 101 个 LINQ 示例。

# LINQ 执行

作为提醒，使用 LINQ 查询数据库以获取实体列表时，查询不会在查询被迭代、转换为 List<T>（或数组）或绑定到列表控件（如数据网格）之前执行。对于单记录查询，当使用单记录调用（First()、Single() 等）时，语句会立即执行。

在 EF Core 5 中新增，您可以在大多数 LINQ 查询中调用 ToQueryString() 方法来检查对数据库执行的查询。对于拆分查询，ToQueryString() 方法仅返回将执行的第一个查询。在可用时，本节中的测试将变量（qs）设置为此值，以便您在调试测试时检查查询。

除非另有特别说明，否则第一组测试在 CustomerTests.cs 类中。

# 获取所有记录

要获取表的全部记录，只需直接使用 DbSet<T> 属性，而无需任何 LINQ 语句。添加以下 Fact：

```
[Fact]
public void ShouldGetAllOfTheCustomers()
{
    var qs = Context.Customers.ToQueryString();
    var customers = Context.Customers.ToList();
    Assert.Equal(5, customers.Count);
}
```

该语句被转换为以下 SQL：

```
SELECT [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [dbo].[Customers] AS [c]
```

对于无键实体，如 CustomerOrderViewModel 也是如此，它被配置为从 CustomerOrderView 获取其数据。

```
modelBuilder.Entity<CustomerOrderViewModel>().HasNoKey().ToView("CustomerOrderView", "dbo");
```

用于视图模型的 DbSet<T> 实例为键实体提供了 DbSet<T> 的所有查询功能。区别在于更新能力。视图模型更改无法持久保存到数据库，而键实体可以。向 OrderTest.cs 类添加以下测试以显示从视图获取数据：

```
public void ShouldGetAllViewModels()
{
    var qs = Context.Orders.ToQueryString();
    var orders = Context.Orders.ToList();
}
```



```
Assert.NotEmpty(orders);
Assert.Equal(5,orders.Count);
}
```

The statement gets translated into the following SQL:

```
SELECT [c].[Color], [c].[FirstName], [c].[IsDrivable], [c].[LastName], [c].[Make], [c].[PetName]
FROM [dbo].[CustomerOrderView] AS [c]
```

## Filter Records

The Where() method is used to filter records from the DbSet<T>. Multiple Where() methods can be fluently chained to dynamically build the query. Chained Where() methods are always combined as and clauses. To create an or statement, use the same Where() clause.

The following test returns the customer where the last name starts with a W (case insensitive):

```
[Fact]
public void ShouldGetCustomersWithLastNameW()
{
    IQueryable<Customer> query = Context.Customers
    .Where(x => x.PersonalInformation.LastName.StartsWith("W"));
    var qs = query.ToQueryString();
    List<Customer> customers = query.ToList();
    Assert.Equal(2, customers.Count);
}
```

The LINQ query gets translated into the following SQL:

```
SELECT [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [dbo].[Customers] AS [c]
WHERE [c].[LastName] IS NOT NULL AND ([c].[LastName] LIKE N'W%')
```

The following test returns the customer where the last name starts with a W (case insensitive) and the first name starts with an M (case insensitive) and demonstrates chaining Where() methods in a LINQ query:

```
[Fact]
public void ShouldGetCustomersWithLastNameWAndFirstNameM()
{
    IQueryable<Customer> query = Context.Customers
    .Where(x => x.PersonalInformation.LastName.StartsWith("W"))
    .Where(x => x.PersonalInformation.FirstName.StartsWith("M"));
    var qs = query.ToQueryString();
    List<Customer> customers = query.ToList();
    Assert.Single(customers);
}
```

The following test returns the customer where the last name starts with a W (case insensitive) and the first name starts with an M (case insensitive) using a single Where() method:

```
[Fact]
public void ShouldGetCustomersWithLastNameWAndFirstNameM()
{
    IQueryable<Customer> query = Context.Customers
        .Where(x => x.PersonalInformation.LastName.StartsWith("W") &&
            x.PersonalInformation.FirstName.StartsWith("M"));
    var qs = query.ToQueryString();
    List<Customer> customers = query.ToList();
    Assert.Single(customers);
}
```

Both queries get translated into the following SQL:

```
SELECT [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [Dbo].[Customers] AS [c]
WHERE ([c].[LastName] IS NOT NULL AND ([c].[LastName] LIKE N'W%'))
AND ([c].[FirstName] IS NOT NULL AND ([c].[FirstName] LIKE N'M%'))
```

The following test returns the customer where the last name starts with a W (case insensitive) or the last name starts with an H (case insensitive):

```
[Fact]
public void ShouldGetCustomersWithLastNameWOrH()
{
    IQueryable<Customer> query = Context.Customers
        .Where(x => x.PersonalInformation.LastName.StartsWith("W") ||
            x.PersonalInformation.LastName.StartsWith("H"));
    var qs = query.ToQueryString();
    List<Customer> customers = query.ToList();
    Assert.Equal(3, customers.Count);
}
```

This gets translated into the following SQL:

```
SELECT [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [Dbo].[Customers] AS [c]
WHERE ([c].[LastName] IS NOT NULL AND ([c].[LastName] LIKE N'W%'))
OR ([c].[LastName] IS NOT NULL AND ([c].[LastName] LIKE N'H%'))
```

The following test returns the customer where the last name starts with a W (case insensitive) or the last name starts with an H (case insensitive). This test demonstrates using the EF.Functions.Like() method. Note that you must include the wildcard (%) yourself.

```
[Fact]
public void ShouldGetCustomersWithLastNameWOrH()
{
    IQueryable<Customer> query = Context.Customers
        .Where(x => EF.Functions.Like(x.PersonalInformation.LastName, "W%") ||
            EF.Functions.Like(x.PersonalInformation.LastName, "H%"));
    var qs = query.ToQueryString();
    List<Customer> customers = query.ToList();
    Assert.Equal(3, customers.Count);
}
```

This gets translated into the following SQL (notice it does not check for null):

```
SELECT [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [dbo].[Customers] AS [c]
WHERE (([c].[LastName] LIKE N'%') OR ([c].[LastName] LIKE N'H%')
```

The following test in the CarTests.cs class uses a Theory to test for the number of Car records in the Inventory table based on the MakeId (the IgnoreQueryFilters() method is covered in the “Global Query Filters” section):

```
[Theory]
[InlineData(1, 2)]
[InlineData(2, 1)]
[InlineData(3, 1)]
[InlineData(4, 2)]
[InlineData(5, 3)]
[InlineData(6, 1)]
public void ShouldGetTheCarsByMake(int makeId, int expectedCount)
{
    IQueryable<Car> query =
        Context.Cars.IgnoreQueryFilters().Where(x => x.MakeId == makeId);
    var qs = query.ToQueryString();
    var cars = query.ToList();
    Assert.Equal(expectedCount, cars.Count);
}
```

Each InlineData row becomes a unique test in the test runner. For this example, six tests are processed, and six queries are executed against the database. Here is the SQL from one of the tests (the only difference in the queries from the other tests in the Theory is the value for MakeId):

```
DECLARE @__makeId_0 int = 1;
SELECT [i].[Id], [i].[Color], [i].[IsDrivable], [i].[MakeId], [i].[PetName], [i].[TimeStamp]
FROM [dbo].[Inventory] AS [i]
WHERE [i].[MakeId] = @__makeId_0
```

The following Theory test shows a filtered query with CustomerOrderViewModel (place the test in the OrderTests.cs class):

```
[Theory]
[InlineData("Black",2)]
[InlineData("Rust",1)]
[InlineData("Yellow",1)]
[InlineData("Green",0)]
[InlineData("Pink",1)]
[InlineData("Brown",0)]
public void ShouldGetAllViewModelsByColor(string color, int expectedCount)
{
    var query = _repo.GetOrdersViewModel().Where(x=>x.Color == color);
    var qs = query.ToQueryString();
    var orders = query.ToList();
    Assert.Equal(expectedCount,orders.Count);
}
```

The generated query for the first InlineData test is listed here:

```
DECLARE @__color_0 nvarchar(4000) = N'Black';
SELECT [c].[Color], [c].[FirstName], [c].[IsDrivable], [c].[LastName], [c].[Make], [c].[PetName]
FROM [dbo].[CustomerOrderView] AS [c]
WHERE [c].[Color] = @__color_0
```

## Sort Records

The OrderBy() and OrderByDescending() methods set the sort(s) for the query, ascending and descending, respectively. If subsequent sorts are required, use the ThenBy() and ThenByDescending() methods. Sorting is shown in the following test:

```
[Fact]
public void ShouldSortByLastNameThenFirstName()
{
    //Sort by Last name then first name
    var query = Context.Customers
        .OrderBy(x => x.PersonalInformation.LastName)
        .ThenBy(x => x.PersonalInformation.FirstName);
    var qs = query.ToQueryString();
    var customers = query.ToList();
    //if only one customer, nothing to test
    if (customers.Count <= 1) { return; }
    for (int x = 0; x < customers.Count - 1; x++)
    {
        var pi = customers[x].PersonalInformation;
        var pi2 = customers[x + 1].PersonalInformation;
        var compareLastName = string.Compare(pi.LastName,
            pi2.LastName, StringComparison.CurrentCultureIgnoreCase);
        Assert.True(compareLastName <= 0);
        if (compareLastName != 0) continue;
        var compareFirstName = string.Compare(pi.FirstName,
            pi2.FirstName, StringComparison.CurrentCultureIgnoreCase);
        Assert.True(compareFirstName <= 0);
    }
}
```

The preceding LINQ query gets translated into the following:

```
SELECT [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [Dbo].[Customers] AS [c]
ORDER BY [c].[LastName], [c].[FirstName]
```

## Reverse Sort Records

The Reverse() method reverses the entire sort order, as demonstrated in the next test:

```
[Fact]
public void ShouldSortByFirstNameThenLastNameUsingReverse()
{
    //Sort by Last name then first name then reverse the sort
    var query = Context.Customers
        .OrderBy(x => x.PersonalInformation.LastName)
        .ThenBy(x => x.PersonalInformation.FirstName)
        .Reverse();
    var qs = query.ToQueryString();
    var customers = query.ToList();
    //if only one customer, nothing to test
    if (customers.Count <= 1) { return; }

    for (int x = 0; x < customers.Count - 1; x++)
    {
        var pi1 = customers[x].PersonalInformation;
        var pi2 = customers[x + 1].PersonalInformation;
        var compareLastName = string.Compare(pi1.LastName,
        pi2.LastName, StringComparison.CurrentCultureIgnoreCase);
        Assert.True(compareLastName >= 0);
        if (compareLastName != 0) continue;
        var compareFirstName = string.Compare(pi1.FirstName,
        pi2.FirstName, StringComparison.CurrentCultureIgnoreCase);
        Assert.True(compareFirstName >= 0);
    }
}
```

The preceding LINQ query gets translated into the following:

```
SELECT [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [Dbo].[Customers] AS [c]
ORDER BY [c].[LastName] DESC, [c].[FirstName] DESC
```

## Retrieve a Single Record

There are three main methods for returning a single record with a query: First()/FirstOrDefault(), Last()/LastOrDefault(), and Single()/SingleOrDefault(). While all three return a single record, their approaches all differ. The three methods and their variants are detailed here:

- First() returns the first record that matches the query condition and any ordering clauses. If no ordering is specified, then the record returned is based on database order. If no record is returned, an exception is thrown.
- The FirstOrDefault() behavior matches First() except that if no records match the query, the method returns the default value for the type (null).





- Single() 返回与查询条件和任何排序子句匹配的第一条记录。如果未指定排序，则返回的记录基于数据库顺序。如果没有记录或匹配的记录多于一条，则抛出异常。
- SingleOrDefault() 的行为与 Single() 匹配，只是如果没有记录匹配查询，方法会返回该类型的默认值（null）。
- Last() 返回与查询条件和任何排序子句匹配的最后一条记录。如果未指定排序，则抛出异常。如果没有记录返回，则抛出异常。
- LastOrDefault() 的行为与 Last() 匹配，只是如果没有记录匹配查询，方法会返回该类型的默认值（null）。

所有方法也可以接受一个 Expression<Func<T, bool>>（lambda）来过滤结果集。这意味着你可以将 Where() 表达式放在 First()/Single() 方法的调用中。以下语句是等价的：

```
Context.Customers.Where(c=>c.Id < 5).First();
Context.Customers.First(c=>c.Id < 5);
```

由于单记录 LINQ 语句的立即执行，ToQueryString() 方法不可用。所列出的查询转换是通过使用 SQL Server Profiler 提供的。

## Using First

使用 First() 和 FirstOrDefault() 的无参数形式时，将返回第一条记录（基于数据库顺序或任何之前的排序子句）。

以下测试基于数据库顺序获取第一条记录：

```
[Fact]
public void GetFirstMatchingRecordDatabaseOrder()
{
    //Gets the first record, database order
    var customer = Context.Customers.First();
    Assert.Equal(1, customer.Id);
}
```

前面的 LINQ 查询被转换为以下内容：

```
SELECT TOP(1) [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [dbo].[Customers] AS [c]
```

以下测试基于“姓氏、名字”顺序获取第一条记录：

```
[Fact]
public void GetFirstMatchingRecordNameOrder()
{
    //Gets the first record, lastname, first name order
    var customer = Context.Customers
        .OrderBy(x => x.PersonalInformation.LastName)
        .ThenBy(x => x.PersonalInformation.FirstName)
        .First();
    Assert.Equal(1, customer.Id);
}
```

前面的 LINQ 查询被转换为以下内容：

```
SELECT TOP(1) [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [dbo].[Customers] AS [c]
ORDER BY [c].[LastName], [c].[FirstName]
```

以下测试断言如果使用 First() 时没有匹配项，则会抛出异常：

```
[Fact]
public void FirstShouldThrowExceptionIfNoneMatch()
{
    //Filters based on Id. Throws due to no match
    Assert.Throws<InvalidOperationException>(() => Context.Customers.First(x => x.Id == 10));
}
```

前面的 LINQ 查询被转换为以下内容：

```
SELECT TOP(1) [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [dbo].[Customers] AS [c]
WHERE [c].[Id] = 10
```

注意 Assert.Throws() 是一种特殊的断言语句。它期望代码在表达式中抛出异常。如果没有抛出异常，断言将失败。

使用 FirstOrDefault() 时，如果没有返回数据，结果不是异常，而是一个 null 记录。

```
[Fact]
public void FirstOrDefaultShouldReturnDefaultIfNoneMatch()
{
    //Expression<Func<Customer, bool>> is a lambda expression
    Expression<Func<Customer, bool>> expression = x => x.Id == 10;
    //Returns null when nothing is found
    var customer = Context.Customers.FirstOrDefault(expression);
    Assert.Null(customer);
}
```

前面的 LINQ 查询被转换为与之前相同的内容：

```
SELECT TOP(1) [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [dbo].[Customers] AS [c]
WHERE [c].[Id] = 10
```

## Using Last

使用 Last() 和 LastOrDefault() 的无参数形式时，将返回最后一条记录（基于任何之前的排序子句）。

以下测试基于“姓氏、名字”顺序获取最后一条记录：

```
[Fact]
public void GetLastMatchingRecordNameOrder()
{
    //Gets the last record, lastname desc, first name desc order
    var customer = Context.Customers
        .OrderBy(x => x.PersonalInformation.LastName)
        .ThenBy(x => x.PersonalInformation.FirstName)
        .Last();
    Assert.Equal(4, customer.Id);
}
```

EF Core 会反转 order by 子句，然后取 top(1) 来获取结果。以下是执行的查询：

```
SELECT TOP(1) [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [dbo].[Customers] AS [c]
ORDER BY [c].[LastName] DESC, [c].[FirstName] DESC
```

## Using Single

概念上，Single()/SingleOrDefault() 与 First()/FirstOrDefault() 相同。主要区别是 Single()/SingleOrDefault() 返回 Top(2) 而不是 Top(1)，并且如果数据库返回两条记录则抛出异常。

以下测试检索 Id == 1 的单条记录：

```
[Fact]
public void GetOneMatchingRecordWithSingle()
{
    //Gets the first record, database order
    var customer = Context.Customers.Single(x => x.Id == 1);
    Assert.Equal(1, customer.Id);
}
```

前面的 LINQ 查询被转换为以下内容：

```
SELECT TOP(2) [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [dbo].[Customers] AS [c]
WHERE [c].[Id] = 1
```

如果没有记录返回，Single() 会抛出异常。

```
[Fact]
public void SingleShouldThrowExceptionIfNoneMatch()
{
    //Filters based on Id. Throws due to no match
    Assert.Throws<InvalidOperationException>(() => Context.Customers.Single(x => x.Id == 10));
}
```

前面的 LINQ 查询被转换为以下内容：

```
SELECT TOP(2) [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [dbo].[Customers] AS [c]
WHERE [c].[Id] = 10
```

使用 Single() 或 SingleOrDefault() 且返回多于一条记录时，会抛出异常。

```
[Fact]
public void SingleShouldThrowExceptionIfMoreThanOneMatch()
{
    // Throws due to more than one match
    Assert.Throws<InvalidOperationException>(() => Context.Customers.Single());
}
```

```
[Fact]
public void SingleOrDefaultShouldThrowExceptionIfMoreThanOneMatch()
{
    // Throws due to more than one match
    Assert.Throws<InvalidOperationException>(() => Context.Customers.SingleOrDefault());
}
```

前面的 LINQ 查询被转换为以下内容：

```
SELECT TOP(2) [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [dbo].[Customers] AS [c]
```

使用 SingleOrDefault() 时，如果没有数据返回，结果不是异常，而是一个 null 记录。

```
[Fact]
public void SingleOrDefaultShouldReturnDefaultIfNoneMatch()
{
    //Expression<Func<Customer, bool>> is a lambda expression
    Expression<Func<Customer, bool>> expression = x => x.Id == 10;
    //Returns null when nothing is found
    var customer = Context.Customers.SingleOrDefault(expression);
    Assert.Null(customer);
}
```

前面的 LINQ 查询被转换为以下内容：

```
SELECT TOP(1) [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [dbo].[Customers] AS [c]
WHERE [c].[Id] = 10
```

## Global Query Filters

回想一下，Car 实体上有一个全局查询过滤器，用于过滤掉 IsDrivable 为 false 的汽车。

```
modelBuilder.Entity<Car>(entity =>
{
    entity.HasQueryFilter(c => c.IsDrivable);
    ...
});
```

打开 CarTests.cs 类并添加以下测试（除非另有具体说明，否则下一节中的所有测试都在 CarTests.cs 类中）：

```
[Fact]
public void ShouldReturnDrivableCarsWithQueryFilterSet()
{
    IQueryable<Car> query = Context.Cars;
    var qs = query.ToQueryString();
    var cars = query.ToList();
    Assert.NotEmpty(cars);
    Assert.Equal(9, cars.Count);
}
```

此外，回想一下我们在数据初始化过程中创建了 10 辆汽车，其中一辆被设置为不可驾驶。当查询执行时，全局查询过滤器会被应用，并执行以下 SQL：

```
SELECT [i].[Id], [i].[Color], [i].[IsDrivable], [i].[MakeId], [i].[PetName], [i].[TimeStamp]
FROM [dbo].[Inventory] AS [i]
WHERE [i].[IsDrivable] = CAST(1 AS bit)
```

> 注意 全局查询过滤器在加载相关实体以及使用 FromSqlRaw() 和 FromSqlInterpolated() 时也会被应用。这些内容将在稍后介绍。

### Disable the Query Filters

要为查询中的实体禁用全局查询过滤器，请将 IgnoreQueryFilters() 方法添加到 LINQ 查询中。这会禁用查询中所有实体的所有过滤器。如果有多个实体具有全局查询过滤器，并且某些实体的过滤器是必需的，则必须将它们添加到 LINQ 语句的 Where() 方法中。

将以下测试添加到 CarTests.cs 类，该测试禁用查询过滤器并返回所有记录：

```
[Fact]
public void ShouldGetAllOfTheCars()
{
    IQueryable<Car> query = Context.Cars.IgnoreQueryFilters();
    var qs = query.ToQueryString();
    var cars = query.ToList();
    Assert.Equal(10, cars.Count);
}
```





正如预期的那样，消除不可驾驶汽车的 WHERE 子句不再出现在生成的 SQL 中。

```
SELECT [i].[Id], [i].[Color], [i].[IsDrivable], [i].[MakeId], [i].[PetName], [i].[TimeStamp]
FROM [dbo].[Inventory] AS [i]
```

## Query Filters on Navigation Properties

除了在 Car 实体上的全局查询过滤器外，我们还在 Order 实体的 CarNavigation 属性上添加了一个查询过滤器。

```
modelBuilder.Entity<Order>().HasQueryFilter(e => e.CarNavigation!.IsDrivable);
```

要查看其效果，将以下测试添加到 OrderTests.cs 类中：

```
[Fact]
public void ShouldGetAllOrdersExceptFiltered()
{
    var query = Context.Orders.AsQueryable();
    var qs = query.ToQueryString();
    var orders = query.ToList();
    Assert.NotEmpty(orders);
    Assert.Equal(4, orders.Count);
}
```

生成的 SQL 如下所示：

```
SELECT [o].[Id], [o].[CarId], [o].[CustomerId], [o].[TimeStamp]
FROM [dbo].[Orders] AS [o]
INNER JOIN (
    SELECT [i].[Id], [i].[IsDrivable]
    FROM [dbo].[Inventory] AS [i]
    WHERE [i].[IsDrivable] = CAST(1 AS bit)
) AS [t] ON [o].[CarId] = [t].[Id]
WHERE [t].[IsDrivable] = CAST(1 AS bit)
```

由于 CarNavigation 导航属性是必需的导航属性，查询转换引擎使用 INNER JOIN，消除了 Car 为不可驾驶的 Order 记录。

要返回所有记录，请在 LINQ 查询中添加 IgnoreQueryFilters()。

## Load Related Data Eagerly

如前一章所述，通过导航属性链接的实体可以使用预加载在一次查询中实例化。Include() 方法表示与相关实体的连接，而 ThenInclude() 方法用于后续连接。这两种方法都将在这些测试中演示。如前所述，当 Include()/ThenInclude() 方法被翻译成 SQL 时，必需关系使用内连接，可选关系使用左连接。

将以下测试添加到 CarTests.cs 类以展示单个 Include()：

```
[Fact]
public void ShouldGetAllOfTheCarsWithMakes()
{
    IIncludableQueryable<Car, Make?> query =
        Context.Cars.Include(c => c.MakeNavigation);
    var queryString = query.ToQueryString();
    var cars = query.ToList();
    Assert.Equal(9, cars.Count);
}
```

该测试将 MakeNavigation 属性添加到结果中，使用以下 SQL 执行内连接。请注意，全局查询过滤器正在生效。

```
SELECT [i].[Id], [i].[Color], [i].[IsDrivable], [i].[MakeId], [i].[PetName], [i].[TimeStamp],
    [m].[Id], [m].[Name], [m].[TimeStamp]
FROM [dbo].[Inventory] AS [i]
INNER JOIN [dbo].[Makes] AS [m] ON [i].[MakeId] = [m].[Id]
WHERE [i].[IsDrivable] = CAST(1 AS bit)
```

第二个测试使用两组相关数据。第一组是获取 Make 信息（与上一个测试相同），第二组是获取 Orders 然后获取 Orders 附加的 Customers。整个测试还过滤掉了有任何订单的 Car 记录。可选关系生成左连接。

```
[Fact]
public void ShouldGetCarsOnOrderWithRelatedProperties()
{
    IIncludableQueryable<Car, Customer?> query = Context.Cars
        .Where(c => c.Orders.Any())
        .Include(c => c.MakeNavigation)
        .Include(c => c.Orders).ThenInclude(o => o.CustomerNavigation);
    var queryString = query.ToQueryString();
    var cars = query.ToList();
    Assert.Equal(4, cars.Count);
    cars.ForEach(c =>
    {
        Assert.NotNull(c.MakeNavigation);
        Assert.NotNull(c.Orders.ToList()[0].CustomerNavigation);
    });
}
```

以下是生成的查询：

```
SELECT [i].[Id], [i].[Color], [i].[IsDrivable], [i].[MakeId], [i].[PetName], [i].[TimeStamp],
    [m].[Id], [m].[Name], [m].[TimeStamp], [to].[Id], [to].[CarId], [to].[CustomerId],
    [to].[TimeStamp], [to].[Id0], [to].[TimeStamp0], [to].[FirstName], [to].[FullName],
    [to].[LastName], [to].[Id1]
FROM [dbo].[Inventory] AS [i]
    INNER JOIN [dbo].[Makes] AS [m] ON [i].[MakeId] = [m].[Id]
    LEFT JOIN (SELECT [o].[Id], [o].[CarId], [o].[CustomerId], [o].[TimeStamp],
        [c].[Id] AS [Id0], [c].[TimeStamp] AS [TimeStamp0], [c].[FirstName], [c].[FullName],
        [c].[LastName], [t].[Id] AS [Id1]
        FROM [dbo].[Orders] AS [o]
            INNER JOIN (
                SELECT [io].[Id], [io].[IsDrivable]
                FROM [dbo].[Inventory] AS [io]
                WHERE [io].[IsDrivable] = CAST(1 AS bit)
            ) AS [t] ON [o].[CarId] = [t].[Id]
            INNER JOIN [dbo].[Customers] AS [c] ON [o].[CustomerId] = [c].[Id]
            WHERE [t].[IsDrivable] = CAST(1 AS bit)
    ) AS [to] ON [i].[Id] = [to].[CarId]
WHERE ([i].[IsDrivable] = CAST(1 AS bit)) AND EXISTS (
    SELECT 1
    FROM [dbo].[Orders] AS [oo]
    INNER JOIN (
        SELECT [i1].[Id], [i1].[Color], [i1].[IsDrivable],
        [i1].[MakeId], [i1].[PetName], [i1].[TimeStamp]
        FROM [dbo].[Inventory] AS [i1]
        WHERE [i1].[IsDrivable] = CAST(1 AS bit)
    ) AS [t1] ON [oo].[CarId] = [t1].[Id]
    WHERE ([t1].[IsDrivable] = CAST(1 AS bit))
        AND ([i].[Id] = [oo].[CarId])
)
ORDER BY [i].[Id], [m].[Id], [to].[Id], [to].[Id1], [to].[Id0];
```

## Splitting Queries on Related Data

LINQ 查询中添加的连接越多，结果查询就越复杂。EF Core 5 中新增了将复杂连接作为拆分查询运行的能力。请参考前一章了解完整讨论，但总结来说，在 LINQ 查询中添加 AsSplitQuery() 方法会指示 EF Core 将对数据库的调用拆分为多次调用。这可以在以数据不一致的风险为代价获得效率。将以下测试添加到你的测试夹具中：

```
[Fact]
public void ShouldGetCarsOnOrderWithRelatedPropertiesAsSplitQuery()
{
    IQueryable<Car> query = Context.Cars.Where(c => c.Orders.Any())
        .Include(c => c.MakeNavigation)
        .Include(c => c.Orders).ThenInclude(o => o.CustomerNavigation)
        .AsSplitQuery();
    var cars = query.ToList();
    Assert.Equal(4, cars.Count);
    cars.ForEach(c =>
    {
        Assert.NotNull(c.MakeNavigation);
        Assert.NotNull(c.Orders.ToList()[0].CustomerNavigation);
    });
}
```

ToQueryString() 方法仅返回第一个查询，因此以下查询是使用 SQL Server Profiler 捕获的：

```
SELECT [i].[Id], [i].[Color], [i].[IsDrivable], [i].[MakeId], [i].[PetName], [i].[TimeStamp], [m].[Id], [m].[Name], [m].[TimeStamp]
FROM [dbo].[Inventory] AS [i]
INNER JOIN [dbo].[Makes] AS [m] ON [i].[MakeId] = [m].[Id]
WHERE ([i].[IsDrivable] = CAST(1 AS bit)) AND EXISTS (
    SELECT 1
    FROM [dbo].[Orders] AS [o]
    INNER JOIN (
        SELECT [io].[Id], [io].[Color], [io].[IsDrivable], [io].[MakeId], [io].[PetName],
        [io].[TimeStamp]
        FROM [dbo].[Inventory] AS [io]
        WHERE [io].[IsDrivable] = CAST(1 AS bit)
    ) AS [t] ON [o].[CarId] = [t].[Id]
    WHERE ([t].[IsDrivable] = CAST(1 AS bit)) AND ([i].[Id] = [o].[CarId])
)
ORDER BY [i].[Id], [m].[Id]
```

```
SELECT [to].[Id], [to].[CarId], [to].[CustomerId], [to].[TimeStamp], [to].[Id1], [to].[TimeStamp1], [to].[FirstName], [to].[FullName], [to].[LastName], [i].[Id], [m].[Id]
FROM [dbo].[Inventory] AS [i]
INNER JOIN [dbo].[Makes] AS [m] ON [i].[MakeId] = [m].[Id]
INNER JOIN (
    SELECT [o].[Id], [o].[CarId], [o].[CustomerId], [o].[TimeStamp], [c].[Id] AS [Id1], [c].[TimeStamp] AS [TimeStamp1], [c].[FirstName], [c].[FullName], [c].[LastName]
    FROM [dbo].[Orders] AS [o]
    INNER JOIN (
        SELECT [io].[Id], [io].[IsDrivable]
        FROM [dbo].[Inventory] AS [io]
        WHERE [io].[IsDrivable] = CAST(1 AS bit)
    ) AS [t] ON [o].[CarId] = [t].[Id]
    INNER JOIN [dbo].[Customers] AS [c] ON [o].[CustomerId] = [c].[Id]
    WHERE [t].[IsDrivable] = CAST(1 AS bit)
) AS [to] ON [i].[Id] = [to].[CarId]
WHERE ([i].[IsDrivable] = CAST(1 AS bit)) AND EXISTS (
    SELECT 1
    FROM [dbo].[Orders] AS [oo]
    INNER JOIN (
        SELECT [ii].[Id], [ii].[Color], [ii].[IsDrivable], [ii].[MakeId], [ii].[PetName],
        [ii].[TimeStamp]
        FROM [dbo].[Inventory] AS [ii]
        WHERE [ii].[IsDrivable] = CAST(1 AS bit)
    ) AS [t1] ON [oo].[CarId] = [t1].[Id]
    WHERE ([t1].[IsDrivable] = CAST(1 AS bit)) AND ([i].[Id] = [oo].[CarId])
)
ORDER BY [i].[Id], [m].[Id]
```

是否拆分查询取决于你的业务需求。

## Filtering Related Data

EF Core 5 引入了在包含集合属性时进行过滤的能力。在 EF Core 5 之前，获取集合导航属性的过滤列表的唯一方法是通过显式加载。将以下测试添加到 MakeTests.cs 类中，该测试演示了获取所有 Make 记录以及那些为黄色的汽车：

```
[Fact]
public void ShouldGetAllMakesAndCarsThatAreYellow()
{
    var query = Context.Makes.IgnoreQueryFilters()
        .Include(x => x.Cars.Where(x => x.Color == "Yellow"));




```csharp
var qs = query.ToQueryString();
var makes = query.ToList();
Assert.NotNull(makes);
Assert.NotEmpty(makes);
Assert.NotEmpty(makes.Where(x => x.Cars.Any()));
Assert.Empty(makes.First(m => m.Id == 1).Cars);
Assert.Empty(makes.First(m => m.Id == 2).Cars);
Assert.Empty(makes.First(m => m.Id == 3).Cars);
Assert.Single(makes.First(m => m.Id == 4).Cars);
Assert.Empty(makes.First(m => m.Id == 5).Cars);
}
```

生成的 SQL 如下：

```sql
SELECT [m].[Id], [m].[Name], [m].[TimeStamp], [t].[Id], [t].[Color], [t].[IsDrivable],
[t].[MakeId], [t].[PetName], [t].[TimeStamp]
FROM [dbo].[Makes] AS [m]
LEFT JOIN (
    SELECT [i].[Id], [i].[Color], [i].[IsDrivable], [i].[MakeId], [i].[PetName],
    [i].[TimeStamp]
    FROM [dbo].[Inventory] AS [i]
    WHERE [i].[Color] = N'Yellow'
) AS [t] ON [m].[Id] = [t].[MakeId]
ORDER BY [m].[Id], [t].[Id]
```

将查询改为拆分查询会生成以下 SQL（来自 SQL Server Profiler 的集合）：

```sql
SELECT [m].[Id], [m].[Name], [m].[TimeStamp]
FROM [dbo].[Makes] AS [m]
ORDER BY [m].[Id]
```

```sql
SELECT [t].[Id], [t].[Color], [t].[IsDrivable], [t].[MakeId], [t].[PetName], [t].[TimeStamp], [m].[Id]
FROM [dbo].[Makes] AS [m]
INNER JOIN (
    SELECT [i].[Id], [i].[Color], [i].[IsDrivable], [i].[MakeId], [i].[PetName], [i].[TimeStamp]
    FROM [dbo].[Inventory] AS [i]
    WHERE [i].[Color] = N'Yellow'
) AS [t] ON [m].[Id] = [t].[MakeId]
ORDER BY [m].[Id]
```

## 显式加载相关数据

如果需要在主体实体被查询到内存之后加载相关数据，可以通过后续数据库调用从数据库中检索相关实体。这通过派生 `DbContext` 上的 `Entry()` 方法触发。当加载一对多关系的“多”端实体时，请使用 `Entry` 结果上的 `Collection()` 方法。要加载一对多关系（或一对一关系）的“一”端实体，请使用 `Reference()` 方法。在 `Collection()` 或 `Reference()` 方法上调用 `Query()` 会返回一个 `IQueryable<T>`，可用于获取查询字符串（如以下测试所示）和管理查询筛选器（如下一节所示）。要执行查询并加载记录，请在 `Collection()`、`Reference()` 或 `Query()` 方法上调用 `Load()` 方法。当调用 `Load()` 时，查询会立即执行。

以下测试（回到 `CarTests.cs` 类中）展示了如何加载 `Car` 实体上的引用导航属性：

```csharp
[Fact]
public void ShouldGetReferenceRelatedInformationExplicitly()
{
    var car = Context.Cars.First(x => x.Id == 1);
    Assert.Null(car.MakeNavigation);
    var query = Context.Entry(car).Reference(c => c.MakeNavigation).Query();
    var qs = query.ToQueryString();
    query.Load();
    Assert.NotNull(car.MakeNavigation);
}
```

生成的 SQL 如下：

```sql
DECLARE @__p_0 int = 1;
SELECT [m].[Id], [m].[Name], [m].[TimeStamp]
FROM [dbo].[Makes] AS [m]
WHERE [m].[Id] = @__p_0
```

该测试展示了如何加载 `Car` 实体上的集合导航属性：

```csharp
[Fact]
public void ShouldGetCollectionRelatedInformationExplicitly()
{
    var car = Context.Cars.First(x => x.Id == 1);
    Assert.Empty(car.Orders);
    var query = Context.Entry(car).Collection(c => c.Orders).Query();
    var qs = query.ToQueryString();
    query.Load();
    Assert.Single(car.Orders);
}
```

生成的 SQL 如下：

```sql
DECLARE @__p_0 int = 1;
SELECT [o].[Id], [o].[CarId], [o].[CustomerId], [o].[TimeStamp]
FROM [dbo].[Orders] AS [o]
INNER JOIN (
    SELECT [i].[Id], [i].[IsDrivable]
    FROM [dbo].[Inventory] AS [i]
    WHERE [i].[IsDrivable] = CAST(1 AS bit)
) AS [t] ON [o].[CarId] = [t].[Id]
WHERE ([t].[IsDrivable] = CAST(1 AS bit)) AND ([o].[CarId] = @__p_0)
```

## 使用查询筛选器显式加载相关数据

除了塑造在预先加载相关数据时生成的查询外，全局查询筛选器在显式加载相关数据时也会生效。请看以下测试（在 `MakeTests.cs` 类中）：

```csharp
[Theory]
[InlineData(1, 1)]
[InlineData(2, 1)]
[InlineData(3, 1)]
[InlineData(4, 2)]
[InlineData(5, 3)]
[InlineData(6, 1)]
public void ShouldGetAllCarsForAMakeExplicitlyWithQueryFilters(int makeId, int carCount)
{
    var make = Context.Makes.First(x => x.Id == makeId);
    IQueryable<Car> query = Context.Entry(make).Collection(c => c.Cars).Query();
    var qs = query.ToQueryString();
    query.Load();
    Assert.Equal(carCount, make.Cars.Count());
}
```

该测试类似于“筛选记录”一节中的 `ShouldGetTheCarsByMake()`。不过，该测试不是只获取具有特定 `MakeId` 的 `Car` 记录，而是先获取一个 `Make` 记录，然后为内存中的 `Make` 记录显式加载 `Car` 记录。生成的查询如下：

```sql
DECLARE @__p_0 int = 5;
SELECT [i].[Id], [i].[Color], [i].[IsDrivable], [i].[MakeId], [i].[PetName], [i].[TimeStamp]
FROM [dbo].[Inventory] AS [i]
WHERE ([i].[IsDrivable] = CAST(1 AS bit)) AND ([i].[MakeId] = @__p_0)
```

请注意，即使查询中的主体实体是 `Make` 记录，查询筛选器仍在使用。要在显式加载记录时关闭查询筛选器，请将 `IgnoreQueryFilters()` 与 `Query()` 方法结合使用。以下是关闭查询筛选器的测试（同样在 `MakeTests.cs` 类中）：

```csharp
[Theory]
[InlineData(1, 2)]
[InlineData(2, 1)]
[InlineData(3, 1)]
[InlineData(4, 2)]
[InlineData(5, 3)]
[InlineData(6, 1)]
public void ShouldGetAllCarsForAMakeExplicitly(int makeId, int carCount)
{
    var make = Context.Makes.First(x => x.Id == makeId);
    IQueryable<Car> query =
        Context.Entry(make).Collection(c => c.Cars).Query().IgnoreQueryFilters();
    var qs = query.IgnoreQueryFilters().ToQueryString();
    query.Load();
    Assert.Equal(carCount, make.Cars.Count());
}
```

## 使用 LINQ 的 SQL 查询

如果特定查询的 LINQ 语句过于复杂，或者测试显示性能不理想，可以使用 `DbSet<T>` 的 `FromSqlRaw()` 或 `FromSqlInterpolated()` 方法通过原始 SQL 语句检索数据。SQL 语句可以是内联 T-SQL `SELECT` 语句、存储过程或表值函数。如果查询是开放式查询（例如，没有终止分号的 T-SQL 语句），则可以将 LINQ 语句添加到 `FromSqlRaw()`/`FromSqlInterpolated()` 方法中，以进一步定义生成的查询。整个查询在服务器端执行，将 SQL 语句与 LINQ 语句生成的 SQL 合并在一起。

如果语句已终止或包含无法在此基础上构建的 SQL（例如，使用公用表表达式），该查询仍会在服务器端执行，但任何额外的筛选和处理都必须在客户端以 LINQ to Objects 的形式完成。

`FromSqlRaw()` 会完全按照键入的方式执行查询。`FromSqlInterpolated()` 使用 C# 字符串内插，然后将内插值转换为参数。以下测试（在 `CarTests.cs` 类中）展示了使用这两种方法（有和没有全局查询筛选器）的示例：

```csharp
[Fact]
public void ShouldNotGetTheLemonsUsingFromSql()
{
    var entity = Context.Model.FindEntityType($"{typeof(Car).FullName}");
    var tableName = entity.GetTableName();
    var schemaName = entity.GetSchema();
    var cars = Context.Cars.FromSqlRaw($"Select * from {schemaName}.{tableName}").ToList();
    Assert.Equal(9, cars.Count);
}
```

```csharp
[Fact]
public void ShouldGetTheCarsUsingFromSqlWithIgnoreQueryFilters()
{
    var entity = Context.Model.FindEntityType($"{typeof(Car).FullName}");
    var tableName = entity.GetTableName();
    var schemaName = entity.GetSchema();
    var cars = Context.Cars.FromSqlRaw($"Select * from {schemaName}.{tableName}")
        .IgnoreQueryFilters().ToList();
    Assert.Equal(10, cars.Count);
}
```

```csharp
[Fact]
public void ShouldGetOneCarUsingInterpolation()
{
    var carId = 1;
    var car = Context.Cars
        .FromSqlInterpolated($"Select * from dbo.Inventory where Id = {carId}")
        .Include(x => x.MakeNavigation)
        .First();
    Assert.Equal("Black", car.Color);
    Assert.Equal("VW", car.MakeNavigation.Name);
}
```

```csharp
[Theory]
[InlineData(1, 1)]
[InlineData(2, 1)]
```


```csharp
[InlineData(3, 1)]
[InlineData(4, 2)]
[InlineData(5, 3)]
[InlineData(6, 1)]
public void ShouldGetTheCarsByMakeUsingFromSql(int makeId, int expectedCount)
{
    var entity = Context.Model.FindEntityType($"{typeof(Car).FullName}");
    var tableName = entity.GetTableName();
    var schemaName = entity.GetSchema();
    var cars = Context.Cars.FromSqlRaw($"Select * from {schemaName}.{tableName}")
        .Where(x => x.MakeId == makeId).ToList();
    Assert.Equal(expectedCount, cars.Count);
}
```

使用 FromSqlRaw()/FromSqlInterpolated() 方法时有一些规则：从 SQL 语句返回的列必须与模型上的映射列匹配，必须返回模型的所有列，并且不能返回相关数据。

# 聚合方法

EF Core 还支持服务器端聚合方法（Max()、Min()、Count()、Average() 等）。聚合方法可以添加到 LINQ 查询的末尾并配合 Where() 方法使用，或者过滤表达式也可以包含在聚合方法本身中（就像 First() 和 Single() 一样）。聚合在服务器端执行，查询返回单个值。全局查询过滤器也会影响聚合方法，并且可以使用 IgnoreQueryFilters() 禁用。

本节所示的所有 SQL 语句均使用 SQL Server Profiler 收集。

这个第一个测试（在 CarTests.cs 中）只是统计数据库中所有 Car 记录的数量。由于查询过滤器仍然处于活动状态，计数返回九辆汽车。

```csharp
[Fact]
public void ShouldGetTheCountOfCars()
{
    var count = Context.Cars.Count();
    Assert.Equal(9, count);
}
```

执行的 SQL 如下所示：

```sql
SELECT COUNT(*)
FROM [dbo].[Inventory] AS [i]
WHERE [i].[IsDrivable] = CAST(1 AS bit)
```

通过添加 IgnoreQueryFilters()，Count() 方法返回 10，并且 where 子句从 SQL 查询中移除。

```csharp
[Fact]
public void ShouldGetTheCountOfCarsIgnoreQueryFilters()
{
    var count = Context.Cars.IgnoreQueryFilters().Count();
    Assert.Equal(10, count);
}
```

```sql
-- Generated SQL
SELECT COUNT(*) FROM [dbo].[Inventory] AS [i]
```

以下测试（也在 CarTests.cs 中）演示了带 where 条件 的 Count() 方法。第一个测试将表达式直接添加到 Count() 方法中，第二个测试将 Count() 方法添加到 LINQ 语句的末尾。

```csharp
[Theory]
[InlineData(1, 1)]
[InlineData(2, 1)]
[InlineData(3, 1)]
[InlineData(4, 2)]
[InlineData(5, 3)]
[InlineData(6, 1)]
public void ShouldGetTheCountOfCarsByMakeP1(int makeId, int expectedCount)
{
    var count = Context.Cars.Count(x => x.MakeId == makeId);
    Assert.Equal(expectedCount, count);
}
```

```csharp
[Theory]
[InlineData(1, 1)]
[InlineData(2, 1)]
[InlineData(3, 1)]
[InlineData(4, 2)]
[InlineData(5, 3)]
[InlineData(6, 1)]
public void ShouldGetTheCountOfCarsByMakeP2(int makeId, int expectedCount)
{
    var count = Context.Cars.Where(x => x.MakeId == makeId).Count();
    Assert.Equal(expectedCount, count);
}
```

两个测试都创建相同的 SQL 调用到服务器，如下所示（MakeId 随每次测试根据 InlineData 变化）：

```sql
exec sp_executesql N'SELECT COUNT(*)
FROM [dbo].[Inventory] AS [i]
WHERE ([i].[IsDrivable] = CAST(1 AS bit)) AND ([i].[MakeId] = @__makeId_0)'
,N'@__makeId_0 int',@__makeId_0=6
```

# Any() 和 All()

Any() 和 All() 方法检查一组记录是否匹配条件（Any()）或是否所有记录都匹配条件（All()）。就像聚合方法一样，它们可以添加到 LINQ 查询的末尾并配合 Where() 方法使用，或者过滤表达式也可以包含在方法本身中。Any() 和 All() 方法在服务器端执行，查询返回布尔值。全局查询过滤器也会影响 Any() 和 All() 方法，并且可以使用 IgnoreQueryFilters() 禁用。

本节所示的所有 SQL 语句均使用 SQL Server Profiler 收集。

这个第一个测试（在 CarTests.cs 中）检查是否有任何汽车记录具有特定的 MakeId。

```csharp
[Theory]
[InlineData(1, true)]
[InlineData(11, false)]
public void ShouldCheckForAnyCarsWithMake(int makeId, bool expectedResult)
{
    var result = Context.Cars.Any(x => x.MakeId == makeId);
    Assert.Equal(expectedResult, result);
}
```

第一个理论测试执行的 SQL 如下所示：

```sql
exec sp_executesql N'SELECT CASE
    WHEN EXISTS (
        SELECT 1
        FROM [dbo].[Inventory] AS [i]
        WHERE ([i].[IsDrivable] = CAST(1 AS bit)) AND ([i].[MakeId] = @__makeId_0)) THEN
        CAST(1 AS bit)
    ELSE CAST(0 AS bit)
END',N'@__makeId_0 int',@__makeId_0=1
```

这个第二个测试检查所有汽车记录是否具有特定的 MakeId。

```csharp
[Theory]
[InlineData(1, false)]
[InlineData(11, false)]
public void ShouldCheckForAllCarsWithMake(int makeId, bool expectedResult)
{
    var result = Context.Cars.All(x => x.MakeId == makeId);
    Assert.Equal(expectedResult, result);
}
```

第一个理论测试执行的 SQL 如下所示：

```sql
exec sp_executesql N'SELECT CASE
    WHEN NOT EXISTS (
        SELECT 1
        FROM [dbo].[Inventory] AS [i]
        WHERE ([i].[IsDrivable] = CAST(1 AS bit)) AND ([i].[MakeId] <> @__makeId_0)) THEN
        CAST(1 AS bit)
    ELSE CAST(0 AS bit)
END',N'@__makeId_0 int',@__makeId_0=1
```

# 从存储过程获取数据

需要检查的最后一个数据检索模式是从存储过程获取数据。虽然与 EF 6 相比，EF Core 在存储过程方面存在一些差距，但请记住 EF Core 建立在 ADO.NET 之上。我们只需要向下降低一层，并记住我们在 ORM 之前如何调用存储过程。CarRepo 中的以下方法创建所需的参数（输入和输出），利用 ApplicationDbContext Database 属性，并调用 ExecuteSqlRaw()：

```csharp
public string GetPetName(int id)
{
    var parameterId = new SqlParameter
```

```csharp
{
    ParameterName = "@carId",
    SqlDbType = System.Data.SqlDbType.Int,
    Value = id,
};

var parameterName = new SqlParameter
{
    ParameterName = "@petName",
    SqlDbType = System.Data.SqlDbType.NVarChar,
    Size = 50,
    Direction = ParameterDirection.Output
};

var result = Context.Database
    .ExecuteSqlRaw("EXEC [dbo].[GetPetName] @carId, @petName OUTPUT", parameterId,
    parameterName);
return (string)parameterName.Value;
}
```

有了这段代码，测试变得很简单。将以下测试添加到 CarTests.cs 类中：

```csharp
[Theory]
[InlineData(1, "Zippy")]
[InlineData(2, "Rusty")]
[InlineData(3, "Mel")]
[InlineData(4, "Clunker")]
[InlineData(5, "Bimmer")]
[InlineData(6, "Hank")]
[InlineData(7, "Pinky")]
[InlineData(8, "Pete")]
[InlineData(9, "Brownie")]
public void ShouldGetValueFromStoredProc(int id, string expectedName)
{
    Assert.Equal(expectedName, new CarRepo(Context).GetPetName(id));
}
```

# 创建记录

记录通过在代码中创建它们，将它们添加到其 DbSet<T> 中，并对上下文调用 SaveChanges()/SaveChangesAsync() 来添加到数据库中。当执行 SaveChanges() 时，ChangeTracker 报告所有已添加的实体，EF Core（以及数据库提供程序）创建适当的 SQL 语句以插入记录。

作为提醒，SaveChanges() 在隐式事务中执行，除非使用显式事务。如果保存成功，则服务器生成的<|message|>


## 使用 Attach 添加单条记录

当实体的主键映射到 SQL Server 中的标识列时，如果主键属性的值为零，EF Core 会将该实体实例视为 Added。以下测试创建了一个新的 Car 实体，其 Id 保持默认值零。当该实体附加到 ChangeTracker 时，其状态会被设置为 Added，调用 SaveChanges() 后，实体将被添加到数据库中。

```
[Fact]
public void ShouldAddACarWithAttach()
{
    ExecuteInATransaction(RunTheTest);

    void RunTheTest()
    {
        var car = new Car
        {
            Color = "Yellow",
            MakeId = 1,
            PetName = "Herbie"
        };
        var carCount = Context.Cars.Count();
        Context.Cars.Attach(car);
        Assert.Equal(EntityState.Added, Context.Entry(car).State);
        Context.SaveChanges();
        var newCarCount = Context.Cars.Count();
        Assert.Equal(carCount + 1, newCarCount);
    }
}
```

## 一次添加多条记录

要在单个事务中插入多条记录，请使用 DbSet<T> 的 AddRange() 方法，如以下测试所示（请注意，在 SQL Server 中，持久化数据时若要使用批处理，至少必须执行四个操作）：

```
[Fact]
public void ShouldAddMultipleCars()
{
    ExecuteInATransaction(RunTheTest);

    void RunTheTest()
    {
        //Have to add 4 to activate batching
        var cars = new List<Car>
        {
            new() { Color = "Yellow", MakeId = 1, PetName = "Herbie" },
            new() { Color = "White", MakeId = 2, PetName = "Mach 5" },
            new() { Color = "Pink", MakeId = 3, PetName = "Avon" },
            new() { Color = "Blue", MakeId = 4, PetName = "Blueberry" },
        };

var carCount = Context.Cars.Count();
Context.Cars.AddRange(cars);
Context.SaveChanges();
var newCarCount = Context.Cars.Count();
Assert.Equal(carCount + 4, newCarCount);
}
}
```

添加语句会被批处理为一次数据库调用，并且会查询所有生成的列。当查询结果返回到 EF Core 时，实体会使用服务器端的值进行更新。执行的 SQL 语句如下：

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
INNER JOIN @inserted0 i ON ([t].[Id] = i.[Id])
ORDER BY [i].[_Position];',
N'@p0 nvarchar(50),@p1 int,@p2 nvarchar(50),@p3 nvarchar(50),@p4 int,@p5 nvarchar(50),
@p6 nvarchar(50),@p7 int,@p8 nvarchar(50),@p9 nvarchar(50),@p10 int,@p11 nvarchar(50)',
@p0=N'Yellow',@p1=1,@p2=N'Herbie',@p3=N'White',@p4=2,@p5=N'Mach 5',@p6=N'Pink',@p7=3,
@p8=N'Avon',@p9=N'Blue',@p10=4,@p11=N'Blueberry'
```

## 添加记录时的标识列注意事项

当实体具有一个定义为主键的数值属性时，该属性默认会映射到 SQL Server 中的标识列。对于键属性为默认值（零）的实体，EF Core 会将其视为新实体；对于键属性为非默认值的实体，则会将其视为已存在于数据库中的实体。如果创建新实体时将主键属性设置为非零数字，然后尝试将其添加到数据库，EF Core 将无法添加该记录，因为未启用标识插入。Initialize data 代码演示了如何启用标识插入。

## 添加对象图

向数据库添加实体时，只要将子记录添加到父记录的集合属性中，就可以在同一次调用中添加子记录，而无需将它们显式添加到各自的 DbSet<T> 中。例如，创建一个新的 Make 实体，并将一个子 Car 记录添加到 Make 的 Cars 属性中。当 Make 实体被添加到 DbSet<Make> 属性时，EF Core 也会自动开始跟踪子 Car 记录，而无需将其显式添加到 DbSet<Car> 属性中。执行 SaveChanges() 会同时保存 Make 和 Car。以下测试对此进行了演示：

```
[Fact]
public void ShouldAddAnObjectGraph()
{
    ExecuteInATransaction(RunTheTest);

    void RunTheTest()
    {
        var make = new Make {Name = "Honda"};
        var car = new Car { Color = "Yellow", MakeId = 1, PetName = "Herbie" };
        //Cast the Cars property to List<Car> from IEnumerable<Car>
        ((List<Car>)make.Cars).Add(car);
        Context.Makes.Add(make);
        var carCount = Context.Cars.Count();
        var makeCount = Context.Makes.Count();
        Context.SaveChanges();
        var newCarCount = Context.Cars.Count();
        var newMakeCount = Context.Makes.Count();
        Assert.Equal(carCount+1,newCarCount);
        Assert.Equal(makeCount+1,newMakeCount);
    }
}
```

添加语句不会进行批处理，因为语句数量少于两条，而在 SQL Server 中，批处理从四条语句开始。执行的 SQL 语句如下：

```
exec sp_executesql N'SET NOCOUNT ON;
INSERT INTO [dbo].[Makes] ([Name])
VALUES (@p0);
SELECT [Id], [TimeStamp]
FROM [dbo].[Makes]
WHERE @@ROWCOUNT = 1 AND [Id] = scope_identity();

',N'@p0 nvarchar(50)',@p0=N'Honda'
```

```
exec sp_executesql N'SET NOCOUNT ON;
INSERT INTO [dbo].[Inventory] ([Color], [MakeId], [PetName])
VALUES (@p1, @p2, @p3);
SELECT [Id], [IsDrivable], [TimeStamp]
FROM [dbo].[Inventory]
WHERE @@ROWCOUNT = 1 AND [Id] = scope_identity();

',N'@p1 nvarchar(50),@p2 int,@p3 nvarchar(50)',@p1=N'Yellow',@p2=7,@p3=N'Herbie'
```

## 更新记录

更新记录时，需要先将其作为受跟踪实体加载到 DbSet<T> 中，通过代码对其进行修改，然后在上下文中调用 SaveChanges()。执行 SaveChanges() 时，ChangeTracker 会报告所有已修改的实体，EF Core（以及数据库提供程序）会创建适当的 SQL 语句来更新记录。

## 实体状态

编辑受跟踪实体时，EntityState 会被设置为 Modified。更改成功保存后，其状态会恢复为 Unchanged。

## 更新受跟踪实体

更新单条记录与添加单条记录非常相似。先从数据库中将记录加载为受跟踪实体，进行一些修改，然后调用 SaveChanges()。请注意，由于实体已被跟踪，因此不需要在 DbSet<T> 上调用 Update()/UpdateRange() 方法。以下测试只更新一条记录，但如果更新并保存多个受跟踪实体，过程也是相同的。

```
[Fact]
public void ShouldUpdateACar()
{
    ExecuteInASharedTransaction(RunTheTest);

    void RunTheTest(IDbContextTransaction trans)
    {
        var car = Context.Cars.First(c => c.Id == 1);
        Assert.Equal("Black",car.Color);
        car.Color = "White";
        //Calling update is not needed because the entity is tracked
        //Context.Update(car);
        Context.SaveChanges();
        Assert.Equal("White", car.Color);
        var context2 = TestHelpers.GetSecondContext(Context, trans);
        var car2 = context2.Cars.First(c => c.Id == 1);
        Assert.Equal("White", car2.Color);
    }
}
```

前面的代码在两个 ApplicationDbContext 实例之间使用了共享事务。这样做是为了隔离执行测试的上下文与检查测试结果的上下文。

执行的 SQL 语句如下：

```
exec sp_executesql N'SET NOCOUNT ON;
UPDATE [dbo].[Inventory] SET [Color] = @p0
WHERE [Id] = @p1 AND [TimeStamp] = @p2;
SELECT [TimeStamp]
FROM [dbo].[Inventory]
WHERE @@ROWCOUNT = 1 AND [Id] = @p1;

',N'@p1 int,@p0 nvarchar(50),@p2 varbinary(8)',@p1=1,@p0=N'White',@p2=0x000000000000862D
```

> 注意：前面的 where 子句不仅检查了 Id 列，还检查了 TimeStamp 列。并发检查将在稍后介绍。





## 更新未跟踪的实体

未跟踪的实体同样可以用于更新数据库记录。此过程与更新已跟踪实体类似，不同之处在于，实体是在代码中创建的（而不是通过查询获取的），并且必须通知 EF Core，该实体已存在于数据库中，需要对其进行更新。

创建实体实例后，有两种方式可以通知 EF Core 需要将此实体作为更新进行处理。第一种方式是在 `DbSet<T>` 上调用 `Update()` 方法，将状态设置为 Modified，如下所示：

```
context2.Cars.Update(updatedCar);
```

第二种方式是使用上下文实例和 `Entry()` 方法，将状态设置为 Modified，如下所示：

```
context2.Entry(updatedCar).State = EntityState.Modified;
```

无论采用哪种方式，仍然必须调用 `SaveChanges()`，才能持久化这些值。
下面的示例将一条记录作为未跟踪实体读取，根据该记录创建 `Car` 类的新实例，并修改一个属性（Color）。然后，根据取消注释的代码行，它会设置状态，或者在 `DbSet<T>` 上使用 `Update()` 方法。`Update()` 方法同样会将状态更改为 Modified。随后，测试调用 `SaveChanges()`。这里使用额外上下文的目的，是确保测试准确，并且不同上下文之间不会相互影响。

```
[Fact]
public void ShouldUpdateACarUsingState()
{
    ExecuteInASharedTransaction(RunTheTest);

    void RunTheTest(IDbContextTransaction trans)
    {
        var car = Context.Cars.AsNoTracking().First(c => c.Id == 1);
        Assert.Equal("Black", car.Color);
        var updatedCar = new Car
        {
            Color = "White", //Original is Black
            Id = car.Id,
            MakeId = car.MakeId,
            PetName = car.PetName,
            Timestamp = car.Timestamp,
            IsDrivable = car.IsDrivable
        };
        var context2 = TestHelpers.GetSecondContext(Context, trans);
        //Either call Update or modify the state
        context2.Entry(updatedCar).State = EntityState.Modified;
        //context2.Cars.Update(updatedCar);
        context2.SaveChanges();
        var context3 =
            TestHelpers.GetSecondContext(Context, trans);
        var car2 = context3.Cars.First(c => c.Id == 1);
        Assert.Equal("White", car2.Color);
    }
}
```

执行的 SQL 语句如下：

```
exec sp_executesql N'SET NOCOUNT ON;
UPDATE [dbo].[Inventory] SET [Color] = @p0
WHERE [Id] = @p1 AND [TimeStamp] = @p2;
SELECT [TimeStamp]
FROM [dbo].[Inventory]
WHERE @@ROWCOUNT = 1 AND [Id] = @p1;
',N'@p1 int,@p0 nvarchar(50),@p2 varbinary(8)',@p1=1,@p0=N'White',@p2=0x0000000000000862D
```

## 并发检查

上一章详细介绍了并发检查。提醒一下，当实体定义了 `TimeStamp` 属性时，在将更改（更新或删除）持久化到数据库时，该属性的值会被用于 `where` 子句中。查询不再只搜索主键，而是会将 `TimeStamp` 值添加到查询中，如下例所示：

```
UPDATE [dbo].[Inventory] SET [PetName] = @p0
WHERE [Id] = @p1 AND [TimeStamp] = @p2;
```

下面的测试展示了如何创建并发异常、捕获该异常，并使用 `Entries` 获取原始值、当前值以及数据库中当前存储的值。获取当前数据库值需要再次访问数据库。

```
[Fact]
public void ShouldThrowConcurrencyException()
{
    ExecuteInATransaction(RunTheTest);

    void RunTheTest()
    {
        var car = Context.Cars.First();
        //Update the database outside of the context
        Context.Database.ExecuteSqlInterpolated(
            $"Update dbo.Inventory set Color='Pink' where Id = {car.Id}");
        car.Color = "Yellow";
        var ex = Assert.Throws<CustomConcurrencyException>(
            () => Context.SaveChanges());
        var entry = ((DbUpdateConcurrencyException) ex.InnerException)?.Entries[0];
        PropertyValues originalProps = entry.OriginalValues;
        PropertyValues currentProps = entry.CurrentValues;
        //This needs another database call
        PropertyValues databaseProps = entry.GetDatabaseValues();
    }
}
```

执行的 SQL 调用如下。第一个是更新语句，第二个用于获取数据库值。

```
exec sp_executesql N'SET NOCOUNT ON;
UPDATE [dbo].[Inventory] SET [Color] = @p0
WHERE [Id] = @p1 AND [TimeStamp] = @p2;
SELECT [TimeStamp]
FROM [dbo].[Inventory]
WHERE @@ROWCOUNT = 1 AND [Id] = @p1;'
,N'@p1 int,@p0 nvarchar(50),@p2 varbinary(8)',@p1=1,@p0=N'Yellow',@p2=0x0000000000008665

exec sp_executesql N'SELECT TOP(1) [i].[Id], [i].[Color], [i].[IsDrivable], [i].[MakeId], [i].[PetName], [i].[TimeStamp]
FROM [dbo].[Inventory] AS [i]
WHERE [i].[Id] = @__p_0',N'@__p_0 int',@__p_0=1
```

## 删除记录

通过在 `DbSet<T>` 上调用 `Remove()`，或将实体的状态设置为 Deleted，可以将单个实体标记为删除。通过在 `DbSet<T>` 上调用 `RemoveRange()`，可以将记录列表标记为删除。删除过程会根据 `OnModelCreating()` 中配置的规则（或 EF Core 的约定），对导航属性产生级联影响。如果由于级联策略而阻止删除，则会抛出异常。

## 实体状态

当对正在跟踪的实体调用 `Remove()` 方法时，其 `EntityState` 会被设置为 Deleted。删除语句成功执行后，该实体会从 `ChangeTracker` 中移除，其状态会更改为 Detached。请注意，除非实体超出作用域并被垃圾回收，否则它仍然存在于应用程序中。

## 删除已跟踪的记录

删除过程与更新过程类似。实体被跟踪后，对该实例调用 `Remove()`，然后调用 `SaveChanges()`，即可从数据库中删除记录。

```
[Fact]
public void ShouldRemoveACar()
{
    ExecuteInATransaction(RunTheTest);

    void RunTheTest()
    {
        var carCount = Context.Cars.Count();
        var car = Context.Cars.First(c => c.Id == 2);
        Context.Cars.Remove(car);
        Context.SaveChanges();
        var newCarCount = Context.Cars.Count();
        Assert.Equal(carCount - 1, newCarCount);
        Assert.Equal(
            EntityState.Detached,
            Context.Entry(car).State);
    }
}
```

调用 `SaveChanges()` 后，实体实例仍然存在，但已不再位于 `ChangeTracker` 中。检查 `EntityState` 时，其状态将为 Detached。

删除操作执行的 SQL 调用如下：

exec sp_executesql N'SET NOCOUNT ON; DELETE FROM [dbo].[Inventory] WHERE [Id] = @p0 AND [TimeStamp] = @p1; SELECT @@ROWCOUNT;' ,N'@p0 int,@p1 varbinary(8)',@p0=2,@p1=0x00000000000008680

## 删除未跟踪的实体

未跟踪的实体可以采用与更新记录相同的方式删除记录。不同之处在于，需要通过调用 `Remove()`/`RemoveRange()` 或将状态设置为 Deleted 来跟踪实体，然后调用 `SaveChanges()`。

下面的示例将一条记录作为未跟踪实体读取，根据该记录创建 `Car` 类的新实例，并修改一个属性（Color）。然后，根据取消注释的代码行，它会设置状态，或者在 `DbSet<T>` 上使用 `Remove()` 方法。随后，测试调用 `SaveChanges()`。这里使用额外上下文的目的，是确保不同上下文之间不会相互影响。

```
[Fact]
public void ShouldRemoveACarUsingState()
{
    ExecuteInASharedTransaction(RunTheTest);

    void RunTheTest(IDbContextTransaction trans)
    {
        var carCount = Context.Cars.Count();
        var car = Context.Cars.AsNoTracking().First(c => c.Id == 2);
        var context2 = TestHelpers.GetSecondContext(Context, trans);
        //Either call Remove or modify the state
        context2.Entry(car).State = EntityState.Deleted;
        //context2.Cars.Remove(car);
        context2.SaveChanges();
        var newCarCount = Context.Cars.Count();
        Assert.Equal(carCount - 1, newCarCount);
        Assert.Equal(
            EntityState.Detached,
            Context.Entry(car).State);
    }
}
```

## 捕获级联删除失败

当由于级联规则导致删除记录失败时，EF Core 会抛出 `DbUpdateException`。下面的测试展示了这一过程：

```
[Fact]
public void ShouldFailToRemoveACar()
{
    ExecuteInATransaction(RunTheTest);
```



```csharp
void RunTheTest()
{
    var car = Context.Cars.First(c => c.Id == 1);
    Context.Cars.Remove(car);
    Assert.Throws<CustomDbUpdateException>(
        () => Context.SaveChanges());
}
```

## 并发检查

如果实体具有 `TimeStamp` 属性，删除操作也会使用并发检查。有关更多信息，请参阅“更新记录”一节中的“并发检查”部分。

## 本章小结

本章运用了上一章所学的知识，完成了 AutoLot 数据库的数据访问层。你使用了 EF Core 命令行工具来搭建现有数据库的模型，将模型更新到最终版本，然后创建并应用了迁移。为了封装数据访问，添加了仓储，并且带有示例数据的数据库初始化代码可以以可重复、可靠的方式删除和创建数据库。本章的其余部分重点介绍了对数据访问层的测试驱动开发。至此，我们完成了对数据访问和 Entity Framework Core 的探索之旅。
