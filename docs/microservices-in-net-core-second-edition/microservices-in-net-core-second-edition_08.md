## Data ownership and data storage

### This chapter covers

- Which data microservices store
- Understanding how data ownership follows business capabilities
- Using data replication for speed and robustness
- Building read models from event feeds with event subscribers
- Implementing data storage in microservices

软件系统创建、使用和转换数据。如果没有数据，大多数软件系统就没有什么价值，微服务系统也是如此。在本章中，您将学习数据应存储在何处以及哪个微服务应负责使其保持最新。此外，您还将学习如何使用数据复制使微服务系统更具健壮性和更快。

## 6.1 Each microservice has a data store

第一章中确定的微服务的一个特征是，每个微服务都应拥有自己的数据存储。该数据存储中的数据完全由微服务控制，并且正是微服务所需的数据。首先，它是属于微服务实现的能力的数据，但它也是支持数据，例如从事件源创建的缓存数据和读模型。

每个微服务都拥有数据存储这一事实意味着您不需要为所有微服务使用相同的数据库技术。您可以为每个微服务选择适合其需要存储的数据的数据库技术。

微服务通常需要存储三种类型的数据：

- 属于微服务实现的能力的数据。这是微服务负责并必须保持安全和最新的数据。
- 微服务引发的事件。在命令处理期间，微服务可能需要引发事件以通知系统其余部分有关其负责的数据的更新。
- 基于来自其他微服务的事件中的数据或偶尔基于对其他微服务的查询的数据的读模型。

这三种类型的数据可能存储在不同的数据库中，甚至是不同类型的数据库中。

## 6.2 Partitioning data between microservices

当您决定在微服务系统中存储数据的地点时，会有相互竞争的因素在起作用。两个主要因素是数据所有权和局部性：

- *数据所有权*意味着负责保持数据的正确、安全和最新。
- *数据局部性*指的是微服务所需的数据存储的位置。通常，数据应存储在附近——最好是在微服务本身中。

这两个因素可能存在冲突，为了同时满足两者，您通常需要在多个地方存储数据。这是可以的，但重要的是其中只有一个地方被视为权威来源。图 6.1 说明了这一点；而一个微服务存储数据的权威副本，其他微服务可以在自己的数据存储中镜像该数据。

![](img/608267d7803dc57a1a896ad121f37868_152_0.png)

Figure 6.1 Microservices A and C collaborate with microservice B. Microservices A and C can store mirrors of the data owned by microservice B, but the authoritative copy is stored in microservice B’s own data store.

cart. Most of the microservices in figure 6.2 are dimmed to put the focus on three microservices: shopping cart, product catalog, and recommendations.

图 6.2 中突出显示的每个微服务都处理一个业务能力：购物车微服务负责跟踪用户的购物车；产品目录微服务负责为系统其余部分提供产品目录的信息访问；推荐微服务负责为电子商务网站的用户计算并提供产品推荐。数据与这些业务能力相关联，每个微服务都拥有并负责与其能力相关的数据。图 6.3 显示了每个微服务拥有的数据。说一个微服务拥有某块数据意味着它必须存储该数据并成为该数据的权威来源。

![](img/608267d7803dc57a1a896ad121f37868_154_0.png)

Figure 6.3 Each microservice owns the data belonging to the business capability it implements.

### 6.2.2 Rule 2: Replicate for speed and robustness

在微服务系统中决定数据应存储在哪里的第二个因素是局部性。微服务查询自己的数据库获取数据与微服务查询另一个微服务获取相同数据之间存在很大差异。查询自己的数据库通常比查询另一个微服务更快、更可靠。

一旦您决定了数据的所有权，您可能会发现您的微服务需要互相请求数据。这种类型的协作会产生一定的耦合：一个微服务查询另一个微服务意味着第一个与另一个耦合。如果第二个微服务宕机或变慢，第一个微服务将受到影响。

为了放松这种耦合，您可以缓存查询响应。有时您会按原样缓存响应，但有时您可以存储基于查询响应的读模型。在这两种情况下，您必须决定何时以及如何缓存数据



