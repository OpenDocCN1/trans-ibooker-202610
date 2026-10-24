# 7
流行架构与
全局总结

本章是前一章的延续，因为流行的架构通常基于云。在本章中，我们将重点讨论每个软件架构师都应该掌握的主要架构趋势。

更具体地说，我们将涵盖以下主题：

- API驱动的架构
- 微服务架构示例实践
- 无服务器架构示例实践

因为我已经介绍了无服务器和微服务的理论方面，我想满足你作为开发者的需求，带你走过更具体的例子，尽管可能不那么技术中立。到本章结束时，你应该已经掌握API驱动架构的基础知识，并开始使用无服务器和微服务。

## 技术要求

如果你想实践本章中提供的解释，你将需要以下内容：

- **Visual Studio 2019**：用于打开GitHub上提供的解决方案。
- **Kubernetes**：你需要一个普通的集群，如**MiniKube**或**Docker Desktop**（内置K8s）。你也可以使用任何云提供的集群（Azure、AWS或GCP）。我使用Azure Kubernetes服务来托管我的演示解决方案。无论你选择什么解决方案，请确保集群可以访问互联网，以便它能拉取我发布到**Docker Hub**的Docker镜像。
- **Azure订阅**：我将Azure用于无服务器示例应用程序。要创建你自己的免费Azure账户，请按照https://azure.microsoft.com/free/ 上的步骤操作。

本章的所有代码示例和图表可在 https://github.com/PacktPublishing/Software-Architecture-for-Humans/tree/master/CHAPTER%207/ 获取。

## API驱动的架构

现代资产附带API，但API本身的概念随着时间的推移而改变。在九十年代，API是某种客户端库，你可以用它来与应用程序交互。在2021年，API是一个作为客户端接口的物理端点，允许客户端与后端服务交互。形式改变了，但目的相同。两种形式都旨在促进集成场景，并将应用程序功能暴露给客户端程序。然而，我注意到，虽然开发人员通常知道如何开发后端服务，但他们往往缺乏API管理解决方案的技能，这些解决方案可用于内部和面向互联网的API。API管理解决方案开箱即用地容纳了一些设计模式：

