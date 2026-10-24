

现在是时候查看 `IShoppingCartStore` 接口的实现。首先，让我们考虑从数据库读取购物车的代码。

## Listing 6.1  使用 Dapper 读取购物车

```
namespace ShoppingCart.ShoppingCart
{
    using System.Data;
    using System.Data.SqlClient;
    using System.Linq;
    using System.Threading.Tasks;
    using Dapper;

    public interface IShoppingCartStore
    {
        Task<ShoppingCart> Get(int userId);
        Task Save(ShoppingCart shoppingCart);
    }

    public class ShoppingCartStore : IShoppingCartStore
    {
        private string connectionString =
            @"Data Source=localhost;Initial Catalog=ShoppingCart;
User Id=SA; Password=yourStrong(!)Password";

        private const string readItemsSql =
            @"
select ShoppingCart.ID, ProductCatalogId,
ProductName, ProductDescription, Currency, Amount
from ShoppingCart, ShoppingCartItem
where ShoppingCartItem.ShoppingCartId = ShoppingCart.ID
and ShoppingCart.UserId=@userId";

        public async Task<ShoppingCart> Get(int userId)
        {
            await using var conn = new SqlConnection(this.connectionString);
            var items = (await
                conn.QueryAsync(
                    readItemsSql,
                    new { UserId = userId }))
                .ToList();
            return new ShoppingCart(
                items.FirstOrDefault()?.ID,
                userId,
                items.Select(x =>
                    new ShoppingCartItem(
                        (int)x.ProductCatalogId,
                        x.ProductName,
                        x.ProductDescription,
                        new Money(x.Currency, x.Amount))));
        }
    }
}
```

> 连接到 MS SQL Docker 容器中 ShoppingCart 数据库的连接字符串

> Dapper 希望你编写自己的 SQL，并且允许你这样做。

> 打开到 ShoppingCart 数据库的连接

> 使用 Dapper 扩展方法执行 SQL 查询

> SQL 查询到 ShoppingCartItem 的结果集

Dapper 是一个简单的工具，它在 IDbConnection 上提供了一些方便的扩展方法，使在 C# 中使用 SQL 变得更容易。它还提供了一些基本映射功能。例如，当 SQL 查询返回的行具有与类中属性名称相同的列名时，Dapper 可以自动映射到该类的实例。

Dapper 并没有试图隐藏你正在使用 SQL 的事实，因此代码中可以看到 SQL 字符串。这可能感觉像是 .NET 最早期的倒退。我发现只要我使用的是简单的数据库模式——正如我在微服务中通常做的那样——C# 代码中的 SQL 字符串并不是问题。

通过 Dapper 将购物车写入数据库也是如此。实现是 ShoppingCartStore 中的以下方法。

## Listing 6.2  使用 Dapper 写入购物车

```
private const string insertShoppingCartSql =
@"insert into ShoppingCart (UserId) OUTPUT inserted.ID VALUES (@UserId)";

private const string deleteAllForShoppingCartSql =
@"delete item from ShoppingCartItem item
inner join ShoppingCart cart on item.ShoppingCartId = cart.ID
and cart.UserId=@UserId";

private const string addAllForShoppingCartSql =
@"insert into ShoppingCartItem
(ShoppingCartId, ProductCatalogId, ProductName,
ProductDescription, Amount, Currency)
values
(@ShoppingCartId, @ProductCatalogId, @ProductName,
@ProductDescription, @Amount, @Currency)";

public async Task Save(ShoppingCart shoppingCart)
{
    await using var conn = new SqlConnection(this.connectionString);
    await conn.OpenAsync();
    await using (var tx = conn.BeginTransaction())
    {
        var shoppingCartId =
            shoppingCart.Id ??
                await conn.QuerySingleAsync<int>(
                    insertShoppingCartSql,
                    new { shoppingCart.UserId }, tx);

        await conn.ExecuteAsync(
            deleteAllForShoppingCartSql,
            new { UserId = shoppingCart.UserId },
            tx);
        await conn.ExecuteAsync(
            addAllForShoppingCartSql,
            shoppingCart.Items.Select(x =>
                new
                {
                    shoppingCartId,
```

> 如果购物车尚无 Id，则在 ShoppingCart 表中创建一行。

> 删除所有预先存在的购物车项目

> 添加当前的购物车项目

到系统——用户做了一些在业务上很有趣的事情——而后者则报告技术细节——一段软件做了某事，因为程序员决定那样实现它。事件应该在微服务实现的功能的抽象级别上具有重要意义，并且它们通常涵盖对底层数据库的多次更新。事件应对应业务级事务，而不是数据库事务。

每当微服务中的领域逻辑引发事件时，它都会存储到微服务中的数据库。在图 6.8 中，这是通过 EventStore 组件完成的，该组件负责与存储事件的数据库对话。

![](img/608267d7803dc57a1a896ad121f37868_167_0.png)

图 6.8 当领域模型引发事件时，EventStore 组件代码必须将其写入数据库。

以下两个部分展示了 EventStore 组件的两种实现。第一种手动将事件存储到 SQL 数据库中的表中，第二种使用开源的 EventStoreDB。

## 手动存储事件

在这里，你将在购物车微服务中构建 EventStore 组件的实现，该实现将事件存储到 SQL Server 中的表中。EventStore 组件负责向该数据库写入事件并从中读取事件。

实现 EventStore 组件涉及以下步骤：

- 1. 在 ShoppingCart 数据库中添加一个 EventStore 表。该表将包含领域模型引发的每个事件的一行。
- 2. 使用 Dapper 实现 EventStore 组件的写入部分。
- 3. 使用 Dapper 实现 EventStore 组件的读取部分。

在深入实现 EventStore 组件之前，这里提醒一下购物车中 Event 类型的样子。

## Listing 6.3  ShoppingCart 微服务中的 Event 类型

```
public record Event(
    long SequenceNumber,
    DateTimeOffset OccuredAt,
    string Name,
    object Content);
```

这是你将在 ShoppingCart 数据库中存储的事件类型。第一步是进入 ShoppingCart 数据库并添加如图 6.9 所示的表。代码下载中文件 `Chapter06\ShoppingCart\database-scripts\create-shopping-cart-db.sql` 中的数据库脚本创建了此表以及 ShoppingCart 数据库中的另外两个表。

| EventStore |  |
|---|---|
| **PK** | **ID** |
|  | Name |
|  | OccuredAt |
|  | Content |

**图 6.9**  **EventStore** 表有四列用于这些类别：事件 ID、事件名称、事件发生时间以及事件内容。

接下来，将名为 EventStore.cs 的文件添加到购物车中，并向其中添加以下用于写入事件的代码。

```
```csharp
namespace ShoppingCart.EventFeed
{
    using System;
    using System.Collections.Generic;
    using System.Data.SqlClient;
    using System.Text.Json;
    using System.Threading.Tasks;
    using Dapper;

    public interface IEventStore
    {
        Task<IEnumerable<Event>> GetEvents(long firstEventSequenceNumber,
            long lastEventSequenceNumber);
        Task Raise(string eventName, object content);
    }

    public class EventStore : IEventStore
    {
        private string connectionString =
            @"Data Source=localhost;Initial Catalog=ShoppingCart;
User Id=SA; Password=yourStrong(!)Password";

        private const string writeEventSql =
            @"insert into EventStore(Name, OccuredAt, Content)
values (@Name, @OccuredAt, @Content)";

        public async Task Raise(string eventName, object content)
        {
            var jsonContent = JsonSerializer.Serialize(content);
```