变得无效。拥有数据的微服务最适合判断数据的有效期。因此，响应微服务所拥有数据的查询时，应在响应中包含缓存头，告知调用方应该缓存响应数据多长时间。

## 使用 HTTP 缓存头控制缓存

HTTP 定义了若干用于控制 HTTP 响应如何缓存的头部。HTTP 缓存机制有两个目的：

- 在很多情况下，消除调用方再次请求已有信息的需求
- 在另一些情况下，消除发送完整 HTTP 响应的需求

为了消除调用方请求已有信息的需求，服务器可以在响应中添加 `cache-control` 头。HTTP 规范定义了可以在 `cache-control` 头中设置的一系列控制指令。最常见的是 `private`/`public` 和 `max-age` 指令。前者指示是否只有调用方（`private`）或任何缓存/中间层（例如代理服务器）也可以缓存响应。`max-age` 指令指示响应可以被缓存多少秒。例如，下面的 `cache-control` 头指示调用方（且仅限调用方）可以将响应缓存 3,600 秒：

```
cache-control: private, max-age=3600
```

换句话说，调用方在 3,600 秒内，只要想对相同 URL 发起相同方法（GET、POST、PUT、DELETE）且相同请求体的 HTTP 请求，就可以复用该响应。在本书中，我只对查询使用缓存，这意味着仅用于 GET 请求。值得注意的是，查询字符串是 URL 的一部分，因此缓存会将查询字符串考虑在内。

为了在调用方持有缓存但已过期响应时避免发送完整响应，可以使用 `ETag` 和 `If-None-Match` 头：服务器可以在响应中添加 `ETag` 头。这是响应的标识符。当调用方之后使用相同方法和相同请求体向相同 URL 发起请求时，可以在名为 `If-None-Match` 的请求头中包含该 ETag。服务器可以读取 ETag，得知调用方缓存的是哪个响应。如果服务器判定缓存响应仍然有效，它可以返回带有“304 Not Modified”状态码的响应，告知客户端使用已经缓存的响应。此外，服务器可以在 304 响应中添加 `cache-control` 头，以延长响应可被缓存的时间。注意，ETag 由服务器设置，之后也由同一服务器读取。

让我们再次看图 6.3 中的微服务。购物车微服务通过查询产品目录微服务来获取产品信息。对于任何给定产品，产品目录信息可能正确的时长，最好由拥有数据的产品目录来决定。因此，产品目录应该在其响应中添加缓存头，购物车应该利用这些头来决定可以缓存响应多长时间。图 6.4 展示了购物车想要向产品目录发起的一系列请求。

![](img/608267d7803dc57a1a896ad121f37868_156_0.png)

**图 6.4** 产品目录微服务可以通过在其 HTTP 响应中包含缓存头，让协作方缓存响应。在本示例中，它设置 `max-age` 来指示响应可以缓存多久，并且还包含一个由产品 ID 和版本构建的 ETag。

在图 6.4 中，第一个请求的响应中的缓存头告诉购物车微服务，它可以将响应缓存 3,600 秒。当购物车第二次想要发起相同请求时，由于经过的时间少于 3,600 秒，因此复用了缓存响应。第三次，由于经过的时间超过 3,600 秒，因此向产品目录微服务发起了请求。该请求在 `If-None-Match` 头中包含了来自第一个响应的 ETag。产品目录使用 ETag 判断响应仍然相同，因此返回

![](img/608267d7803dc57a1a896ad121f37868_158_0.png)

**图 6.5** 购物者跟踪微服务订阅购物车微服务的事件，并跟踪产品被添加到购物车或从购物车移除的次数。

添加到购物车或从购物车移除。但事件是构建这种模型的良好来源。购物者跟踪微服务为每个产品维护两个计数器：一个用于记录产品被添加到购物车的次数，另一个用于记录被移除的次数。每次从购物车收到事件，就会更新其中一个计数器；每次对产品进行查询时，就会读取该产品的两个计数器。

## 6.2.3 微服务将数据存储在哪里？

