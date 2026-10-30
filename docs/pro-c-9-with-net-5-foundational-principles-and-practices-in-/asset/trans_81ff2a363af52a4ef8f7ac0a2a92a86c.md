

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

