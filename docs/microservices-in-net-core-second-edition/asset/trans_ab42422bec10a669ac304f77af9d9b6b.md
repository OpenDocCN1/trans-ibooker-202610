

![](img/608267d7803dc57a1a896ad121f37868_70_0.png)

*图 2.10 购物车微服务事件 feed 将事件发布到电商系统的其他部分。*

实现事件 feed 涉及以下步骤：

- *触发事件。* 购物车领域模型中的代码会在发生重要事件（根据业务规则）时触发事件。重要事件是指向购物车中添加或移除商品。
- *存储事件。* 购物车领域模型触发的事件会存储在微服务的数据存储中。
- *发布事件。* 实现事件 feed 允许其他微服务通过轮询订阅。

我们将依次处理这些步骤。

### 触发事件

要被发布，事件必须先被触发。通常是微服务中的领域代码触发事件，在购物车微服务中也是如此。当向购物车添加商品时，ShoppingCart 领域对象通过在 IEventStore 上调用 Raise 方法并提供事件数据来触发事件。

记住我喜欢将相关内容放在文件夹中，所以我添加了一个名为 EventFeed 的新文件夹，并在其中添加了两个文件：Event.cs 和 EventStore.cs。第一个文件包含事件的模型类型，如以下列表所示。

**列表 2.14 简单事件类型**

```
namespace ShoppingCart.EventFeed
{
    using System;

    public record Event(
        long SequenceNumber,
        DateTimeOffset OccurredAt,
        string Name,
        object Content);
}
```

```
seqNumber,
DateTimeOffset.UtcNow,
eventName,
content));
}
```

EventStore 存储所有传入的事件并跟踪它们的到达顺序。我们将在第 6 章重新讨论事件存储的主题，在那里我们将更深入地研究其实现。

### 一个简单的事件 feed

一旦事件被存储，它们就准备好被发布——从某种意义上说，它们*已经*被发布了。即使一个微服务*订阅*另一个微服务的事件，事件 feed 也是通过让订阅者定期请求新事件（例如每 30 秒一次）来工作的。由于订阅者负责请求新事件，你在购物车微服务中需要做的只是添加一个 HTTP 端点，允许订阅者请求事件。订阅者例如可以发出以下请求来获取编号 100 之后的所有事件：

```
GET /events?start=100 HTTP/1.1
Host: shoppingcart.my.company.com
Accept: application/json
```

或者，如果订阅者想要限制每次调用的传入事件数量，它可以在请求中添加 end 参数：

```
GET /events?start=100&end=200 HTTP/1.1
Host: shoppingcart.my.company.com
Accept: application/json
```

将此 /events 端点的实现，如以下列表所示，放在 EventFeed 文件夹中的一个新控制器中。该端点接受可选的起始点和可选的结束点，允许其他微服务请求事件范围。

**列表 2.17 向其他微服务公开事件**

```
namespace ShoppingCart.EventFeed
{
    using System.Linq;
    using Microsoft.AspNetCore.Mvc;

    [Route("/events")]
    public class EventFeedController : Controller
    {
        private readonly IEventStore eventStore;

        public EventFeedController(IEventStore eventStore) =>
            this.eventStore = eventStore;

        [HttpGet("")]
        public Event[] Get(
            [FromQuery] long start,
            [FromQuery] long end = long.MaxValue)
        =>
            this.eventStore
                .GetEvents(start, end)
                .ToArray();
    }
}
```

从查询字符串参数读取 start 和 end 值

返回原始事件列表。MVC 会负责将事件序列化为响应体。

EventFeedController 大多使用你已经遇到过的 MVC 功能。唯一的新内容是 start 和 end 值是从查询字符串参数中读取的。方法参数前面的 FromQuery 参数告诉 MVC 这些是查询字符串参数。

EventFeedController 使用事件存储根据客户端的 start 和 end 值过滤事件。虽然过滤最好在数据库级别完成，但以下简单实现很好地说明了这一点。

