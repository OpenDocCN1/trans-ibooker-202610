

```
$ az servicebus namespace authorization-rule keys list --resource-group $RES_GROUP --namespace-name $NAMESPACE_NAME --name RootManageSharedAccessKey --query primaryConnectionString
```

Azure 提供开源库，可在所有流行的编程语言中向 Azure 服务发送和接收消息。以下示例在 Python 中构建了一个简单的生产者和消费者：

```
from azure.servicebus import ServiceBusClient, ServiceBusMessage

import os
connstr = os.environ['SERVICE_BUS_CONN_STR']
queue_name = os.environ['SERVICE_BUS_QUEUE_NAME']

messages = ["Testing 1","Testing 2", "Testing 3"]

with ServiceBusClient.from_connection_string(connstr) as client:
    s = client.get_queue_sender(queue_name)
    r = client.get_queue_receiver(queue_name)

    print(s,r)

    for m in messages:
        s.send_messages(ServiceBusMessage(m))

    while True:
        print(r.receive_messages(max_message_count=1,max_wait_time=10))
```

除了 RabbitMQ 之外，熟悉 AWS SNS 和 SQS 的读者会发现 Service Bus 非常直观，能够满足类似的使用场景。

## Azure Event Hubs

Azure Event Hubs 设计用于提供 Apache Kafka 的部分功能，以满足分区和消费者组的使用场景，但以托管的云服务形式提供（见图 10-15）。虽然你失去了在虚拟机或 Kubernetes 上运行 Kafka 时对后端基础设施的可见性，但你获得了与 Azure Blob Storage、Azure Data Lakes 和 Azure Stream Analytics 等其他数据服务更紧密的集成，等等。Azure 还支持 AMQP 1.0 和 Kafka 1.0 协议，以保持与客户端库的兼容性。

![](img/8d47d1b5dbca55e784ae602145a894c7_349_0.png)

图 10-15. Azure Event Hubs 架构

Azure Event Hubs 有一些概念是从 Azure Service Bus 转移过来的。我们在这里快速回顾一下：

*生产者*

这些是向 Azure Event Hubs 发送消息的客户端。

*分区*

消息被分片到各个分区，以便消息的副本可以被存储。

### 消费者组

这些是协同工作以协调方式消费消息的客户端。

### 使用 Terraform 管理 Azure Event Hubs

为向 Azure Event Hubs 发送消息而预配所需资源的 Terraform 代码与我们之前用于在 Azure Service Bus 中配置队列和主题的代码非常相似。我们必须声明一个资源组和一个命名空间，我们的 Event Hubs 实例将属于该命名空间：

```
provider "azurerm" {
  features {}
}

resource "azurerm_resource_group" "example" {
  name     = "mdfranz-eventbus-group"
  location = "East US"
}

resource "azurerm_eventhub_namespace" "example" {
  name                = "mdfranz-eventhub-namespace"
  location            = "East US"
  resource_group_name = azurerm_resource_group.example.name
  sku                 = "Standard"
  capacity            = 1
}

resource "azurerm_eventhub" "example" {
  name                = "mdfranz-eventhub"
  namespace_name      = azurerm_eventhub_namespace.example.name
  resource_group_name = azurerm_resource_group.example.name
  partition_count     = 1
  message_retention   = 1
}

resource "azurerm_eventhub_consumer_group" "example" {
  name                = "mdfranz-consumergroup"
  resource_group_name = azurerm_resource_group.example.name
  namespace_name      = azurerm_eventhub_namespace.example.name
  eventhub_name       = azurerm_eventhub.example.name
}
```

创建消费者组是可选的，因为存在默认消费者组。还请注意，具有多个消费者组需要标准 SKU 而不是基本 SKU。

与 Azure Event Bus 一样，有多种方式可以对 Event Hubs 端点进行身份验证。最简单的方法是使用连接字符串，如以下示例所示：

