

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