一个微服务可以使用一个、两个或更多数据库。微服务存储的某些数据可能非常适合某种数据库类型，而其他数据可能更适合另一种类型。有许多可行的数据库技术可供选择，这里我不做比较。不过，在选择时，你可以考虑一些广泛的数据库类别，包括关系型数据库、键值存储、文档数据库、列存储和图数据库。

如果你的生产环境位于某个主要公有云中，你可以利用一些非常有吸引力的数据库服务。这些服务在数据库类别（关系型数据库、文档数据库等）上有所不同，在运维特性上也有所不同。有些是“无服务器”的，几乎不需要维护；另一些则更为传统。这些因素也应纳入考虑。

微服务所选数据库技术（一种或多种）可能受到许多因素的影响，包括：

-   你的数据是什么形状？它是否适合关系模型、文档模型、键值存储，还是图结构？
-   写入场景是怎样的？写入的数据量有多大？写入是突发的，还是在时间上均匀分布？
-   读取场景是怎样的？每次读取多少数据？总共读取多少？读取是否是突发的？
-   与读取相比，写入的数据量如何？
-   团队已经熟悉开发和在生产中运行哪些数据库？

问自己这些问题——并找到答案——不仅有助于你选择合适的数据库，而且很可能会加深你对微服务预期具备的非功能性质量的理解。你会了解微服务必须有多可靠、必须处理多大的负载、负载是什么样、可接受的延迟是多少，等等。

获得这种更深层次的理解很有价值，但请注意，我并不建议你每次创建新的微服务时都针对不同数据库的优缺点进行重大分析。你应该能够快速启动一个新的微服务并部署到生产环境。目标不是找到一种完美适合该工作的数据库技术——你只需要根据前面问题的答案找到一种合适的即可。你可能面临这样的情况：文档数据库看起来是个不错的选择，并且你有信心 Couchbase、CosmosDB 和 MongoDB 都非常适合。在这种情况下，选择其中之一即可。最好能快速让微服务使用其中一个进入生产环境，并在稍后阶段，可能用使用另一个的实现替换该微服务，而不是因为你在详细分析 Couchbase、CosmosDB 和 MongoDB 而推迟微服务第一版发布到生产环境。

### 系统中有多少种数据库技术？

关于应该在微服务中使用哪种数据库的决定，并不仅仅取决于哪种数据库适合该微服务。你需要考虑更广泛的情况。在微服务系统中，会有许多微服务和许多数据存储。值得考虑的是，你希望在系统中有多少种不同的数据库技术。在标准化少数几种数据库技术和放任自由之间有一个权衡。

支持标准化的一侧有这些目标：

-   在生产环境中可靠地运行数据库，并长期持续做到这一点
-   开发人员能够进入他们以前没有接触过的微服务代码库并高效工作





偏向自由竞争的模式具有以下类型的目标：

- 能够根据可维护性、性能、安全、可靠性等方面，为每个微服务选择最优的数据库技术。
- 保持微服务的可替换性。如果新开发者接手一个微服务并且不同意数据库的选择，他们应该能够替换数据库，甚至替换整个微服务。

这些目标之间的权衡因组织而异。认识到存在权衡非常重要，但我会进一步建议在组织内部进行一定的标准化。

## 6.3 在微服务中实现数据存储

我们已经讨论了在微服务系统中数据应该放在哪里，包括微服务应该拥有哪种数据以及镜像哪种数据。现在是时候转换思路，来看存储数据所需的代码。

我将重点介绍微服务如何存储其拥有的数据，包括如何存储其引发的事件。我将首先向你展示如何使用 SQL Server 和轻量级数据访问库 Dapper 来实现这一点。然后我将展示如何将事件存储在专门为存储事件而设计的数据库中——名为 EventStoreDB 的数据库。

## 本章使用的新技术

在本章中，你将开始使用几种新技术：