```
$ az eventhubs namespace authorization-rule keys list --resource-group mdfranz-eventbus-group --namespace-name mdfranz-eventhub-namespace --name RootManageSharedAccessKey
{
  "aliasPrimaryConnectionString": null,
  "aliasSecondaryConnectionString": null,
  "keyName": "RootManageSharedAccessKey",
  "primaryConnectionString": "Endpoint=sb://mdfranz-eventhub-namespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=jFMUbfQZX8UjGjcCKLAeIp5CZVLcWcSGpuKdJX/CMzK=",
  "primaryKey": "jFMUbfQZX8UjGjcCKLAeIp5CZVLcWcSGpuKdJX/CMzK=",
  "secondaryConnectionString": "Endpoint=sb://mdfranz-eventhub-namespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=gFy7SuuGjw12GfoHXd6UQeZdy5Y0xu/gStZtQUhaxsY=",
  "secondaryKey": "gFy7SuuGjw12GfoHXd6UQeZdy5Y0xu/gStZtQUhaxsY="
}
```

> 注意
> 参数 EntityPath=mdfanz-eventhub（即 Endpoint=sb://mdfanz-eventhub-namespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;EntityPath=mdfanz-eventhubSharedAccessKey=jfMUbFqZX8UjG1cKLAeIp5CZVLcwcSGpuKdJX/CMzk=）必须添加到连接字符串中，因为如果你使用 NewHubfromConnectionString 创建客户端，Send 方法不允许将事件网格名称指定为参数，因此你需要改为添加 EntityPath。

## Azure Event Grid

Azure Event Grid 于 2018 年正式发布，是 Azure 提供的最佳消息代理之一，因为它允许多个事件源路由到 Azure 生态系统中的各种目的地。消息可以发送到/从我们刚刚讨论的现有消息服务（Azure Service Bus 和 Azure Event Hubs）以及其他服务和端点（见图 10-16），例如 Azure Functions、Web Hooks、Logic Apps，以及其他应用内用途（例如跟踪用户与应用程序的交互方式）。Azure Event Hubs 是一种首屈一指的“无服务器”产品，按事件计费。

![](img/8d47d1b5dbca55e784ae602145a894c7_351_0.png)

图 10-16. Event Grid 生态系统

在图 10-16 中，请注意 Event Grid 的推送特性，与我们在其他系统中习惯的通用推拉拓扑不同。

Azure Event Grid 使用的术语与我们讨论的其他平台略有不同：

### 域

域类似于命名空间，用于在租户内分隔使用场景。

### 主题

对于 Event Grid，存在两种类型的主题：系统主题，来自 Azure 系统（Event Hubs、Blob Storage 等），如图 10-16 左侧所示；以及自定义主题，可以是您自己的自定义方案，包装在 Event Grid Schema 中。

### 部署和使用事件网格

Azure 事件网格具有简约的 Terraform 部署，如以下示例所示：

```
resource "azurerm_resource_group" "example" {
  name     = "example-resources"
  location = "East US"
}
```

```
resource "azurerm_eventgrid_topic" "example" {
  name                = "my-eventgrid-topic"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name

  tags = {
    environment = "Production"
  }
}
```

你需要更改主题的名称，因为它在你部署到的 Azure 区域中是全局唯一的。一旦此 Terraform 部署应用完成，将创建一个新的 Event Grid 主题，名称为 https://<event-grid-topic-name>.<topic-location>.eventgrid.azure.net/api/events；或在我们的例子中，为 https://my-eventgrid-topic.east-us.eventgrid.azure.net/api/events。你可以通过运行 az eventgrid topic key list --resource-group example-resources --name my-eventgrid-topic 来找到该主题的访问密钥。

这里有一个使用 azure-eventgrid Python 包的简单生产者：

```
from azure.identity import DefaultAzureCredential
from azure.eventgrid import EventGridPublisherClient, EventGridEvent

event = EventGridEvent(
    data={"team": "azure-sdk"},
    subject="Door1",
    event_type="Azure.Sdk.Demo",
    data_version="2.0"
)

credential = DefaultAzureCredential("<access-key>")
endpoint = `https://my-eventgrid-topic.east-us.eventgrid.azure.net/api/events`
client = EventGridPublisherClient(endpoint, credential)
client.send(event)
```

大量关于如何使用 Event Grid 编程的优秀示例可在 Azure Event Grid Python 客户端库 中找到。

## 总结

在为新应用或现有应用选择消息技术时，你有很多选择。像往常一样，关于如何运行某项技术的答案通常是“视情况而定！”我们在本章中讨论的开源和 Azure 消息产品之间存在显著的功能重叠。在选择消息服务之前，确保你了解自己的持久性、规模和安全需求，将为你部署消息基础设施奠定成功的基础。我们现在将进入我们最后的基础设施部分，无服务器计算。

