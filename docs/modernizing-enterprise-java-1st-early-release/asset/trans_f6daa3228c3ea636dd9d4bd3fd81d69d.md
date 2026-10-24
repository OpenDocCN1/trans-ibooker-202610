

```java
package com.redhat.cloudnative.catalog;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class CatalogApplication {

    public static void main(String[] args) {
        SpringApplication.run(CatalogApplication.class, args);
    }
}
```

① 这是一个便捷注解，它添加了自动配置、组件扫描，并允许定义额外配置。它等同于使用默认属性的 `@Configuration`、`@EnableAutoConfiguration` 和 `@ComponentScan`。

## 创建领域模型

接下来，我们需要为我们的微服务提供一些数据，这些数据代表我们 Coolstore 电商网站的目录。同样，这里我们定义一个领域模型，用于与持久层进行高层交互，并定义一个接口，使 REST 端点能够与服务和数据模型之间进行通信（如图 2-6 所示）。

![](img/8e80afd9c3d238f2221423409b47c62a_52_0.png)

数据库使用 Spring 应用程序配置文件进行配置，该文件位于属性文件 `application.properties` 中。让我们看一下这个文件，了解数据库连接详情。

您可以在本书的 [GitHub 仓库](#) 中找到该文件。

```properties
spring.application.name=catalog
server.port=8080
spring.datasource.url=jdbc:h2:mem:catalog;DB_CLOSE_ON_EXIT=FALSE
spring.datasource.username=sa
spring.datasource.password=
spring.datasource.driver-class-name=org.h2.Driver
```

- 1. H2 数据库的 JDBC URL
- 2. 使用 H2 内存数据库

让我们创建领域模型，这与之前为 Inventory 微服务创建的模型类似。

您可以在本书的 [GitHub 仓库](#) 中找到该文件。

```java
package com.redhat.cloudnative.catalog;

import java.io.Serializable;

import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity ①
@Table(name = "PRODUCT") ②
public class Product implements Serializable {

    private static final long serialVersionUID = 1L;

    @Id ③
    private String itemId;

    private String name;

    private String description;

    private double price;

    public Product() {
    }

    public String getItemId() {
        return itemId;
    }

    public void setItemId(String itemId) {
        this.itemId = itemId;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }

    @Override
    public String toString() {
        return "Product [itemId=" + itemId + ", name=" + name + ", price=" + price + "]";
    }
}
```

- 1. `@Entity` 将该类标记为 JPA 实体
- 2. `@Table` 通过定义表名和数据库约束来定制建表过程，此处表名为 *CATALOG*
- 3. `@Id` 标记表的主键

## 创建数据仓库

Spring Data 仓库抽象通过减少为各种持久化存储实现数据访问层所需的样板代码数量，简化了在 Spring 应用程序中处理数据模型的方式。`Repository` 及其子接口是 Spring Data 的核心概念，它是一个标记接口，为被管理的实体类提供数据操作功能。当应用程序启动时，Spring 会查找所有标记为仓库的接口，并为找到的每个接口配置所需的持久化技术，同时为该仓库接口提供实现。

我们现在在 *com.redhat.cloudnative.catalog* 包中创建一个名为 *ProductRepository* 的新 Java 接口，并扩展 `CrudRepository` 接口，以向 Spring 表明我们希望暴露一整套操作实体的方法。

您可以在本书的 [GitHub 仓库](...) 中找到该文件。

```java
package com.redhat.cloudnative.catalog;

import org.springframework.data.repository.CrudRepository;

public interface ProductRepository extends CrudRepository<Product, String> { }
```

> ① [CrudRepository](...)：用于向 Spring 表明我们希望暴露一整套操作实体的方法的接口。

## 创建 RESTful 服务

Spring Boot 在 Spring 应用程序中默认使用 Spring Web MVC 作为 RESTful 技术栈。我们现在在 *com.redhat.cloudnative.catalog* 包中创建一个名为 *CatalogController* 的新 Java 类，并暴露一个符合 [表 2-4](...) 定义的 REST 端点。

### 表 2-4. Catalog REST API 端点

| 路径 | HTTP 方法 | 描述 |
|---|---|---|
| /api/catalog/ | GET | 返回商店中所有可用商品的目录，将 Inventory 服务中的商品与 Catalog 服务中的数据进行匹配。 |

您可以在本书的 [GitHub 仓库](book's GitHub repository) 中找到该文件。

```java
package com.redhat.cloudnative.catalog;

import java.util.List;
import java.util.Spliterator;
import java.util.stream.Collectors;
import java.util.stream.StreamSupport;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.MediaType;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping(value = "/api/catalog") ①
public class CatalogController {

    @Autowired ②
    private ProductRepository repository; ③

    @ResponseBody
    @GetMapping(produces = MediaType.APPLICATION_JSON_VALUE)
    public List<Product> getAll() {
        Spliterator<Product> products = repository.findAll().spliterator();
        return StreamSupport.stream(products, false).collect(Collectors.toList());
    }
}
```

① `@RequestMapping` 表明上述 REST 服务定义了一个可通过 HTTP GET 在 `/api/catalog` 访问的端点

② Spring Boot 在运行时自动为 *ProductRepository* 提供实现，并使用 [@Autowired](https://...) 注解将其注入控制器

③ 控制器类上的 `repository` 属性用于从数据库检索产品列表

现在一切已就绪，可以启动我们的第二个微服务，它将监听 9000 端口以避免与另一个微服务冲突。

```bash
mvn spring-boot:run
```

您应该会看到类似如下的输出：

```text
[INFO] --- spring-boot-maven-plugin:2.1.4.RELEASE-redhat-00001:run (default-cli) @ catalog ---
[INFO] Attaching agents: []
2020-12-02 17:12:18.528  INFO 61053 --- [           main] trationDelegate$BeanPostProcessorChecker : Bean 'org.springframework.cloud.autoconfigure.ConfigurationPropertiesRebinderAutoConfiguration' of type [org.springframework.cloud.autoconfigure.ConfigurationPropertiesRebinderAutoConfiguration$$EnhancerBySpringCGLIB$$e898759c] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)

  ____             _                 ____              _
 /\\ / ___'_ __ _ _(_)_ __  __ _    / ___|  ___   ___ | |_
( ( )\___ | '_ | '_| | '_ \/ _` |   \___ \ / _ \ / _ \| | |
 \\/  ___)| |_)| | | | | || (_| |    ___) | (_) | (_) | | |
  '  |____| .__|_| |_|_| |_\__, |   |____/ \___/ \___/|_| |_|
 =========|_|==============|___/================================
 :: Spring Boot ::        (v2.1.6.RELEASE)

StandardService : Starting service [Tomcat]
2020-12-02 17:12:20.064  INFO 61053 --- [           main] org.apache.catalina.core.StandardEngine : Starting Servlet Engine: Apache Tomcat/9.0.7.redhat-16
2020-12-02 17:12:20.220  INFO 61053 --- [           main] o.a.c.c.C.[Tomcat].[localhost].[/] : Initializing Spring embedded WebApplicationContext
```

