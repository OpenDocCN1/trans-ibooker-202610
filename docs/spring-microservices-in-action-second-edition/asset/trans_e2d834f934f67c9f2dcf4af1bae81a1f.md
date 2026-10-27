

## 11.3.1 设置 Spring Cloud Sleuth 和 Zipkin 依赖

我们已经在许可服务和组织服务中包含了 Spring Cloud Sleuth 依赖。这些 JAR 文件现在包含了必要的 Spring Cloud Sleuth 库，以便在服务中启用 Spring Cloud Sleuth。接下来，我们需要包含一个新依赖，即 `spring-cloud-sleuth-zipkin` 依赖，以便与 Zipkin 集成。下面的清单显示了 Spring Cloud Gateway、许可服务和组织服务中应该存在的 Maven 条目。

**清单 11.6 客户端 Spring Cloud Sleuth 和 Zipkin 依赖**

```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-sleuth-zipkin</artifactId>
</dependency>
```

## 11.3.2 配置服务以指向 Zipkin

JAR 文件就绪后，我们需要配置每个想要与 Zipkin 通信的服务。我们将通过设置一个 Spring 属性来定义用于与 Zipkin 通信的 URL。需要设置的属性是 `spring.zipkin.baseUrl` 属性。该属性在 Spring Cloud Config Server 仓库中每个服务的配置文件中设置（例如，许可服务的 `/configserver/src/main/resources/config/licensing-service.properties` 文件）。要在本地运行，请将 `baseUrl` 属性的值设置为 `localhost:9411`。但是，如果你想用 Docker 运行，则需要用 `zipkin:9411` 覆盖该值，如下所示：

```
zipkin.baseUrl: zipkin:9411
```

### Zipkin、RabbitMQ 和 Kafka

Zipkin 能够通过 RabbitMQ 或 Kafka 将其追踪数据发送到 Zipkin 服务器。从功能角度来看，如果我们使用 HTTP、RabbitMQ 或 Kafka，Zipkin 的行为没有区别。使用 HTTP 追踪时，Zipkin 使用异步线程发送性能数据。使用 RabbitMQ 或 Kafka 收集追踪数据的主要优点是，如果我们的 Zipkin 服务器宕机，发送到 Zipkin 的追踪消息会被“排队”，直到 Zipkin 能够获取数据。关于如何配置 Spring Cloud Sleuth 通过 RabbitMQ 和 Kafka 向 Zipkin 发送数据，Spring Cloud Sleuth 文档中已有介绍，因此我们在此不再赘述。

## 11.3.3 配置 Zipkin 服务器

有几种设置 Zipkin 的方法，但我们将使用带有 Zipkin 服务器的 Docker 容器。这个选项将使我们避免在架构中创建新项目。为了设置 Zipkin，我们将在项目的 Docker 文件夹中的 `docker-compose.yml` 文件中添加以下注册条目：

```
zipkin:
  image: openzipkin/zipkin
  container_name: zipkin
  ports:
    - 9411:9411
  networks:
    backend:
      aliases:
        - "zipkin"
```

运行 Zipkin 服务器几乎不需要什么配置。我们在运行 Zipkin 时需要配置的少数事项之一是 Zipkin 用于存储追踪数据的后端数据存储。Zipkin 支持四种不同的后端数据存储：