**列表 2.18 根据起始点和结束点过滤事件**

```
public IEnumerable<Event> GetEvents(
    long firstEventSequenceNumber,
    long lastEventSequenceNumber) =>
        database
            .Where(e =>
                e.SequenceNumber >= firstEventSequenceNumber &&
                e.SequenceNumber <= lastEventSequenceNumber)
            .OrderBy(e => e.SequenceNumber);
```

有了 /events 端点，想要订阅购物车微服务事件的微服务可以通过轮询端点来实现。订阅者可以——也应该——使用 start 和 end 查询字符串参数，确保只获取新事件。如果在订阅者轮询时购物车宕机，订阅者稍后可以再次请求相同的事件。同样，如果订阅者宕机了一段时间，它可以通过从上次看到的事件开始请求事件来追赶购物车的事件。如前所述，这不是一个完整的事件 feed 实现，但它使微服务能够订阅事件，并且代码很简单。

你现在已经完成了第一个微服务的版本 1 实现。如你所见，微服务很小且专注度窄：它只提供一种业务能力。你也可以看到微服务代码往往简单易懂。这就是为什么你可以期望快速创建新的微服务并替换现有微服务。

### 2.3 运行代码

现在购物车微服务的所有代码都已就位，你可以像在第 1 章运行示例那样运行它：从 Visual Studio 内部，或从命令行使用 dotnet。你可以用 RestClient 或类似工具测试所有端点。

当你第一次尝试通过 HTTP GET 到 /shoppingcart/123 获取购物车时，购物车将是空的。尝试通过 HTTP POST 到 /shoppingcart/123/items 添加一些商品，然后再次获取它；响应应包含已添加的商品。你也可以查看 /events 处的事件 feed，你应该能看到每个添加商品的事件。

**警告** 我没有展示 EventStore 或 ShoppingCartStore 的实现。如果你没有创建这些的实现，你的微服务将无法工作。

### 总结

- 实现一个完整的微服务不需要太多代码。购物车微服务只有以下内容：
  - 两个简短的控制器
  - 一个简单的 ShoppingCart 领域类
  - 一个用于调用产品目录微服务的客户端类
  - 两个简单的数据访问类：ShoppingCartDataStore 和 EventStore（本章未展示）
- MVC 使实现 HTTP API 变得简单。MVC 提供的路由属性使向微服务添加端点变得容易。只需添加一个 action 方法并在其上放置一个像 [HttpGet("")] 或 [HttpPost("")] 这样的路由属性即可。
- 你应该始终期望其他微服务可能宕机。为了防止错误传播，每个远程调用都应包装在处理失败的策略中。
- Polly 库对于实现失败处理策略并将其包装在远程调用周围非常有用。
- 实现一个基本的事件 feed 很简单，并且使其他微服务能够对事件做出反应。本章实现的简陋事件 feed 只是一个简短的控制器。
- 领域模型代码通常负责触发事件，这些事件随后存储在事件存储中并通过事件 feed 发布。

## 将微服务部署到 Kubernetes

### 本章涵盖

- 将微服务打包到 Docker 容器中
- 将微服务容器部署到本地主机上的 Kubernetes
- 在 Azure 的 AKS（Azure Kubernetes Service）上创建基本的 Kubernetes 集群
- 将微服务容器部署到 AKS 上的 Kubernetes 集群

在第 2 章中，我们开发了一个简单的购物车微服务，但我们只在本地主机上直接使用 dotnet run 命令运行它。在本章中，我们将该微服务部署到公共云中的类似生产环境。我们的微服务可以在许多不同的环境中运行，但我们将只选择其中一个作为示例。我们将重点介绍如何将购物车微服务运行在 Microsoft Azure 的 Kubernetes 集群中。为了实现这一目标，本章我们将：

- 将购物车放入 Docker 容器中
- 在本地主机上设置 Kubernetes，以便我们拥有一个测试环境

