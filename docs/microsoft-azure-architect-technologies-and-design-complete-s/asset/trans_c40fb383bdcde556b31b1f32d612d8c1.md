

```
static CsharpGuitarHttpClientController()
{
    httpClient = new HttpClient();
}
public async Task<Product> GetTheCoolestGuitarEver(string id)
{
    ...
}
}
```

当你这样做时，同一个连接对象会被用于所有指向同一服务器的出站连接。当你连接到 Azure SQL 数据库、Azure Cosmos DB 或任何需要实例化连接对象的系统时，模式也是相同的。

## 云编码入门

在整本书中，多次提到第 7 章是你将看到如何实际实现某些概念的地方。在本节中，你将看到这些示例，或获得一些关于如何继续进行的提示。将提供以下示例：

- 触发后台作业
- 连接到区域/全局数据库实例
- 使用 Azure 队列存储 SDK
- 使用表单身份验证、证书、Windows 身份验证、MFA、开放标准、托管标识和服务主体身份验证
- 从数据库读取加密数据

### 触发后台作业

后台作业是一段代码，通常需要高 CPU、高内存或长时间运行的活动。实际上，代码片段可以做任何耗时过长而无法实时触发后台作业并等待响应的事情。触发后台作业主要有两种方式：通过事件或通过计划。例如，在 WebJob 的上下文中，事件触发器会调用其公开的 REST API。你可能还记得，WebJob 是在 Azure 应用服务（PaaS）上下文中运行的后台作业处理器。假设你有一个名为 csharpguitar 的 Azure 应用服务，它具有以下终结点：

```
csharpguitar.azurewebsites.net
```

可以使用类似的终结点触发 WebJob。该终结点类似于以下内容：

```
https://csharp guitar.scm.azurewebsites.net/api/triggeredwebjobs/
<webjobName>/run
```

这些可以从浏览器手动调用，也可以从使用 HttpClient 类的其他应用程序调用。另一种选择是使用像 CRON 作业这样的调度器，按特定时间间隔触发批处理作业或 WebJob 的执行。

### 连接到区域/全局数据库实例

在第 5 章中，你了解了 Azure Cosmos DB 的全局复制功能。问题是，如何让代码足够智能，以连接到离客户端位置最近的只读实例？Azure Cosmos DB SDK 中一个名为 ConnectionPolicy 的类公开了一个名为 PreferredLocations 的属性。首选位置在创建用于连接数据库的客户端时使用。SDK 内部的代码会选择最优位置。这非常酷。实现这一点的代码片段如下：

```
ConnectionPolicy connectionPolicy = new ConnectionPolicy();
connectionPolicy.PreferredLocations.Add(LocationNames.SouthCentralUS);
connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope);
DocumentClient docClient =
    new DocumentClient(accountEndPoint, accountKey, connectionPolicy);
```

### 使用 Azure 队列存储 SDK

关于 Azure 队列存储，需要了解的一些重要原则与以下三个方法相关：PeekMessage()、GetMessage() 和 DeleteMessage()。这些方法以及使用它们所需的客户端代码，可以通过 NuGet 包从此处下载：

```
www.nuget.org/packages/Microsoft.Azure.Storage.Queue
```

Visual Studio 的安装说明也可在该 URL 中找到。

PeekMessage() 方法通过以下代码片段调用。请注意，要使此代码片段正常工作，其前后还需要一些代码。例如，你必须实例化存储帐户、实例化存储客户端并标识存储队列。此示例/伪代码仅用于解释以下实现概念。

```
CloudQueueMessage peekedMessage = queue.PeekMessage();
```

以下是一段调用 GetMessage() 方法及其异步替代方法 GetMessageAsync() 的代码：

```
CloudQueueMessage message = queue.GetMessage();
CloudQueueMessage retrievedMessage = await queue.GetMessageAsync();
```

PeekMessage() 和 GetMessage() 方法之间的区别在于，GetMessage() 方法在被调用时会阻塞线程，并等待消息到达队列后才返回。线程将处于等待状态，这可能不是什么大问题，因为如果方法调用必须等待，那就意味着没有消息需要处理。尽管如此，在大多数（如果不是所有）场景中，释放而不是阻塞线程始终是一种良好的做法。

PeekMessage() 的好处在于，当它被调用时，它会告诉你队列中是否有消息正在等待处理，而不会阻塞线程或等待消息到达。因此，除非 PeekMessage() 返回状态表明有消息可供处理，否则没有必要调用 GetMessage()。PeekMessage() 不会对消息设置任何锁，也不会以任何方式修改它。

你也可以考虑调用 GetMessageAsync() 方法，它会释放线程，但会通过 await 等待队列中有消息可供处理。使用哪种流程取决于应用程序的需求。关于使用异步方法，有一点需要注意：所有包含 await 关键字的方法本身都必须是异步的（一直向上）。不建议在异步类中调用 Result() 或 Wait()。这可能会导致严重的阻塞和挂起。

在你的应用程序调用了其中一个获取消息的方法之后，你需要调用 DeleteMessage()，以便队列知道消息已被处理。请注意，此方法也有异步版本，并且它不返回值；但如果删除失败，它会返回一个可捕获的异常。

```
queue.DeleteMessage(retrievedMessage);
await queue.DeleteMessageAsync(retrievedMessage);
```

如果你的代码中出现异常，消息未被处理，那么在某时刻消息将变为未锁定状态，并再次可供处理。队列知道消息已被成功处理的唯一方式，是你在处理完成后从代码中调用 DeleteMessage() 方法。你应该认真考虑将代码放在 try{} ... catch{} 块中，并在 catch 表达式中执行一些代码来尝试重新处理，将消息放入毒信队列，务必记录日志，并决定稍后如何处理它。消息有可能格式错误，会一遍又一遍地失败，所以仅仅重新处理消息并不是最优选择，除非你在 catch 表达式中有良好的逻辑并妥善处理它们。

## 表单、证书、Windows、MFA、开放标准、托管标识和服务主体身份验证

在应用程序中实现身份验证的方法有很多。有些已经详细讨论过，有些则没有。我们将简要介绍几种类型，从表单身份验证开始。

### 表单身份验证

表单身份验证是在 ASP.NET 应用程序中实现的，首先需要安装 IIS 模块。然后，使用类似于以下片段的语法将其配置安装到 web.config 文件中：

```
<configuration>
    <system.web>
        <authentication mode="Forms" />
    </system.web>
</configuration>
```

图 7.3 显示了表单身份验证的 IIS 模块已安装但尚未启用。

# 图 7.3 IIS 身份验证提供程序

![](img/95043b3df926bcdadadb082b49262037_567_0.png)

启用 IIS 模块和 web.config 文件后，身份验证提供程序即被启用。此配置的范围是针对在物理 web.config 文件位置直接相关的目录结构内及其下运行的网站。

关于配置应用位置与 web.config 文件位置关系的说明，原因是此身份验证提供程序可以在 Web 服务器上启用，但除非你在特定网站上专门启用它，否则它不会被使用。除了配置 web.config 之外，你还必须拥有身份验证（AuthN），其中你的自定义用户身份数据库存储凭据。任何定义用户在通过身份验证后是否有权访问应用程序内特定资源的组关联，都将利用授权（AuthZ）来确认访问权限。执行 AuthN 和 AuthZ 过程所需的代码必须由在应用程序本身中工作的开发人员编写。这不是一个“开箱即用”的身份验证解决方案。然而，它确实是一种可靠、复杂且有效的在互联网应用程序中实现安全性的方法。

