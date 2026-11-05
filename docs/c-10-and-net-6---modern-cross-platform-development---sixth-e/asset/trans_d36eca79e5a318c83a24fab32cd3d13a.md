
```
if (exception != null)
{
    Write($"Exception: {exception.Message}");
}
WriteLine();
}
}
```

- 2. 在 Program.cs 中，取消注释 QueryingCategories 方法并注释掉其他方法，以便我们可以监视在连接两个表时生成的 SQL 语句。
- 3. 运行代码，并注意以下已记录的 SQL 语句，如以下为节省空间而编辑过的输出所示：

```
Using SQLServer database provider.
Categories and how many products they have:
Level: Debug, Event Id: 20100, State: Executing DbCommand [Parameters=[], CommandType='Text', CommandTimeout='30']
SELECT [c].[CategoryId], [c].[CategoryName], [c].[Description], [p].[ProductId], [p].[CategoryId], [p].[UnitPrice], [p].[Discontinued], [p].[ProductName], [p].[UnitsInStock]
FROM [Categories] AS [c]
LEFT JOIN [Products] AS [p] ON [c].[CategoryId] = [p].[CategoryId]
ORDER BY [c].[CategoryId], [p].[ProductId]
Beverages has 12 products.
Condiments has 12 products.
Confections has 13 products.
Dairy Products has 10 products.
Grains/Cereals has 7 products.
Meat/Poultry has 6 products.
Produce has 5 products.
Seafood has 12 products.
```

## 使用查询标签记录日志

在记录 LINQ 查询时，在复杂场景中关联日志消息可能很棘手。EF Core 2.2 引入了查询标签功能来帮助解决此问题，允许您向日志添加 SQL 注释。

您可以使用 TagWith 方法来注释 LINQ 查询，如以下代码所示：

```
IQueryable<Product> products = db.Products
    .TagWith("Products filtered by price and sorted.")
    .Where(product => product.Cost > price)
    .OrderByDescending(product => product.Cost);
```

这将在日志中添加一条 SQL 注释，如以下输出所示：

```
-- Products filtered by price and sorted.
```

## 使用 Like 进行模式匹配

EF Core 支持常见的 SQL 语句，包括用于模式匹配的 Like：

1. 在 Program.cs 底部添加一个名为 QueryingWithLike 的方法，如以下代码所示，并注意：
- 我们已启用日志记录。
- 我们提示用户输入产品名称的一部分，然后使用 EF.Functions.Like 方法在 ProductName 属性中任意位置进行搜索。
- 对于每个匹配的产品，我们输出其名称、库存以及是否已停产：

```csharp
static void QueryingWithLike()
{
    using (Northwind db = new())
    {
        ILoggerFactory loggerFactory = db.GetService<ILoggerFactory>();
        loggerFactory.AddProvider(new ConsoleLoggerProvider());

        Write("Enter part of a product name: ");
        string? input = ReadLine();

        IQueryable<Product> products = db.Products
            .Where(p => EF.Functions.Like(p.ProductName, $"%{input}%"));

        if (products is null)
        {
            WriteLine("No products found.");
            return;
        }

        foreach (Product p in products)
        {
            WriteLine("{0} has {1} units in stock. Discontinued? {2}",
                p.ProductName, p.Stock, p.Discontinued);
        }
    }
}
```

2. 在 Program.cs 中注释掉现有方法，并调用 QueryingWithLike。

3. 运行代码，输入部分产品名称，例如 che，并查看结果，如以下输出所示：

```
Using SQLServer database provider.
Enter part of a product name: che
Level: Debug, Event Id: 20100, State: Executing DbCommand [Parameters=[@__Format_1='?' (Size = 40)], CommandType='Text', CommandTimeout='30']
SELECT "p"."ProductId", "p"."CategoryId", "p"."UnitPrice",
"p"."Discontinued", "p"."ProductName", "p"."UnitsInStock" FROM "Products"
AS "p"
WHERE "p"."ProductName" LIKE @__Format_1
Chef Anton's Cajun Seasoning has 53 units in stock. Discontinued? False
Chef Anton's Gumbo Mix has 0 units in stock. Discontinued? True
Queso Manchego La Pastora has 86 units in stock. Discontinued? False
Gumbär Gummibärchen has 15 units in stock. Discontinued? False
```

> EF Core 6.0 引入了另一个有用的函数 EF.Functions.Random，它映射到一个数据库函数，该函数返回 0 到 1 之间的伪随机数（不包含边界）。例如，您可以将随机数乘以表中行的计数来从该表中选择一行随机行。

