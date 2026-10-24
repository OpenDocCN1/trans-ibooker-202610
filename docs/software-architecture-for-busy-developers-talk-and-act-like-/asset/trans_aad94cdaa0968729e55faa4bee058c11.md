

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

