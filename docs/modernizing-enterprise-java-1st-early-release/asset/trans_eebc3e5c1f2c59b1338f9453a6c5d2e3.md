

2020-12-02 17:12:20.220  INFO 61053 --- [        main]
...

你的应用现在已在端口 9000 上监听我们配置的端点，你可以通过导航到 http://localhost:9000/ 来验证它。

你应该能从 REST API 看到以下输出，返回一个表示产品列表的 JSON 对象：

```
[
  {
    "itemId":"100000",
    "name":"Red Fedora",
    "description":"Official Red Hat Fedora",
    "price":34.99
  },
  {
    "itemId":"329299",
    "name":"Quarkus T-shirt",
    "description":"This updated unisex essential fits like a well-loved favorite, featuring a crew neck, short sleeves and designed with superior combed and ring-spun cotton.",
    "price":10.0
  },
  {
    "itemId":"329199",
    "name":"Pronounced Kubernetes",
    "description":"Kubernetes is changing how enterprises work in the cloud. But one of the biggest questions people have is: How do you pronounce it?",
    "price":9.0
  },
  {
    "itemId":"165613",
    "name":"Knit socks",
    "description":"Your brand will get noticed on these full color knit socks. Imported.",
    "price":4.15
  },
  {
    "itemId":"165614",
    "name":"Quarkus H2Go water bottle",
    "description":"Sporty 16. 9 oz double wall stainless steel thermal bottle\ with copper vacuum insulation, and threaded insulated lid. Imprinted. Imported.",
    "price":14.45
  },
  {
    "itemId":"165954",
    "name":"Patagonia Refugio pack 28L",
    "description":"Made from 630-denier 100% nylon (50% recycled/50% high-tenacity)\ plain weave; lined with 200-denier 100% recycled polyester. ...",
    "price":6.0
  },
  {
    "itemId":"444434",
    "name":"Red Hat Impact T-shirt",
    "description":"This 4. 3 ounce, 60% combed ringspun cotton/40% polyester\ jersey t- shirt features a slightly heathered appearance. The fabric\ laundered for reduced shrinkage. Next Level brand apparel. Printed.",
    "price":9.0
  },
  {
    "itemId":"444435",
    "name":"Quarkus twill cap",
    "description":"100% cotton chino twill cap with an unstructured, low-profile,\ six-panel design. The crown measures 3 1/8 and this features a Permacurv\ visor and a buckle closure with a grommet.",
    "price":13.0
  },
  {
    "itemId":"444437",
    "name":"Nanobloc Universal Webcam Cover",
    "description":"NanoBloc Webcam Cover fits phone, laptop, desktop, Pc,\ MacBook Pro, iMac, ...",
    "price":2.75
  }
]
```

> NOTE

本书代码列表中输出已在 `pretty` 模式下格式化。你会注意到我们来自 Quarkus Inventory 微服务的商品与来自 Spring Boot Catalog 微服务的描述和价格的组合。如果回忆起之前对商品 329299 的测试信息，它就是一个 `Quarkus T-shirt`。

恭喜你创建了第二个微服务，现在是时候为我们的后端连接一个前端了。为此，我们将在下一节中使用具有 Reactive Java 的软件 API 网关。

## 使用 Vert.x 创建网关服务

[Eclipse Vert.x](Eclipse Vert.x) 是一个用于在 Java 虚拟机 JVM 上构建响应式应用的事件驱动工具包。Vert.x 不强制特定框架或打包模型，它可以在现有应用和框架中使用，只需将 Vert.x jar 文件添加到应用类路径即可添加响应式功能。

Eclipse Vert.x 实现了 [The Reactive Manifesto](The Reactive Manifesto) 定义的响应式系统，并构建服务使其具备：

+   - Responsive: 以合理时间处理请求
+   - Resilient: 在出现故障时保持响应
+   - Elastic: 在各种负载下保持响应并能够水平伸缩
+   - Message-driven: 组件通过异步消息传递进行交互

它被设计为事件驱动且非阻塞，事实上事件被传递到绝不能被阻塞的事件循环中。与传统应用不同，Vert.x 使用很少的线程负责将事件分派给事件处理器。如果事件循环被阻塞，事件将不再被传递，因此代码需要留意这种执行模型（如图 2-7 所示）。

![](img/8e80afd9c3d238f2221423409b47c62a_62_0.png)

图 2-7. Vert.x 事件循环

在我们的架构中，此微服务将充当异步软件 API 网关，作为一个响应式 Java 微服务开发，高效地将流量路由和分发到云原生电子商务网站的 Inventory 和 Catalog 组件。

![](img/8e80afd9c3d238f2221423409b47c62a_63_0.png)

图 2-8. API 网关 Vert.x 微服务