- SQL Server——微软的 SQL 数据库。就本章目的而言，许多不同的数据库都可以使用。我选择 SQL Server，因为我怀疑它对许多读者来说会很熟悉。
- Dapper (https://github.com/StackExchange/dapper-dot-net)—一个轻量级对象关系映射器（ORM）。
- EventStoreDB (https://www.eventstore.com/eventstoredb)—一种专门为存储事件而设计的数据库产品。

## Dapper：轻量级 O/RM

Dapper 是一个用于从 C# 在 SQL 数据库中工作数据的简单库。它属于有时被称为微 ORM 的库家族，其中还包括 Simple.Data 和 Massive。这些库专注于易用性和速度，并拥抱 SQL。

而更传统的 ORM 会编写读取和写入数据库所需的所有 SQL，Dapper 要求你自己编写 SQL。当处理具有简单模式的数据库时，我发现这种方式很解放。

为了选择微服务的轻量级技术，我选择 Dapper 而不是 Entity Framework 或 NHibernate 等全功能 ORM。通常微服务的数据库很简单，在这种情况下，我发现为其添加一个薄层——比如 Dapper——整体上更容易实现。我本可以选用其他微 ORM 中的任何一个，但我喜欢 Dapper。在本章中，你将使用 Dapper 与 SQL Server 通信，但 Dapper 也可与 PostgreSQL、MySQL 或 Azure SQL 等其他 SQL 数据库一起使用。

## EventStoreDB：专用事件数据库

EventStoreDB 是一个专门为存储事件而设计的开源数据库服务器。EventStoreDB 将事件存储为 JSON 文档，但它与文档数据库不同，因为它假设 JSON 文档是事件流的一部分。尽管 EventStoreDB 由于其对存储事件的高度专注而是一个利基产品，但它被广泛使用，并在高负载生产场景中证明了自己。

除了存储事件之外，EventStoreDB 还提供读取和订阅事件的功能。例如，EventStoreDB 公开其自己的事件源——作为 ATOM 源——客户端可以订阅。如果不介意依赖 EventStoreDB，使用其 ATOM 事件源向其他微服务公开事件可以成为本书中你将实现事件源的一种可行替代方案。

EventStoreDB 通过公开用于存储、读取和订阅事件的 HTTP API 来工作。有许多 EventStoreDB 客户端库使用各种语言——包括 C#、F#、Java、Scala、Erlang、Haskell 和 JavaScript——使与数据库的工作变得更容易。

### 6.3.1 准备开发环境

在我们深入实现之前，我们需要在本地主机上运行 SQL Server，我们将在 Docker 容器中执行此操作。

首先，将最新的 SQL Server docker 镜像拉取到你的机器上

```
docker pull mcr.microsoft.com/mssql/server
```

然后运行它：

```
docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=yourStrong(!)Password' -p 1433:1433 -d mcr.microsoft.com/mssql/server
```

最后，通过列出本地运行的容器并检查 SQL Server 是否在列表中来确认 SQL 是否确实正在运行：

```
docker ps

CONTAINER ID        IMAGE                              COMMAND                  CREATED          STATUS          PORTS                    NAMES
3388b710892f        mcr.microsoft.com/mssql/server    "/opt/mssql/bin/perm..."  52 minutes ago   Up 52 minutes   0.0.0.0:1433->1433/tcp   friendly_bell
```

如果 SQL Server 容器未启动，你可能需要在 Docker run 命令中使用双引号而不是单引号。

有了这个，我们就可以开始在我们的微服务中实现数据存储了。

## 6.3.2 存储微服务拥有的数据

一旦你决定了微服务拥有哪些数据，存储这些数据就相对直接了。如何实现取决于你选择的数据库。特定于微服务的唯一区别是数据存储完全由微服务本身拥有和访问。

作为示例，让我们回到购物车微服务。它拥有用户的购物车，因此存储它们。你将使用 Dapper 在 SQL Server 中存储购物车。

你在第 2 章中实现了购物车微服务的大部分内容。在这里，你将填入数据存储部分。

> **NOTE**  我们使用 SQL Server 作为示例，但我们对 SQL Server 所做的一切同样可以用任何其他关系数据库完成，无论是 PostgreSQL、MySQL，还是 Azure SQL 或 AWS Aurora 等云托管关系数据库。

如果你熟悉在 SQL Server 中存储数据，实现应该不会让你感到惊讶，这正是重点。存储微服务拥有的数据不需要涉及任何花哨的东西。存储购物车的步骤如下：

- 1. 创建一个数据库。
- 2. 使用 Dapper 实现读取、写入和更新购物车的代码。

首先，你将创建一个用于存储购物车的简单数据库。它将有两个表，如图 6.6 所示。

ShoppingCart 数据库和两个表——ShoppingCart 和 ShoppingCartItem——可以用 SQL 脚本创建，你可以在连接到本地主机端口 1433 上的 SQL Server 容器后，使用各种工具（包括 SQL Management Studio 和 Visual Studio Code）执行这些脚本：

```
CREATE DATABASE ShoppingCart
GO

USE [ShoppingCart]
GO

CREATE TABLE [dbo].[ShoppingCart] (
    [ID] int IDENTITY(1,1) PRIMARY KEY,
    [UserId] [bigint] NOT NULL,
    CONSTRAINT ShoppingCartUnique UNIQUE ([ID], [UserID])
)
GO

CREATE INDEX ShoppingCart_UserId
ON [dbo].[ShoppingCart] (UserId)
GO
```

| ShoppingCart |  |
|---|---|
| **PK** | **ID** |
|  | UserId |

| ShoppingCartItem |  |
|---|---|
| **PK** | **ID** |
| **FK** | ShoppingCartId |
|  | ProductCatalogId |
|  | ProductName |
|  | ProductDescription |
|  | Amount |
|  | Currency |

![](img/608267d7803dc57a1a896ad121f37868_162_0.png)

```
CREATE TABLE [dbo].[ShoppingCartItem] (
    [ID] int IDENTITY(1,1) PRIMARY KEY,
    [ShoppingCartId] [int] NOT NULL,
    [ProductCatalogId] [bigint] NOT NULL,
    [ProductName] [nvarchar](100) NOT NULL,
    [ProductDescription] [nvarchar](500) NULL,
    [Amount] [int] NOT NULL,
    [Currency] [nvarchar](5) NOT NULL
)
GO

ALTER TABLE [dbo].[ShoppingCartItem] WITH CHECK ADD CONSTRAINT [
    FK_ShoppingCart] FOREIGN KEY([ShoppingCartId])
REFERENCES [dbo].[ShoppingCart] ([Id])
GO

ALTER TABLE [dbo].[ShoppingCartItem] CHECK CONSTRAINT [FK_ShoppingCart]
GO

CREATE INDEX ShoppingCartItem_ShoppingCartId
ON [dbo].[ShoppingCartItem] (ShoppingCartId)
GO
```

> NOTE 如果你连接容器中的 SQL Server 时遇到问题，可能是因为它使用了一个开发证书，你必须显式信任该证书。

数据库就绪后，你可以在购物车微服务中实现读取、写入和更新数据库的代码。你将把 Dapper NuGet 包安装到微服务中。记住，你可以通过在与 ShoppingCart.csproj 文件相同的文件夹中从命令行使用 dotnet add package dapper 命令，将 Dapper 添加到 ShoppingCart 项目来完成此操作。添加 Dapper 包后，包含包引用的项目组，ShoppingCart.csproj 文件应如下所示：

```
<ItemGroup>
  <PackageReference Include="Dapper" Version="2.0.35" />
  <PackageReference Include="Microsoft.Extensions.Http.Polly" Version="3.1.0" />
  <PackageReference Include="Polly" Version="7.2.0" />
  <PackageReference Include="Scrutor" Version="3.1.0" />
</ItemGroup>
```

添加 Dapper 库

在第 2 章中，购物车微服务期望一个 IShoppingCart 接口的实现。你将稍微修改该接口以允许其实现异步调用数据库。这是修改后的接口：

```
public interface IShoppingCartStore
{
    Task<ShoppingCart> Get(int userId);
    Task Save(ShoppingCart shoppingCart);
}
```




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




```csharp
await using var conn = new SqlConnection(this.connectionString);
await conn.ExecuteAsync(
    writeEventSql,
    new
    {
        Name = eventName,
        OccurredAt = DateTimeOffset.Now,
        Content = jsonContent
    });
}
```

这段代码无法编译，因为 EventStore 接口还有另一个方法，用于读取事件。读取部分的实现如下所示。

### Listing 6.5 EventStore 用于读取事件的方法

```csharp
private const string readEventsSql =
    @"select * from EventStore where ID >= @Start and ID <= @End";

public async Task<IEnumerable<Event>> GetEvents(
    long firstEventSequenceNumber,
    long lastEventSequenceNumber)
{
    await using var conn = new SqlConnection(this.connectionString);
    return await conn.QueryAsync<Event>(
        readEventsSql,
        new
        {
            Start = firstEventSequenceNumber,
            End = lastEventSequenceNumber
        });
}
```

> **NOTE** 存储事件本质上相当于在 EventStore 表的一行中存储事件内容的 JSON 序列化结果，同时附带事件的 ID、事件名称以及事件触发的时间。存储事件并通过事件 feed 发布它们的概念可能是新的，但实现起来相当简单。

这就是实现一个基本事件存储所需的一切。购物车微服务现在可以在领域模型中触发事件，并依赖 EventStore 组件将其写入 ShoppingCart 数据库中的 EventStore 表。此外，购物车有一个事件 feed，你在第 2 章中实现了它，现在它使用 EventStore 组件从数据库读取事件。当订阅者轮询 feed 时，它会将事件发送给事件订阅者。

这个事件存储实现非常基础，并不适用于所有生产用例。例如，当微服务开始从多个并发线程触发事件时，尤其是在高负载下，它可能会遇到锁竞争问题。然而，这个示例确实展示了存储事件的含义。

> **NOTE** 有多个高质量的开源项目在关系型数据库之上实现了事件存储，包括 SqlStreamStore (https://sqlstreamstore.readthedocs.io) 和 Marten (https://marten.db.io/)。

## STORING EVENTS USING THE EVENTSTOREDB SYSTEM

你现在将在购物车微服务中实现 EventStore 组件的另一个版本，这次使用开源的 EventStoreDB。与在 SQL Server 中存储事件相比，使用 EventStoreDB 的优势在于其 API 专为存储事件、读取事件和订阅新事件而设计。EventStoreDB 是一个开源的、成熟的、经过充分测试的事件存储数据库，可以在负载下扩展并稳定运行。此外，它开箱即带了一些附加功能，例如用于检查事件的 Web 界面和 Atom 事件 feed。当然，SQL Server 也是成熟的、经过充分测试的、可扩展且稳定的，但它并非专门针对存储事件而设计。

你将通过以下步骤实现此版本。完成后，你将在购物车微服务中基于 EventStoreDB 拥有一个完全可用的 EventStore 组件实现：

1. 在 Docker 容器中运行 EventStoreDB。
2. 通过 EventStore 组件将事件写入 EventStoreDB。
3. 通过 EventStore 组件从 EventStoreDB 读取事件。

> **NOTE** 你可以在 https://eventstore.com/ 了解更多关于 EventStoreDB 的信息，远不止本章所涵盖的内容。

你可以用以下命令拉取 EventStoreDB Docker 镜像：

```bash
docker pull eventstore/eventstore
```

拉取后，你可以这样运行 EventStoreDB 容器：

```bash
docker run --name eventstore-node -it -p 2113:2113 -p 1113:1113 --rm eventstore/eventstore:latest --run-projections All --enable-external-tcp --enable-atom-pub-over-http
```

> **NOTE** 如果你在 localhost 上运行 EventStoreDB 容器时收到错误 “TLS is enabled on at least one TCP/HTTP interface—a certificate is required to run EventStoreDB”，你可以向 Docker run 命令添加 `--insecure` 开关。这会移除 TLS 提供的<|message|> 安全，不应在生产环境中使用。

你可以通过访问 http://127.0.0.1:2113/ 来检查 EventStoreDB 是否正在运行。你应该会看到一个登录提示，允许你使用用户名 “admin” 和密码 “changeit” 登录。

为了从购物车微服务代码中使用 EventStoreDB，你首先需要将 EventStore.Client NuGet 包添加到项目中。安装后，你就可以针对 EventStoreDB 实现 EventStore 组件。以下列表显示了写入事件的代码。

### Listing 6.6 Storing events to the EventStoreDB

```csharp
namespace ShoppingCart.EventFeed
{
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Text.Json;
    using System.Threading.Tasks;
    using EventStore.ClientAPI;

    public class EsEventStore : IEventStore
    {
        private const string ConnectionString =
            "tcp://admin:changeit@localhost:1113";

        public async Task Raise(string eventName, object content)
        {
            using var connection =
                EventStoreConnection.Create(
                    ConnectionSettings.Create().DisableTls().Build(),
                    new Uri(ConnectionString));
            await connection.ConnectAsync();
            await connection.AppendToStreamAsync(
                "ShoppingCart",
                ExpectedVersion.Any,
                new EventData(
                    Guid.NewGuid(),
                    "ShoppingCartEvent",
                    isJson: true,
                    data: Encoding.UTF8.GetBytes(
                        JsonSerializer.Serialize(content)),
                    metadata: Encoding.UTF8.GetBytes(
                        JsonSerializer.Serialize(new EventMetadata
                        {
                            OccurredAt = DateTimeOffset.UtcNow,
                            EventName = eventName
                        }))));
        }

        public record EventMetadata(
            DateTimeOffset OccurredAt,
            string EventName);
    }
}
```

此代码将购物车微服务自己的 Event 类型映射到 EventStoreDB 的 EventData 类型，然后将其存储到 EventStoreDB。读取事件的实现如下所示。

### Listing 6.7 Reading events from the EventStoreDB

```csharp
public async Task<IEnumerable<Event>>
GetEvents(long firstEventSequenceNumber, long lastEventSequenceNumber)
{
    using var connection =
        EventStoreConnection.Create(
            ConnectionSettings.Create().DisableTls().Build(),
            new Uri(ConnectionString));
    await connection.ConnectAsync();
    var result = await connection.ReadStreamEventsForwardAsync(
        "ShoppingCart",
        start: firstEventSequenceNumber,
        count: (int)(lastEventSequenceNumber - firstEventSequenceNumber),
        resolveLinkTos: false);
    return result.Events
        .Select(e =>
            new
            {
                Content = Encoding.UTF8.GetString(e.Event.Data),
                Metadata = JsonSerializer.Deserialize<EventMetadata>(
                    Encoding.UTF8.GetString(e.Event.Metadata),
                    new JsonSerializerOptions
                    {
                        PropertyNameCaseInsensitive = true
                    })!
            })
        .Select((e, i) =>
            new Event(
                i + firstEventSequenceNumber,
                e.Metadata.OccurredAt,
                e.Metadata.EventName,
                e.Content));
}
```

- 从 EventStoreDB 读取事件
- 访问来自 EventStoreDB 的结果中的事件
- 获取每个事件的内容部分
- 获取每个事件的元数据部分
- 将事件映射为来自 EventStoreDB Event 对象的事件

此代码从 EventStoreDB 读取事件，反序列化事件的内容和元数据部分，然后将它们映射回购物车微服务自己的 Event 类型。

这完成了基于 EventStoreDB 的 EventStore 组件的实现。让我们现在来看使用缓存。

## 6.3.4 Setting cache headers in HTTP responses

让我们再次考虑图 6.2 中的微服务。购物车微服务使用通过查询产品目录微服务获得的产品信息；你在第 2 章中实现了购物车微服务在该协作中的部分。在这里，你将首先在实现端点的代码中设置缓存头



产品目录。然后，你将重写购物车中调用产品目录的代码，以读取并使用缓存头。

假设产品目录微服务中的 `/products` 端点实现在名为 `ProductCatalogController` 的 MVC 控制器中。你将以查询参数的形式接收一个以逗号分隔的产品 ID 列表。该端点返回由该产品 ID 列表标识的每个产品的产品信息。实现方式与本书中你已见过的 MVC 控制器类似。新的部分是，你将使用 `ResponseCache` 属性向响应添加一个缓存控制头，允许客户端缓存响应 24 小时。

```
namespace ProductCatalog
{
    using System.Collections.Generic;
    using System.Linq;
    using Microsoft.AspNetCore.Mvc;

    [Route("/products")]
    public class ProductCatalogController : ControllerBase
    {
        private readonly IProductStore productStore;

        public ProductCatalogController(IProductStore productStore) =>
            this.productStore = productStore;

        [HttpGet("")]
        [ResponseCache(Duration = 86400)]
        public IEnumerable<ProductCatalogProduct> Get(
            [FromQuery] string productIds)
        {
            var products = this.productStore.GetProductsByIds(
                ParseProductIdsFromQueryString(productIds));
            return products;
        }

        private static IEnumerable<int>
            ParseProductIdsFromQueryString(string productIdsString) => ...
    }
}
```

### Listing 6.8  为产品列表添加缓存头

> 添加一个缓存控制头，其中 max-age 以秒为单位

此实现向响应添加了一个缓存控制头，看起来像这样：

```
cache-control: public,max-age:86400
```

该头告诉调用者响应可以被缓存的时间长度由 max-age 指示，max-age 以秒为单位。在本例中，调用者可以缓存响应 86,400 秒（24 小时）。

### 6.3.5  读取和使用缓存头

在第 2 章中，你看到购物车微服务从 `/products` 端点发出调用的代码。代码如下；它是 `ProductCatalogClient` 类的一部分。

### Listing 6.9  调用产品目录微服务

```
private async Task<HttpResponseMessage>
    RequestProductFromProductCatalog(int[] productCatalogIds)
{
    var productsResource = string.Format(getProductPathTemplate,
        string.Join(",", productCatalogIds));
    return await
        this.client.GetAsync(productsResource);
}
```

使用此代码，每当购物车需要产品信息时都会发出一次 HTTP 请求，而不管任何缓存头。这在购物车在 24 小时内多次需要同一产品的相关信息时效率低下，因为这是产品目录微服务响应中设置的 max-age 值。这种情况每次都会发生，当用户向其购物车添加商品，而另一用户在之前的 24 小时内也将其添加到购物车时。这很可能会经常发生。

让我们扩展对产品目录中 `/products` 端点发出调用的代码，使其考虑缓存头。为 `ProductCatalogClient` 添加对实现 `ICache` 接口的缓存的依赖：

```
private readonly HttpClient client;
private readonly ICache cache;
private static string productCatalogueBaseUrl = @"https://git.io/JeHiE";
private static string getProductPathTemplate = "?productIds=[{0}]";

public ProductCatalogClient(HttpClient client, ICache cache)
{
    client.BaseAddress = new Uri(productCatalogueBaseUrl);
    client.DefaultRequestHeaders.Accept.Add(
        new MediaTypeWithQualityHeaderValue("application/json"));
    this.client = client;
    this.cache = cache;
}
```

正如你所回忆的，ASP.NET 会为你处理依赖注入，因此只要在 `IServiceCollection` 中注册了 `ICache` 接口的实现，ASP.NET 就会注入它。在这里，我只展示接口，但在代码下载中，你可以找到一个实现该接口的简单静态缓存。接口很简单，有两个方法：

```
public interface ICache
{
    void Add(string key, object value, TimeSpan ttl);
    object Get(string key);
}
```

ttl 表示生存时间。

你将在 `ProductCatalogClient` 上使用缓存变量，在发出 HTTP 请求之前检查缓存中是否存在有效对象。

style 而不是基于查询的方式。记住第 5 章中基于事件的协作是优选的，因为耦合度降低。

- 微服务使用哪个数据库或数据库是该微服务特有的设计决策。不同的微服务可以使用不同的数据库。
- 存储微服务拥有的数据类似于在其他类型的系统中存储数据。
- 你可以使用 Dapper 从 SQL 数据库读取数据并向其写入数据。
- 存储事件本质上是将序列化事件存储到数据库中。
- 事件存储的简单版本涉及将事件存储到 SQL 数据库中的表中。
- 你也可以通过将事件存储到专门设计用于存储事件的开源 EventStoreDB 来实现 EventStoreDB。
