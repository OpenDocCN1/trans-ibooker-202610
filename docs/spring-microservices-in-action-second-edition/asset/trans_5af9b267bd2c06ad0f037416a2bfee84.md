

现在我们可以开始向服务发送请求，以查看 Kibana 中的实时日志。图 11.11 展示了发送到 ELK 的数据应呈现的样子。如果你没有看到图中显示的页面，请再次点击 Discover 图标。

![](img/3c6dc613426280ca3eafe43dea24391e_352_0.png)

至此，我们已经完成了 Kibana 的全部设置。接下来继续最后一步。

## 11.2.5 在 Kibana 中搜索 Spring Cloud Sleuth 追踪 ID

现在日志已经流入 ELK，我们可以开始了解 Spring Cloud Sleuth 是如何为日志条目添加追踪 ID 的。要查询与单个事务相关的所有日志条目，我们需要获取一个追踪 ID，并在 Kibana 的 Discover 界面（图 11.12）中进行查询。默认情况下，Kibana 使用 Kibana 查询语言（KQL），这是一种简化的查询语法。在编写查询时，你会看到 Kibana 还提供了指南和自动补全选项，以简化创建自定义查询的过程。

> **注意** 要应用下一个过滤器，你需要选择一个有效的追踪 ID。下一个示例中使用的追踪 ID 在你的 Kibana 实例中将无法生效。

图 11.12 展示了如何使用 Spring Cloud Sleuth 追踪 ID 执行查询。这里我们使用追踪 ID `3ff985508b1b9365`。

如果需要查看更详细的信息，我们可以展开每条日志事件。这样，与特定事件关联的所有字段将以表格或 JSON 格式显示。我们还可以看到在 Logstash 处理期间添加或转换的所有附加信息。例如，在清单 11.3 中，我们使用 Logstash 中的 mutate 过滤器添加了一个标签。图 11.13 显示了这个事件的所有字段。

## 11.2.6 使用 Spring Cloud Gateway 将关联 ID 添加到 HTTP 响应中

如果我们检查由 Spring Cloud Sleuth 发起的任意服务调用的 HTTP 响应，就会发现调用中使用的追踪 ID 永远不会出现在 HTTP 响应头中。如果我们查阅 Spring Cloud Sleuth 的文档，会看到其开发团队认为返回任何追踪数据都可能带来潜在的安全问题（尽管他们并未明确说明他们这样认为的原因）。但我们发现，在调试问题时，在 HTTP 响应中返回关联 ID 或追踪 ID 的价值无可估量。

Spring Cloud Sleuth 允许我们使用其追踪 ID 和跨度 ID 来“装饰”HTTP 响应信息。然而，实现这一过程需要编写三个类并注入两个自定义 Spring Bean。如果你想采用这种方法，可以在 Spring Cloud Sleuth 文档中查看相关内容：

- https://cloud.spring.io/spring-cloud-static/spring-cloud-sleuth/1.0.12.RELEASE/

一个简单得多的解决方案是编写一个 Spring Cloud Gateway 过滤器，将追踪 ID 注入 HTTP 响应。在第 8 章中介绍 Spring Cloud Gateway 时，我们看到了如何构建一个网关响应过滤器，将生成的关联 ID 添加到调用方返回的 HTTP 响应中，供我们的服务使用。现在，我们将修改该过滤器，以添加 Spring Cloud Sleuth 响应头。为了设置网关响应过滤器，我们需要确保 pom.xml 文件中包含 Spring Cloud Sleuth 依赖，如下所示：

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-sleuth</artifactId>
</dependency>
```

我们将使用 spring-cloud-starter-sleuth 依赖来告知 Spring Cloud Sleuth，我们希望网关参与 Spring Cloud 追踪。在本章后面介绍 Zipkin 时，我们会看到网关服务将是任何服务调用中的第一个调用。一旦依赖就绪，实际的响应过滤器实现起来就非常简单了。

下面的清单展示了用于构建该过滤器的源代码。文件位于 `/gatewayserver/src/main/java/com/optimagrowth/gateway/filters/ResponseFilter.java`。

**清单 11.5** 通过响应过滤器添加 Spring Cloud Sleuth 追踪 ID

```java
package com.optimagrowth.gateway.filters;
// Imports removed for conciseness.
import brave.Tracer;
import reactor.core.publisher.Mono;