你可以在此 [book’s GitHub repository](book’s GitHub repository) 找到此微服务的源代码。

## 创建 Vert.x Maven 项目

Vert.x 同时支持 *Maven* 和 *Gradle*，创建新 Vert.x Maven 项目最简单的方式是通过 Vert.x 社区提供的模板项目结构，可在 https://github.com/vert-x3/vertx-maven-starter 获取。

在我们的案例中，我们使用 Red Hat Maven 仓库并添加了 [Table 2-5](Table 2-5) 中所示的设置。

Table 2-5. Vert. x Maven Project Opti

```
<version>${slf4j.version}</version>
</dependency>
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-jdk14</artifactId>
    <version>${slf4j.version}</version>
</dependency>
</dependencies>
...
</project>
```

+   1. 项目元数据
+   2. 使用的 Vert.x 版本
+   3. `GatewayVerticle`：主 Verticle 的名称，它是我们应用的入口点
+   4. 依赖列表：
+      - Vert.x 库：vertx-core, vertx-config, vertx-web, vertx-web-client
+      - Vert.x 的 Rx 支持：vertx-rx-java2

## 创建 API 网关

接下来，我们要创建一个 API 网关作为我们网站 Web 前端的入口点，以从单一位置访问所有后端服务。这种模式可预测地称为 API 网关，是微服务架构中的常见做法。

Vert.x 中的部署单元称为 Verticle。一个 verticle 在事件循环上处理传入事件，其中事件可以是任何内容，如接收网络缓冲区、计时事件，或由其他 verticle 发送的消息。

我们将主 Verticle 定义为 GatewayVerticle，如我们在 pom.xml 中之前声明的那样，并暴露将路由到 Catalog /api/catalog 的 REST 端点，如 Table 2-6 所示。

| Path | HTTP Method | Description |
|---|---|---|
| /api/catalog/ GET |  | 路由流量到 Catalog 并返回包含商店中所有商品的 JSON 对象，将 Inventory 服务中的商品与 Catalog 服务中的数据进行匹配。 |

你可以在此 [book’s GitHub repository](book’s GitHub repository) 找到它。

```
package com.redhat.cloudnative.gateway;

import io.vertx.core.http.HttpMethod;
import io.vertx.core.json.JsonArray;
import io.vertx.core.json.JsonObject;
import io.vertx.ext.web.client.WebClientOptions;
import io.vertx.reactivex.config.ConfigRetriever;
import io.vertx.reactivex.core.AbstractVerticle;
import io.vertx.reactivex.ext.web.Router;
import io.vertx.reactivex.ext.web.RoutingContext;
import io.vertx.reactivex.ext.web.client.WebClient;
import io.vertx.reactivex.ext.web.client.predicate.ResponsePredicate;
import io.vertx.reactivex.ext.web.codec.BodyCodec;
import io.vertx.reactivex.ext.web.handler.CorsHandler;
import io.vertx.reactivex.ext.web.handler.StaticHandler;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import io.reactivex.Observable;
import io.reactivex.Single;

import java.util.ArrayList;
import java.util.List;

public class GatewayVerticle extends AbstractVerticle { ①
    private static final Logger LOG =
    LoggerFactory.getLogger(GatewayVerticle.class);

    private WebClient catalog;
```

```
```java
private WebClient inventory;

@Override
public void start() {
  Router router = Router.router(vertx);
  router.route().handler(CorsHandler.create("*")
    .allowedMethod(HttpMethod.GET));
  router.get("/*").handler(StaticHandler.create("assets"));
  router.get("/health").handler(this::health);
  router.get("/api/products").handler(this::products);

  ConfigRetriever retriever =
      ConfigRetriever.create(vertx);
  retriever.getConfig(ar -> {
    if (ar.failed()) {
      // Failed to retrieve the configuration
    } else {
      JsonObject config = ar.result();

      String catalogApiHost =
          config.getString("COMPONENT_CATALOG_HOST", "localhost");

      Integer catalogApiPort =
          config.getInteger("COMPONENT_CATALOG_PORT", 9000);

      catalog = WebClient.create(vertx,
          new WebClientOptions()
            .setDefaultHost(catalogApiHost)
            .setDefaultPort(catalogApiPort));

      LOG.info("Catalog Service Endpoint: " +
          catalogApiHost + ":" + catalogApiPort.toString());

      String inventoryApiHost =
          config.getString("COMPONENT_INVENTORY_HOST", "localhost");

      Integer inventoryApiPort =
          config.getInteger("COMPONENT_INVENTORY_PORT", 8080);

      inventory = WebClient.create(vertx,
          new WebClientOptions()
            .setDefaultHost(inventoryApiHost)
            .setDefaultPort(inventoryApiPort));

      LOG.info("Inventory Service Endpoint: ");
```

