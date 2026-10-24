

microservice，基于命令的协作已实现。API 网关可以注册和更新用户，但目前还无法查询用户。

### 5.2.4 使用 HTTP GET 实现查询

忠诚度计划微服务可以处理它需要处理的命令，但它无法回答有关已注册用户的查询。请记住，忠诚度计划只需要一个端点来处理查询。如前所述，处理查询的端点是一个 HTTP GET 端点，其 URL 形式为“/users/{userId}”，并以用户表示形式响应。该端点实现了图 5.3 中的两个查询。

**Listing 5.8 GET endpoint to query a user and their loyalty point by user ID**

```csharp
namespace LoyaltyProgram.Users
{
    using System;
    using System.Collections.Generic;
    using Microsoft.AspNetCore.Mvc;

    [Route("/users")]
    public class UsersController : ControllerBase
    {
        private static readonly Dictionary<int, LoyaltyProgramUser>
            RegisteredUsers = new();

        [HttpGet("{userId:int}")]
        public ActionResult<LoyaltyProgramUser> GetUser(int userId) =>
            RegisteredUsers.ContainsKey(userId)
                ? (ActionResult<LoyaltyProgramUser>) Ok(RegisteredUsers[userId])
                : NotFound();

        ...
    }
}
```

这段代码中没有任何你以前没有见过的内容。同样，API 网关微服务中查询此端点所需的代码也不应该令人惊讶：

```csharp
public class LoyaltyProgramClient
{
    ...     
    public async Task<HttpResponseMessage> QueryUser(string arg) =>
        await this.httpClient.GetAsync($"/users/{int.Parse(arg)}");
}
```

这就是基于查询的协作所需的一切。你现在已经实现了忠诚度计划微服务的命令和查询协作。

### 5.2.5 实现基于事件的协作

现在你已经知道如何实现微服务之间的命令和查询协作，是时候将注意力转向基于事件的协作了。图 5.11 重复了忠诚度计划微服务所涉及的协作。忠诚度计划订阅来自特价优惠的事件，并使用这些事件来决定何时通知已注册用户有关新的特价优惠。

![](img/608267d7803dc57a1a896ad121f37868_138_0.png)

Figure 5.11 The event-based collaboration in the loyalty program microservice is the subscription to the event feed in the special offers microservice.

我们将首先查看特价优惠如何以 feed 的形式公开其事件。然后，我们将返回忠诚度计划并为该服务添加第二个进程，该进程将负责订阅和处理事件。

### Implementing an event feed

You saw a simple event feed in chapter 2. The special offers microservice implements its event feed the same way: it exposes an endpoint—"/events"—that returns a list of sequentially numbered events. The endpoint can take two query parameters—start and end—that specify a range of events. For example, a request to the event feed can look like this:

```http
GET /events?start=10&end=110 HTTP/1.1

Host: localhost:5002
Accept: application/json
```

The response to this request might be the following, except that I’ve cut off the response after five events:

```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

[{
  "sequenceNumber": 1,
  "occurredAt": "2020-06-16T20:13:53.6678934+00:00",
  "name": "SpecialOfferCreated",
  "content": {
    "description": "Best deal ever!!!",
    "id": 0
  }
},
{
  "sequenceNumber": 2,
  "occurredAt": "2020-06-16T20:14:22.6229836+00:00",
  "name": "SpecialOfferCreated",
  "content": {
    "description": "Special offer - just for you",
    "id": 1
  }
},
{
  "sequenceNumber": 3,
  "occurredAt": "2020-06-16T20:14:39.841415+00:00",
  "name": "SpecialOfferCreated",
  "content": {
    "description": "Nice deal",
    "id": 2
  }
},
{
  "sequenceNumber": 4,
  "occurredAt": "2020-06-16T20:14:47.3420926+00:00",
  "name": "SpecialOfferUpdated",
  "content": {
    "oldOffer": {
      "description": "Nice deal",
      "id": 2
    },
    "newOffer": {
      "description": "Best deal ever - JUST GOT BETTER",
      "id": 0
    }
  }
},
{
  "sequenceNumber": 5,
  "occurredAt": "2020-06-16T20:14:51.8986625+00:00",
  "name": "SpecialOfferRemoved",
  "content": {
    "offer": {
      "description": "Special offer - just for you",
      "id": 1
    }
  }
}]
```

