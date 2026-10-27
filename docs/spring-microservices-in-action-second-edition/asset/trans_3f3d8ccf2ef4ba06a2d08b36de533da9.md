

在图 11.16 中，我们可以看到从网关的角度来看，整个事务大约花费了 1.151 秒。然而，网关所做的组织服务调用占用了整体调用中 1.151 秒中的 524.689 毫秒。让我们进一步深入每个 span 以获得更多细节。点击 organization-service span 并注意调用的额外细节（图 11.17）。

![](img/3c6dc613426280ca3eafe43dea24391e_361_0.png)

图 11.17 点击单个 span 可提供有关 HTTP 调用和时序的进一步详细信息。

图 11.17 中最有价值的信息之一是客户端（网关）调用组织服务时的分解、组织服务接收调用的时间以及组织服务响应的时间。这种类型的时序信息对于检测和识别网络延迟问题非常宝贵。为了向许可服务的 Redis 调用添加自定义 span，我们将使用以下类：

```
/licensing-service/src/main/java/com/optimagrowth/license/service/client/
OrganizationRestTemplateClient.java
```

使用 OrganizationRestTemplateClient，我们将实现 checkRedisCache() 方法。以下列表显示了此代码。

## Listing 11.7 Adding checkRedisCache() to OrganizationRestTemplate

```
package com.optimagrowth.license.service.client;
//Rest of imports omitted for conciseness

@Component
public class OrganizationRestTemplateClient {
    @Autowired
    RestTemplate restTemplate;

    @Autowired
    Tracer tracer;

    @Autowired
    OrganizationRedisRepository redisRepository;

    private static final Logger logger =
        LoggerFactory.getLogger(OrganizationRestTemplateClient.class);

    private Organization checkRedisCache
        (String organizationId) {
        try {
            return redisRepository.findById(organizationId).orElse(null);
        } catch (Exception ex){
            logger.error("Error encountered while trying to retrieve organization {} check Redis Cache. Exception {}",
                organizationId, ex);
            return null;
        }
    }

    //Rest of class omitted for conciseness
}
```

Tracer 访问 Spring Cloud Sleuth 跟踪信息。

实现 CheckRedisCache 方法

## 11.3.6 Visualizing a more complex transaction

如果我们想精确了解服务调用之间存在哪些服务依赖关系怎么办？我们可以通过网关调用许可服务，然后使用 GET 调用查询 Zipkin 获取许可服务的跟踪，使用以下端点。图 11.18 显示了此调用的详细跟踪。

```
http://localhost:8072/license/v1/organization/4d10ec24-141a-4980-be34-2ddb5e0458c8/license/4af05c3b-a0f3-411d-b5ff-892c62710e15
```

在图 11.18 中，我们可以看到对许可服务的调用涉及八次 HTTP 调用。我们看到对网关的调用，然后从网关到许可服务；许可服务到网关以及网关到组织；最后，组织使用 Apache Kafka 通过许可服务更新 Redis 缓存。

![](img/3c6dc613426280ca3eafe43dea24391e_363_0.png)

图 11.18 查看跟踪的详细信息，显示许可服务调用如何从网关流向许可服务，然后流向组织服务。

## 11.3.7 Capturing messaging traces

消息传递会在应用程序内部引入其自身性能和延迟问题。一个服务可能无法足够快地处理队列中的消息。或者可能存在网络延迟问题。在构建基于微服务的应用程序时，我们遇到了这些场景。

Spring Cloud Sleuth 在服务中注册的任何入站或出站消息通道上发送 Zipkin 跟踪数据。使用 Spring Cloud Sleuth 和 Zipkin，我们可以识别消息何时从队列发布以及何时接收。我们还可以看到消息在队列上接收并处理时发生的行为。而且，你会记得在第 10 章中，当添加、更新或删除组织记录时，会通过 Spring Cloud Stream 生产并发布 Kafka 消息。许可服务接收消息并更新其用于缓存数据的 Redis 键值存储。

接下来我们将删除一个组织记录并观察 Spring Cloud Sleuth 和 Zipkin 跟踪事务。我们将通过 Postman 向组织服务的以下端点发出 DELETE 请求：

```
http://localhost:8072/organization/v1/organization/4d10ec24-141a-4980-be34-2ddb5e0458c7
```

本章前面，我们看到了如何将 trace ID 添加为 HTTP 响应头，添加一个名为 tmx-correlation-id 的新 HTTP 响应头。我们在调用中获得了 tmx-correlation-id，值为 054accff01c9ba6b。我们可以通过在 Zipkin 查询屏幕右上角的搜索框中输入调用返回的 trace ID 来在 Zipkin 中搜索此特定跟踪。图 11.19 显示了我们可以在哪里输入 trace ID。

有了特定的跟踪，我们可以查询 Zipkin 获取事务并查看 DELETE 消息的发布。图 11.20 中的第二个 span 是来自 channel output 的输出消息，用于发布到名为 orgChangeTopic 的 Kafka 主题。图 11.20 显示了输出消息通道及其在 Zipkin 跟踪中的外观。

![](img/3c6dc613426280ca3eafe43dea24391e_364_0.png)

**Figure 11.19** 通过 HTTP `tmx-correlation-id` 响应头返回的 trace ID，我们可以通过在特定 trace ID 上搜索轻松找到事务。

![](img/3c6dc613426280ca3eafe43dea24391e_364_1.png)

**Figure 11.20** Spring Cloud Sleuth 自动跟踪 Spring 消息通道上消息的发布和接收。我们可以使用 Zipkin 查看跟踪的详细信息。

你可以通过点击许可服务 span 来查看许可服务接收消息。图 11.21 显示了此数据。

到目前为止，我们一直使用 Zipkin 来跟踪服务内部的 HTTP 和消息传递调用。但是，如果我们想对未被 Zipkin 监控的第三方服务进行跟踪怎么办？例如，如果我们想获取特定 Redis 或 Postgres SQL 调用的跟踪和时序信息怎么办？幸运的是，Spring Cloud Sleuth 和 Zipkin 允许我们向事务添加自定义 span，以便我们可以跟踪与第三方调用相关的执行时间。

## 11.3.8 Adding custom spans

使用 Zipkin 添加自定义 span 非常容易。我们将首先向许可服务添加一个自定义 span，以便我们可以跟踪从 Redis 获取数据所需的时间。然后我们将向组织服务添加一个自定义 span，以查看从组织数据库检索数据所需的时间。以下列表为许可服务创建了一个名为 readLicensingDataFromRedis 的自定义 span。

## Listing 11.8 Adding the Spring Cloud Sleuth trace ID via a response filter

```
package com.optimagrowth.license.service.client;
//Rest of code removed for conciseness

    private Organization checkRedisCache(String organizationId) {
        ScopedSpan newSpan =
            tracer.startScopedSpan
            ("readLicensingDataFromRedis");
        try {
            return redisRepository.findById(organizationId).orElse(null);
        } catch (Exception ex){
            logger.error("Error encountered while trying to retrieve organization {} check Redis Cache. Exception {}",
                organizationId, ex);
            return null;
        } finally {
            newSpan.tag("peer.service", "redis");
            newSpan.annotate("Client received");
            newSpan.finish();
        }
    }
```

创建名为 readLicensingDataFromRedis 的自定义 span

向 span 添加标签信息并命名 Zipkin 将捕获的服务

关闭并完成 span。如果未执行此操作，我们将在日志中收到 span 未关闭的错误消息。

图 11.21 Zipkin 让我们看到组织服务发布的 Kafka 消息。

![](img/3c6dc613426280ca3eafe43dea24391e_365_0.png)

