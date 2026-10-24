
2020-12-02 17:12:20.220  INFO 61053 --- [        main]
...

Your app is now listening on port 9000 to the endpoint we configured, you can verify it by navigating to http://localhost:9000/

You should see this output from the REST API returning a JSON object representing the product list:

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

The output has been formatted in `pretty` mode in book’s code listing. You’ll notice the combination of our items from the Quarkus Inventory microservice with the description and the price from the Spring boot Catalog microservice. If you recall the info from the previous test with item 329299, it’s a `Quarkus T-shirt`.

Congratulations on creating your second microservice, now it’s time to connect a Frontend to our Backends. In order to do it, we will use a software API gateway with Reactive Java in the next section.

## Create a Gateway Service with Vert.x

[Eclipse Vert.x](Eclipse Vert.x) is an event-driven toolkit for building reactive applications on the Java Virtual Machine (JVM). Vert.x does not impose a specific framework or packaging model, it can be used within your existing applications and frameworks in order to add reactive functionality by just adding the Vert.x jar files to the application classpath.

Eclipse Vert.x enables building reactive systems as defined by [The Reactive Manifesto](The Reactive Manifesto) and build services that are:

+   - Responsive: to handle requests in a reasonable time
+   - Resilient: to stay responsive in the face of failures
+   - Elastic: to stay responsive under various loads and be able to scale up and down
+   - Message-driven: components interact using asynchronous message-passing

It is designed to be event-driven and non-blocking, in fact events are delivered into an event loop that must never be blocked. Unlike traditional applications, Vert.x uses a very small number of threads responsible for dispatching the events to event handlers. If the event loop is blocked, the

events won’t be delivered anymore and therefore the code needs to be mindful of this execution model (as shown in Figure 2-7).

![](img/8e80afd9c3d238f2221423409b47c62a_62_0.png)

Figure 2-7. Vert.x Event Loop

In our Architecture, this microservice will act as an asynchronous software API Gateway, developed as a reactive Java microservice that routes and dispatches efficiently the traffic to the Inventory and Catalog component of Cloud Native e-commerce website.

![](img/8e80afd9c3d238f2221423409b47c62a_63_0.png)

Figure 2-8. API Gateway Vert.x microservice

You find the source code of this microservice in this [book’s GitHub repository](book’s GitHub repository).

## Create Vert.x Maven project

Vert.x supports both *Maven* and *Gradle*, and the easiest way to bootstrap a new Vert.x Maven project is through a template project structure offered by Vert.x community available at https://github.com/vert-x3/vertx-maven-starter.

In our case we are using Red Hat Maven repositories and added the settings shown in [Table 2-5](Table 2-5).

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

+   1. Project metadata
+   2. Vert.x version used
+   3. `GatewayVerticle`: the name of the main Verticle, it’s the entry point for our app
+   4. A list of dependecies:
+      - Vert.x libraries: vertx-core, vertx-config, vertx-web, vertx-web-client
+      - Rx support for Vert.x: vertx-rx-java2

## Create an API Gateway

Next, we want to create an API Gateway as the entry point for the web frontend of our website, to access all backend services from a single place. This pattern is predictably called API Gateway and is a common practice in Microservices architecture.

The unit of deployment in Vert.x is called a Verticle. A verticle processes incoming events over an event-loop, where events can be anything like receiving network buffers, timing events, or messages sent by other verticles.

We define our main Verticle as GatewayVerticle as we declared it previously in the pom.xml, and expose the REST endpoint that will be routed to the Catalog /api/catalog as shown in Table 2-6.

| Path | HTTP Method | Description |
|---|---|---|
| /api/catalog/ GET |  | Routes the traffic to Catalog and returns a JSON object containing all items available in the store, matching items from Inventory service with data from Catalog service. |

You find it in this [book’s GitHub repository](book’s GitHub repository).

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