Content 属性用于事件特定数据，也是 SpecialOfferCreated 事件、SpecialOfferUpdated 事件和 SpecialOfferRemoved 事件之间差异出现的地方。每个事件在 Content 中都有其自己的对象类型。

这就是公开事件 feed 所需的一切。使用基于 HTTP 的事件 feed 发布事件的巨大优势在于其简单性。基于事件的协作可以通过队列系统实现，但这会引入另一个复杂的生产技术，你必须学习使用和维护。某些情况下这种复杂性是合理的，但绝非总是如此。

### Creating an event-subscriber process

订阅事件 feed 本质上意味着你将轮询你订阅的微服务的 events 端点。按间隔，你将向“/events”端点发送 HTTP GET 请求，以检查是否有尚未处理的事件。

我们将此周期性轮询实现为两个主要部分：

- 一个简单的控制台应用程序，用于读取一批事件
- 一个 Kubernetes CronJob，用于按间隔运行控制台应用程序

将这两者结合起来实现了事件订阅：CronJob 确保控制台应用程序按间隔运行，而每次控制台应用程序运行时，它都会发送 HTTP GET 请求以检查是否有事件需要处理。

实现事件订阅进程的第一步是使用以下 dotnet 命令创建一个控制台应用程序：

```powershell
PS> dotnet new console -n EventConsumer
```

并用 dotnet 运行它：

```powershell
PS> dotnet run
```

该应用程序为空，因此目前不会发生任何有趣的事情，但在下一节中我们将使其读取事件。

### Subscribing to an event feed

你现在有一个 EventConsumer 控制台应用程序。它所要做的就是读取一批事件并跟踪下一批事件的起点。这按如下方式完成。

**Listing 5.11 Reading a batch of events from an event feed**

```csharp
using System;
using System.IO;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text.Json;
using System.Threading.Tasks;

// Read the starting point of this batch from a database.
var start = await GetStartIdFromDatastore();
var end = 100;
var client = new HttpClient();

client.DefaultRequestHeaders
    .Accept
    .Add(new MediaTypeWithQualityHeaderValue("application/json"));
using var resp = await client.GetAsync(
    new Uri($"http://special-offers:5002/events?start={start}&end={end}"));
await ProcessEvents(await resp.Content.ReadAsStreamAsync());
await SaveStartIdToDataStore(start);

Task<long> GetStartIdFromDatastore(){...}
async Task ProcessEvents(Stream content){...}
Task SaveStartIdToDataStore(long startId){...}
```

通过上面的代码，EventConsumer 可以读取一批事件，并且每次被调用时，它都会读取下一批事件。剩余的部分是处理事件，如下所示。

**Listing 5.12 Deserializing and then handling events**

```csharp
async Task ProcessEvents(Stream content)
{
    var events =
        await JsonSerializer.DeserializeAsync<SpecialOfferEvent[]>(content)
        ?? new SpecialOfferEvent[];
    foreach (var @event in events)
    {
        Console.WriteLine(@event);
        start = Math.Max(start, @event.SequenceNumber + 1);
    }
}
```

这里有几点需要注意：

- 此方法跟踪已处理的事件。这确保你不会从 feed 请求已处理的事件。
- 如你之前所见，并非所有事件在 Content 属性中携带相同的数据，因此根据 Name 属性，事件处理可以期望 Content 对象中存在不同的数据。你希望在接受传入的 'Content' 属性中的数据时保持宽松——如果特价优惠微服务决定向事件 JSON 添加额外字段，不应造成问题。只要你需要的数据在那里，其余部分可以被忽略。
- 事件被反序列化为 SpecialOfferEvent 类型。这与用于在特价优惠中序列化事件的 EventFeedEvent 类型不同。这是故意的，因为两个微服务不需要对事件有完全相同的视图。只要忠诚度计划不依赖于不存在的数据，一切都很好。

此处使用的 SpecialOfferEvent 类型很简单，仅包含忠诚度计划中使用的字段：