@Configuration
public class ResponseFilter {

    final Logger logger = LoggerFactory.getLogger(ResponseFilter.class);

    @Autowired
    Tracer tracer;

    @Autowired
    FilterUtils filterUtils;

    @Bean
    public GlobalFilter postGlobalFilter() {
        return (exchange, chain) -> {
            return chain.filter(exchange)
                .then(Mono.fromRunnable(() -> {
                    String traceId =
                        tracer.currentSpan()
                            .context()
                            .traceIdString();
                    logger.debug("Adding the correlation id to the outbound headers. {}", traceId);
                    exchange.getResponse().getHeaders()
                        .add(FilterUtils.CORRELATION_ID, traceId);
                    logger.debug("Completing outgoing request for {}.",
                        exchange.getRequest().getURI());
                }));
        };
    }
}
```

设置访问追踪和跨度 ID 信息的入口。

将 Spring Cloud Sleuth 追踪 ID 添加到 HTTP 响应头 `tmx-correlation-id` 中。

由于网关现在已启用 Spring Cloud Sleuth，我们可以通过自动装配 Tracer 类来访问 ResponseFilter 中的追踪信息。这个类允许我们访问当前已执行追踪的相关信息。`tracer.currentSpan().context().traceIdString()` 方法使我们能够以字符串形式检索当前事务的追踪 ID。将追踪 ID 添加到经网关返回的出站 HTTP 响应中非常简单。通过以下方法调用即可完成：

```java
exchange.getResponse().getHeaders()
    .add(FilterUtils.CORRELATION_ID, traceId);
```

有了这段代码后，如果通过网关调用一个 O-stock 微服务，我们应该会在 HTTP 响应头中看到一个名为 `tmx-correlation-id` 的响应头，其值为 Spring Cloud Sleuth 追踪 ID。图 11.14 展示了对以下端点发起 GET 请求以获取组织许可证的调用结果：

```
http://localhost:8072/license/v1/organization/4d10ec24-141a-4980-be34-2ddb5e0458c7/license/4af05c3b-a0f3-411d-b5ff-892c62710e14
```

![](img/3c6dc613426280ca3eafe43dea24391e_356_0.png)

图 11.14 返回 Spring Cloud Sleuth 追踪 ID 后，我们可以轻松地在 Kibana 中查询日志。

## 11.3 使用 Zipkin 进行分布式追踪

拥有一个带关联 ID 的统一日志平台是一种强大的调试工具。然而，在本章余下的内容中，我们将不再追踪日志条目，而是着眼于如何将事务在不同微服务之间流动的过程可视化。一幅清晰简洁的图片胜过百万条日志记录。

分布式追踪涉及提供事务如何在我们不同的微服务之间流动的可视化图景。分布式追踪工具还可以大致估算单个微服务的响应时间。但是，不应将分布式追踪工具与完整的应用性能管理（APM）套件混淆。APM 套件不仅提供有关实际服务代码的即开即用的底层性能数据，还提供响应时间之外的内存、CPU 利用率和 I/O 利用率等性能数据。

这正是 Spring Cloud Sleuth 和 Zipkin（也称为 OpenZipkin）项目大显身手的地方。Zipkin (http://zipkin.io/) 是一个分布式追踪平台，允许我们在多个服务调用之间追踪事务。它可以让我们以图形方式查看事务所花费的时间，并将其分解为调用中涉及每个微服务的时间。Zipkin 是识别微服务架构中性能问题的宝贵工具。设置 Spring Cloud Sleuth 和 Zipkin 涉及：

- 将 Spring Cloud Sleuth 和 Zipkin JAR 文件添加到捕获追踪数据的服务中
- 在每个服务中配置一个 Spring 属性，以指向收集追踪数据的 Zipkin 服务器
- 安装并配置 Zipkin 服务器以收集数据
- 定义每个客户端用于将追踪信息发送到 Zipkin 的采样策略