- 内存数据
- MySQL (http://mysql.com/)
- Cassandra (https://cassandra.apache.org/)
- Elasticsearch (http://elastic.co/)

默认情况下，Zipkin 使用内存数据存储。然而，Zipkin 团队建议不要在生产环境中使用内存数据库。内存数据库保存的数据量有限，并且当 Zipkin 服务器关闭或发生故障时，数据会丢失。

在本示例中，我们将演示如何使用 Elasticsearch 作为数据存储，因为我们已经配置了 Elasticsearch。我们需要添加的唯一额外设置是配置文件 environment 部分中的 `STORAGE_TYPE` 和 `ES_HOSTS` 变量。以下代码显示了完整的 Docker Compose 注册：

```
zipkin:
  image: openzipkin/zipkin
  container_name: zipkin
  depends_on:
    - elasticsearch
  environment:
    - STORAGE_TYPE=elasticsearch
    - "ES_HOSTS=elasticsearch:9300"
  ports:
    - "9411:9411"
  networks:
    backend:
      aliases:
        - "zipkin"
```

## 11.3.4 设置追踪级别

现在我们已经配置了客户端与 Zipkin 服务器通信，并且已经准备好了可运行的服务器。但在开始使用 Zipkin 之前，我们还需要执行一步，即定义每个服务应多久向 Zipkin 写入一次数据。

默认情况下，Zipkin 只将 10% 的事务写入 Zipkin 服务器。这个默认值确保了 Zipkin 不会压垮我们的日志和分析基础设施。

事务采样可以通过在每个向 Zipkin 发送数据的服务上设置 Spring 属性来控制：`spring.sleuth.sampler.percentage` 属性。该属性的取值在 0 到 1 之间，如下所示：

- 值为 0 表示 Spring Cloud Sleuth 不会向 Zipkin 发送任何事务。
- 值为 0.5 表示 Spring Cloud Sleuth 发送 50% 的事务。
- 值为 1 表示 Spring Cloud Sleuth 发送所有事务（100%）。

出于我们的目的，我们将为所有服务发送所有追踪信息（100%）和所有事务。为此，我们可以设置 `spring.sleuth.sampler.percentage` 的值，或者用 `AlwaysSampler` 替换 Spring Cloud Sleuth 中使用的默认 Sampler 类。`AlwaysSampler` 类可以作为 Spring bean 注入到应用程序中。但在这个示例中，我们将在许可服务、组织服务和网关服务的配置文件中使用 `spring.sleuth.sampler.percentage`，如下所示：

```
zipkin.baseUrl: zipkin:9411
spring.sleuth.sampler.percentage: 1
```

## 11.3.5 使用 Zipkin 追踪事务

让我们从一个场景开始本节。假设你是 O-stock 应用程序的开发人员之一，并且本周由你值班。你收到一个客户的工单，抱怨应用程序中的某个屏幕运行缓慢。你怀疑该屏幕使用的许可服务是罪魁祸首。但为什么？在哪里？

在我们的场景中，许可服务依赖于组织服务，并且这两个服务都会调用不同的数据库。哪个服务性能不佳？另外，你知道这些服务正在不断被修改，所以可能有人向其中添加了新的服务调用。

> 注意：理解用户事务中涉及的所有服务及其各自的性能时间，对于支持微服务架构等分布式架构至关重要。

为了解决这个难题，我们将使用 Zipkin 来观察来自组织服务的两个事务，由 Zipkin 服务对其进行追踪。组织服务是一个简单的服务，只调用一个数据库。我们要做的是使用 Postman 通过以下端点向组织服务发送两个调用。组织服务的调用在请求被定向到下游的组织服务实例之前，会先经过网关：

```
GET http://localhost:8072/organization/v1/organization/4d10ec24-141a-4980-be34-2ddb5e0458c6
```

如果我们查看图 11.15 中的截图，我们会看到 Zipkin 捕获了两个事务，并且每个事务被分解为一个或多个跨度（span）。在 Zipkin 中，跨度（span）表示捕获定时信息的特定服务或调用。图 11.15 中的每个事务都有五个跨度：网关中有两个跨度，组织服务有两个，许可服务有一个。

请记住，网关不会盲目转发 HTTP 调用。它接收传入的 HTTP 调用，将其终止，然后构建一个新的调用发送到目标服务（在本例中是组织服务）。原始调用的终止是网关能够为进入网关的每个调用添加响应、前置和后置过滤器的方式。这也是我们在图 11.15 中看到网关服务中有两个跨度的原因。

![](img/3c6dc613426280ca3eafe43dea24391e_360_0.png)

**图 11.15** Zipkin 查询屏幕，我们可以在其中选择要追踪的服务，以及一些基本查询过滤器。

通过网关对组织服务的两次调用分别耗时 1.151 秒和 39.152 毫秒。让我们深入研究耗时最长的调用（1.151 秒）的细节。我们可以通过点击事务并下钻来查看更多细节。图 11.16 显示了我们对组织服务调用的详细信息。

![](img/3c6dc613426280ca3eafe43dea24391e_360_1.png)

**图 11.16** Zipkin 允许我们下钻并查看每个跨度在事务中花费的时间。