## 定义全局筛选器

Northwind 产品可能已停产，因此可能需要确保已停产的产品永远不会在结果中返回，即使程序员在查询中未使用 where 过滤它们也是如此：

1. 在 Northwind.cs 中修改 OnModelCreating 方法以添加一个全局筛选器来移除已停产的产品，如以下代码中突出显示所示：

```
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
...
// global filter to remove discontinued products
modelBuilder.Entity<Product>()
.HasQueryFilter(p => !p.Discontinued);
}
```

2. 运行代码，输入部分产品名称 che，查看结果，并注意 Chef Anton's Gumbo Mix 现在不见了，因为生成的 SQL 语句包含对 Discontinued 列的筛选器，如以下输出中突出显示所示：

```
SELECT "p"."ProductId", "p"."CategoryId", "p"."UnitPrice",
"p"."Discontinued", "p"."ProductName", "p"."UnitsInStock"
```

```
FROM "Products" AS "p"
WHERE ("p"."Discontinued" = 0) AND "p"."ProductName" LIKE @__Format_1
Chef Anton's Cajun Seasoning has 53 units in stock. Discontinued? False
Queso Manchego La Pastora has 86 units in stock. Discontinued? False
Gumbär Gummibärchen has 15 units in stock. Discontinued? False
```

## EF Core 中的加载模式

EF Core 中常用的有三种加载模式：

- **预先加载：** 提前加载数据。
- **延迟加载：** 在需要数据之前立即自动加载数据。
- **显式加载：** 手动加载数据。

在本节中，我们将介绍它们各自。

### 预先加载实体

在 QueryingCategories 方法中，代码当前使用 Categories 属性遍历每个类别，输出类别名称和该类别中的产品数量。

这是因为在编写查询时，我们通过为相关产品调用 Include 方法启用了预先加载。

让我们看看如果不调用 Include 会发生什么：

- 1. 修改查询以注释掉 Include 方法调用，如以下代码所示：
- 2. 在 Program.cs 中注释掉 QueryingCategories 之外的所有方法。
- 3. 运行代码并查看结果，如以下部分输出所示：

```
IQueryable<Category> categories =
    db.Categories; //.Include(c => c.Products);
```

```
Beverages has 0 products.
Condiments has 0 products.
Confections has 0 products.
Dairy Products has 0 products.
Grains/Cereals has 0 products.
Meat/Poultry has 0 products.
Produce has 0 products.
Seafood has 0 products.
```

foreach 中的每个项都是 Category 类的一个实例，该类有一个名为 Products 的属性，即该类别中的产品列表。由于原始查询仅从 Categories 表中选择，因此此属性对每个类别都是空的。

### 启用延迟加载

延迟加载在 EF Core 2.1 中引入，可以自动加载缺失的相关数据。要启用延迟加载，开发人员必须：

- 引用代理的 NuGet 包。
- 配置延迟加载以使用代理。

让我们在实践中看看：

1. 在 WorkingWithEFCore 项目中添加 EF Core 代理的包引用，如以下标记所示：

```
<PackageReference
  Include="Microsoft.EntityFrameworkCore.Proxies"
  Version="6.0.0" />
```

2. 构建项目以还原包。

3. 打开 Northwind.cs，并在 OnConfiguring 方法顶部调用一个扩展方法以使用延迟加载代理，如以下代码中突出显示所示：

```
protected override void OnConfiguring(
  DbContextOptionsBuilder optionsBuilder)
{
  optionsBuilder.UseLazyLoadingProxies();
}
```

现在，每次循环枚举时，并且尝试读取 Products 属性时，延迟加载代理将检查它们是否已加载。如果没有，它将通过执行 SELECT 语句为我们“延迟”加载它们，以仅加载当前类别的产品集，然后正确的计数将返回到输出。

4. 运行代码并注意产品计数现在是正确的。但您会看到延迟加载的问题是需要多次往返数据库服务器才能最终获取所有数据，如以下部分输出所示：

```
Categories and how many products they have:
Level: Debug, Event Id: 20100, State: Executing DbCommand [Parameters=[]],
CommandType='Text', CommandTimeout='30']
SELECT "c"."CategoryId", "c"."CategoryName", "c"."Description" FROM
"Categories" AS "c"
Level: Debug, Event Id: 20100, State: Executing DbCommand [Parameters=[@p_0='?']], CommandType='Text', CommandTimeout='30']
SELECT "p"."ProductId", "p"."CategoryId", "p"."UnitPrice",
"p"."Discontinued", "p"."ProductName", "p"."UnitsInStock"
```