- **面向后端的后端（BFF）**：BFF的目的是提出一个针对特定消费渠道（如移动应用程序）量身定制的API。目的是满足特定客户端的具体要求。移动应用通常在带宽方面（比以前少）受限，屏幕也较小，因此你可能不想返回与常规网站相同数量的信息。同样，根据客户端消费者，你可能想过滤掉一些API操作。API管理解决方案还提供响应转换机制。
- **网关聚合**：这种模式与*外观模式*非常相似，但不是通过代码实现，而是依靠API网关策略来完成繁重的工作。请注意，这种模式也可以很容易地通过基于代码的技术（如**GraphQL** (https://graphql.org/)）来解决。
- **网关卸载**：此模式的目的是将横切关注点（如相互认证、JWT令牌验证、TLS终止、限流、数据缓存等）卸载到API网关。这防止你为这些活动编写代码，并确保到达后端服务的请求首先经过一系列验证。每个非法请求都会被网关丢弃。这不仅使你的开发人员生活更轻松，也使你的应用程序更健壮、更安全。基于云的API网关内置了**拒绝服务（DOS）/分布式拒绝服务（DDOS）**缓解功能。
- **网关路由**：API网关可以根据输入请求和上下文变量，将入站请求动态路由到不同的后端服务。大多数基于云的网关可以将请求路由到最近区域（最靠近客户端应用）和/或最快的后端服务。

API管理解决方案帮助你处理前面列出的模式，但还有更多。它们有效地允许你管理API的生命周期，并将其暴露给内部和外部客户。有许多现成的解决方案，如Azure API管理、AWS API网关、MuleSoft Anypoint API管理器等。作为软件架构师，你不能错过这些类型的解决方案。微服务架构，正如我们将在下一节中看到的，是典型的API驱动架构。

## 微服务架构示例实践

我在*第4章，回顾历史架构风格*中高水平地介绍了微服务。进行全面解释超出了本书的范围。然而，由于微服务已成为一种重要趋势，我想通过一个具体例子更深入地探讨。目标是关注不同服务之间的通信方面，包括同步和异步。

- 订单**运输**服务拾取每个传入的订单事件以开始运输流程。首先，它对订单查询服务执行*同步*查询以检索有关订单的额外详细信息，并相应地开始运输流程。
- 整个通信管道（同步和异步）由**分布式应用程序运行时（Dapr）**确保，这是一个技术无关的运行时。

所有三个服务都托管在K8s集群中。你可以在本书的GitHub存储库中找到完整示例，以及运行此示例应用程序所需的步骤（在.README文件中）。为方便起见，我已将服务容器镜像发布到公共Docker Hub，这样你就不必担心拥有自己的容器注册表。

现在，让我们关注此示例应用程序的基本部分。

## 服务发现与通信

如前所述，Dapr (https://dapr.io/) 用于确保同步和异步服务通信。我没有水晶球，但我很确定Dapr很快就会成为微服务世界中的一等公民。简而言之，它允许你处理非常常见的横切关注点，如发布/订阅、状态管理、绑定等。

Dapr是技术和云无关的。你可以无缝地绑定数十种云服务（AWS、GCP、Azure、阿里云等），而无需自己重新发明轮子。Dapr说明了生态系统日益增长的重要性，作为软件架构师，你必须评估此类框架如何用作架构构建块。一旦你下载了Dapr CLI工具 (https://docs.dapr.io/getting-started/install-dapr-cli/)，安装Dapr只需一行命令：

```
dapr init --runtime-version 1.0.1 -k -
```

-k选项将针对你当前正在使用的Kubernetes集群。





## 探索代码的核心部分

现在，让我们探索代码的核心部分，以确保你理解整个流程。订单处理服务如下图所示：

```
[ApiController]
public class OrderController : ControllerBase{
    private readonly DaprClient _dapr;
    private readonly ILogger<OrderController> _logger;

    public OrderController(ILogger<OrderController> logger, DaprClient dapr){
        _logger = logger;
        _dapr = dapr;
    }

    [HttpPost]
    [Route("order")]
    public async Task<IActionResult> Order([FromBody] Order order, [FromServices] DaprClient daprClient){
        //we pretend to create an order
        _logger.LogInformation($"Order with id {order.Id} created!");
        await _dapr.PublishEventAsync<Order>("bus", "order", order);
        return Ok();
    }

    async Task<IActionResult> PublishOrderEvent(Guid OrderId, OrderEvent.EventType type){
        //we publish the order created event
        var ev = new OrderEvent
        {
            id = OrderId,
            name = "OrderEvent",
            type = type
        };
        await _dapr.PublishEventAsync<OrderEvent>("bus", "order", ev);
        return Ok();
    }
}
```

图 7.2 – 订单处理后端服务

首先，我们通过 DI 获取一个 DaprClient 实例。我们通过 HTTP post 暴露订单端点。我们假装创建了一个订单，然后通过 Dapr 发布订单已创建事件，Dapr 通过稍后我会展示的组件文件链接到我们的 RabbitMQ broker。

运送服务的实现如下所示：

```
[Topic("bus", "order")]
[HttpPost]
[Route("dapr")]
public async Task<ActionResult> ProcessOrderEvent([FromBody] OrderEvent ev){
    _logger.LogInformation($"Received new event");
    _logger.LogInformation("{0} {1} {2}", ev.id, ev.name, ev.type);
    switch (ev.type){
        case OrderEvent.EventType.Created:
            var order = await GetOrder(ev.id);
            if (order!=null){
                _logger.LogInformation($"Starting shipping process for order {ev.id} with " +
                    $"{order.Products.Count} \"products!\"");
            }
            else{
                _logger.LogInformation($"order {ev.id} could not be retrieved, suspending shipping process!");
            }
            break;
        other cases
    }
    return Accepted();
}

async Task<Order> GetOrder(Guid id){
    try{
        return await dapr.InvokeMethodAsync<object, Order>(
            HttpMethod.Get,
            "orderquery",
            id.ToString(),
            null);
    }
    catch (Exception ex){//should be more specific
        _logger.LogError(ex.Message);
        return null;
    }
}
```

ProcessOrderEvent 方法用了一些属性装饰。最重要的一行是第一行 Topic("bus","order")，它使运送服务订阅订单主题。然后，GetOrder 方法用于对订单查询服务进行同步调用以检索有关订单的额外详细信息。此方法依赖于 Dapr 内置的服务发现来查找查询服务。作为服务客户端，你唯一需要知道的就是应用程序标识符，在本例中为 orderquery。当然，前述截图中显示的代码并不适合生产环境——因为你需要处理不幸的情况——但它足以演示通信管道，而这在微服务架构中非常重要。

![](img/78092c233296cbd7723546ae74eec12d_149_0.png)

## 部署应用程序

开始之前，你需要准备好 K8s 集群，必须预先部署 RabbitMQ，并获取其默认凭据（按照 GitHub 上的步骤操作）。完成后，你可以使用此处所示的 YAML 部署文件（已截断）部署应用程序：

```
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: bus
  namespace: microserviceapp
spec:
  type: pubsub.rabbitmq
  version: v1
  metadata:
  - name: host
    value: "amqp://user:pwd@rabbitmq.default.svc.cluster.local:5672"
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: orderprocessing
  namespace: microserviceapp
  labels:
    app: orderprocessing
spec:
  replicas: 1
  selector:
    matchLabels:
      app: orderprocessing
  template:
    metadata:
      labels:
        app: orderprocessing
      annotations:
        dapr.io/enabled: "true"
        dapr.io/app-id: "orderprocessing"
        dapr.io/app-port: "80"
    spec:
      containers:
      - name: orderprocessing
        image: stephaney/orderprocessing:dev
        imagePullPolicy: Always
```

图 7.4 – YAML 部署文件

有趣的部分是第一个块，它部署了 Dapr 组件，该组件通过其类型指向 RabbitMQ。host 属性对应于 RabbitMQ 在集群中监听的端点。第二个块是订单处理服务的部署（另外两个服务以相同方式部署）。在 annotations 部分，我们告诉系统启用 Dapr，并为我们的服务提供一个应用程序标识符。

## 测试应用程序

部署应用后，按照 GitHub 上解释的前提条件，你应该能够对其进行测试。由于我希望尽可能保持简单，我没有创建 ingress 规则，这意味着部署的服务无法从 K8s 外部访问。按照以下步骤测试应用：

- 1. 验证所有 Pod 是否正在运行：

```
Windows PowerShell
PS C:\> kubectl get pods -n microserviceapp
NAME                          READY   STATUS    RESTARTS   AGE
orderprocessing-ff6f5644-5sjzb  2/2     Running   0          87s
orderquery-5557767d84-mcwcc    2/2     Running   0          87s
shippingprocessing-7bcfb96bfb-q6kbq 2/2 Running 0 87s
PS C:\>
```

图 7.5 – 列出 Pod

- 2. 你应该会看到每个 Pod 运行两个容器——服务和 Dapr sidecar。为了下订单，我们需要将订单处理（或任何其他注入 Dapr 的 Pod）流量转发到主机：

```
Windows PowerShell
PS C:\> kubectl -n microserviceapp port-forward orderprocessing-ff6f5644-5sjzb 3500:3500
Forwarding from 127.0.0.1:3500 -> 3500
Forwarding from [::1]:3500 -> 3500
```

图 7.6 – 将流量转发到主机

- 3. 请确保将 Pod 名称替换为你自己的。完成后，我们就可以开始向本地主机端点发出调用了。

我们将重用同一个 K8s 集群来托管事件发布者，该发布者将事件发布到自定义事件网格主题。Azure 函数，由前述图表中的事件处理程序表示，订阅了我们的主题，并在事件网格推送新事件时被触发。正如我之前解释的那样，无服务器架构特别适合事件驱动和异步场景。无服务器的美妙之处在于你可以立即部署代码，并且不必特别担心可扩展性、高可用性等等。既然我们谈论的是代码，让我们看看事件发布者是什么样的。

## 事件发布者代码

事件发布者是一个简单的控制台程序，它向我们将在下一节中部署的事件网格主题发送事件。如下图所示的程序，将主题端点和访问密钥（用于身份验证）作为输入，然后每 100 毫秒简单地发送事件：

```
class Program{
    static async Task Main(string[] args){
        EventGridPublisherClient client = new EventGridPublisherClient(
            new Uri(Environment.GetEnvironmentVariable("EvGridEndpoint")),
            new AzureKeyCredential(Environment.GetEnvironmentVariable("EvGridAccessKey")));
        while(true){
            await client.SendEventAsync(new EventGridEvent(
                "Serverless",
                "Serverless.OrderEvent",
                "1.0",
                Guid.NewGuid().ToString()
            ));
            Thread.Sleep(100);
        }
    }
}
```

图 7.11 – 事件发布者代码

我本可以使用 Dapr 将消息推送到事件网格，但 Dapr 组件的配置并不那么简单，所以我尽可能让它对你来说简单。我已将这个小型控制台应用发布到 Docker Hub。





6. 确保你用一个唯一的值替换 YOURVALUE。Azure 将使用它来连接函数。在我的例子中，我使用了 packtsrvlessarch。请确保不要使用任何花哨的字符。在此阶段，你应该在资源组中找到以下资源：

| Name | Type | Location |
|---|---|---|
| packtofsvc2yqt2cro | Storage account | West Europe |
| packtsrvlessarch | Function App | West Europe |
| packtsrvlessarchofsvc2yqt2cro | Application Insights | West Europe |
| packtsrvlessarchofsvc2yqt2cro | App Service plan | West Europe |

Figure 7.13 – Checking the deployed resources

7. Azure functions 需要一个存储账户和一个应用服务计划（绑定到用于无服务器函数的动态定价层）才能运行。还有一个 Application Insights 实例用于监控函数。

8. 现在，你可以部署位于 .zip 文件中的函数代码：

```
az webapp deployment source config-zip --resource-group packtserverless --name YOURVALUE --src event-consumer.zip
```

9. 现在是部署的最后一部分，即创建事件网格的主题和订阅。这最后一步将之前部署的函数订阅到主题：

```
az deployment group create --template-file serverless2.json --resource-group packtserverless --parameters eventGridTopicName='YOURVALUETp' eventGridSubscriptionName='YOURVALUETpsub' functionAppName='YOURVALUE'
```

10. 请注意，YOURVALUE 仍然代表你的函数名称。在此阶段，你应该拥有所有已部署并相互链接的资源：

| Name | Type | Location |
|---|---|---|
| packtofsvc2yqt2cro | Storage account | West Europe | ... |
| packtsrvlessarch | Function App | West Europe | ... |
| packtsrvlessarchofsvc2yqt2cro | Application Insights | West Europe | ... |
| packtsrvlessarchofsvc2yqt2cro | App Service plan | West Europe | ... |
| packtsrvlessarchtp | Event Grid Topic | West Europe | ... |

Figure 7.14 – Checking if all the resources were deployed

11. 最后一项是事件网格主题。点击它后，你应该会看到确实为 Azure 函数创建了一个订阅：

| Name | Endpoint |
|---|---|
| packtsrvlessarchtpsub | AzureFunction |

Figure 7.15 – The Azure function subscription

12. 名称将为 YOURVALUEtpsub。在查看事件网格主题时，请记下主题的端点和访问密钥。主题端点可以从概览页面获取，访问密钥（两个密钥）可通过左侧菜单访问。只复制其中一个密钥。我们需要端点和其中一个密钥用于我们的事件发布者。

Azure 基础设施已完全部署完毕。然而，我们仍需在 K8s 集群上部署事件发布者，然后才能测试应用程序。所以，我们几乎完成了。让我们完成最后的步骤：

- 1. 编辑 serverless.yml（可在 GitHub 上获取）并用上一步获取的自己的值替换 YOURENDPOINT 和 YOURKEY：

```
spec:
  containers:
  - name: eventpublisher
    image: stephaneey/eventpublisher:dev
    env:
    - name: EvGridEndpoint
      value: "YOURENDPOINT"
    - name: EvGridAccessKey
      value: "YOURKEY"
    imagePullPolicy: Always
```

Figure 7.16 – YAML spec of the event publisher container

- 2. 将 YML 文件部署到你的集群：

```
kubectl apply -f ./serverless.yml
```

恭喜你，完成啦！

## Testing the application

现在一切都已部署完毕，你应该至少有一个事件发布者 Pod 实例正在运行。你可以按如下方式验证：

```
Windows PowerShell
PS C:\> kubectl get pod -n microserviceapp
NAME                          READY   STATUS    RESTARTS   AGE
eventpublisher-68959df74-96w8x 1/1     Running   0          14s
PS C:\> _
```

Figure 7.17 – Checking that the event publisher pod is running

这应该已经向我们的网格发布事件，并且通知应推送到我们的函数。在扩展到更多实例之前，导航到你的 Application Insights 资源并点击左侧的实时指标菜单。一旦指标开始显示，按以下屏幕截图所示扩展事件发布者：

```
Windows PowerShell
PS C:\> kubectl scale deploy/eventpublisher --replicas=50 -n microserviceapp
deployment.apps/eventpublisher scaled
PS C:\> kubectl get pod -n microserviceapp
NAME                          READY   STATUS             RESTARTS   AGE
eventpublisher-68959df74-25kfs 0/1     ContainerCreating  0          6s
eventpublisher-68959df74-294vj 0/1     Pending            0          6s
eventpublisher-68959df74-4v4ld 0/1     Pending            0          6s
eventpublisher-68959df74-5ph4z 0/1     Pending            0          6s
eventpublisher-68959df74-5szv6 0/1     ContainerCreating  0          6s
eventpublisher-68959df74-6qwd7 0/1     ContainerCreating  0          6s
eventpublisher-68959df74-72psp 0/1     Pending            0          6s
eventpublisher-68959df74-7hmvs 0/1     Pending            0          6s
eventpublisher-68959df74-7wjxl 0/1     Pending            0          6s
```

Figure 7.18 – Scaling out the event publisher

我将事件发布者扩展到 50 个实例。这在你的环境中可能有效也可能无效，取决于你的集群容量。如果 50 太多，只需将其减少到 5。目标是创建更多事件并观察 Azure Functions 如何相应地扩展。实时指标屏幕应显示如下内容：

![](img/78092c233296cbd7723546ae74eec12d_159_0.png)

Figure 7.19 – Live metrics showing the request rate and duration

无论你如何扩展事件发布者，都让它运行大约一分钟，然后将部署缩减回零。

如前面的屏幕截图所示，在高峰时段，系统每秒处理约 525 个请求，且大多数执行时间不到 20 毫秒。我们无需配置任何内容；我们只需让云提供商根据需求调整计算能力。以下屏幕截图显示系统已扩展，最多达到五个实例：

![](img/78092c233296cbd7723546ae74eec12d_159_1.png)

Figure 7.20 – Azure function max instance count

停止事件发布者后运行相同的查询将返回无结果，意味着实例不再运行。你无需自己运行查询。因此，这个非常小的无服务器应用程序确实基于一个根据需求自动伸缩的系统。锦上添花的是，在 Azure 中，你每月可免费获得 100 万次执行。你可以强制 Azure Functions 进一步扩展，如果你拥有强大的集群或可以同时从不同系统运行事件发布者。在我的例子中，我只使用了一个单节点实验室集群，这本身就是一个限制因素，难以产生高负载。

我希望你喜欢这次进入无服务器架构神奇世界的短暂旅程。现在，让我们回顾本章。

## Summary

在本章中，我尝试为你提供更具体的流行架构风格示例，因为我在上一章中已经介绍了理论部分。本章提供的两个示例的目的是展示你能多快开始使用云和云原生应用程序。两个示例都依赖于基础设施即代码。剩余的手动步骤是为了保持简单，但请放心，在现实世界中，这可以完全自动化。

两个示例都表明，生态系统在构建新解决方案时发挥着重要作用。对于无服务器应用程序，我们依赖 Azure Functions 和 Azure Event Grid，并利用 K8s 的内置扩展能力来负载测试我们的函数处理程序。在微服务示例中，我们使用了 Dapr，又一个来自 K8s 生态系统的优秀框架。两个演示都旨在强调生态系统的重要性，作为软件架构师，你绝不应忽视它。我们从零开始构建一切的时代已经一去不复返。学习并与不断增长的生态系统保持联系完全是软件架构师的职责。





## 后记

我希望你喜欢你的软件架构之旅。正如你在第一章中所了解到的那样，并没有单一的软件架构愿景。我认为一个优秀的软件架构师必须在应用开发和架构方面专业化，同时也要理解更大的整体。优秀的软件架构师必须能够与每一种类型的利益相关者进行互动。这就是为什么我带你去发现一些流行的框架，例如 TOGAF、ITIL 和 NIST，以及 ATAM 方法论。这些技能（即使是部分）对于作为架构师成长来说是必不可少的。框架帮助你结构化和组织你的工作。你应用它们的程度取决于你工作的组织。

我无法绕过设计模式，因为它们是软件架构师工作不可分割的一部分，但关于它们有很多书籍，所以我不想再写一本关于设计模式的书。我们最后两章展示了云及其相关生态系统在设计应用程序方面是游戏规则的改变者。这一事实经常被许多开发人员和架构师忽视，但如果你不想错过这趟顺风车，就不要陷入同样的陷阱。很明显，云和我所说的容器化的全球化已经到来，所以你最好关注它们。更重要的是，我希望你能领会整本书中我传达的信息：始终对事物做出良好的判断。无论你是在使用框架还是设计模式，都不要盲目地照字面应用。确保你不要采用一刀切的方法，这在实践中永远行不通，或者充其量会产生很多挫败感。要务实，不要教条！

![](img/78092c233296cbd7723546ae74eec12d_163_0.png)

Packt.com

订阅我们的在线数字图书馆，全面访问超过 7,000 本书籍和视频，以及帮助你规划个人发展和推进职业生涯的行业领先工具。更多信息，请访问我们的网站。

## 为什么订阅？

- 花更少的时间学习，花更多的时间编码，使用来自 4,000 多名行业专业人士的实用电子书和视频
- 使用专为你打造的技能计划提升学习效果
- 每月免费获得一本电子书或视频
- 完全可搜索，便于快速访问重要信息
- 复制、粘贴、打印和书签收藏内容

你知道 Packt 为每本出版的书籍都提供电子书版本，文件格式为 PDF 和 ePub 吗？你可以在 packt.com 升级到电子书版本，作为印刷书籍的客户，你有权享受电子书折扣。请通过 customercare@packtpub.com 与我们联系了解更多详情。

在 www.packt.com，你还可以阅读大量免费技术文章，订阅多种免费通讯，并获得 Packt 书籍和电子书的独家折扣和优惠。

## 你可能感兴趣的其他书籍

如果你喜欢这本书，你可能对 Packt 的这些其他书籍感兴趣：

![](img/78092c233296cbd7723546ae74eec12d_164_0.png)

## Serverless 系统的软件架构模式

John Gilbert

ISBN: 978-1-80020-703-5

- 探索架构模式，创建在变化中蓬勃发展的反脆弱系统
- 专注于赋能自主、全栈团队的 DevOps 实践
- 构建企业级 serverless 系统
- 将微服务原则应用于前端
- 了解 SOLID 原则如何应用于软件和数据库架构
- 创建为事件溯源和 CQRS 模式提供动力的事件流处理器
- 部署多区域系统，包括区域健康检查、基于延迟的路由和复制
- 探索用于迁移遗留系统的绞杀者模式

![](img/78092c233296cbd7723546ae74eec12d_165_0.png)

## Azure 云原生架构地图集

Stéphane Eyskens, Ed Price

ISBN: 978-1-80056-232-5

- 获得对 Microsoft Azure 云平台的整体架构知识
- 通过考虑不同的架构视角，探索构建完整 Azure 解决方案的可能性
- 实施架构和部署 Azure 基础设施的最佳实践
- 回顾使用生态系统框架和解决方案构建分布式应用程序的不同模式
- 使用容器化工作负载掌握云原生概念
- 使用 AKS（Azure Kubernetes Service）并将其与服务网格技术结合使用来设计微服务托管平台

## Packt 正在寻找像你这样的作者

如果你有兴趣成为 Packt 的作者，请访问 authors.packtpub.com 并立即申请。我们已经与成千上万的开发人员和技术专业人士合作，就像你一样，帮助他们与全球技术社区分享他们的见解。你可以提交通用申请，申请我们正在招聘作者的特定热门主题，或提交你自己的想法。

## 分享你的想法

现在你已经完成了 *Software Architecture for Busy Developers*，我们很想听听你的想法！如果你是从 Amazon 购买的书籍，请点击此处直接前往该书在 Amazon 的评论页面并分享你的反馈，或在你购买的网站上留下评论。

你的评论对我们和技术社区都很重要，并将帮助我们确保提供优质内容。
