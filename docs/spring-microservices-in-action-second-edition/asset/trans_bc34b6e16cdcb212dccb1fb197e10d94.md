

## 3.2.2 服务器或容器蔓延

微服务最常见的部署模型之一是将一个微服务实例部署在一个容器中。在大型基于微服务的应用中，最终可能会有50到100个服务器或容器（通常是虚拟的）需要单独在生产环境中构建和维护。即使在云端运行这些服务的成本较低，管理和监控这些服务的运维复杂性也可能是巨大的。

> 注意：微服务的灵活性必须与运行所有这些服务器的成本进行权衡。你也可以有其他选择，例如考虑像lambda这样的功能开发，或者在同一台服务器上添加更多的微服务实例。

## 3.2.3 应用类型

微服务面向可重用性，对于构建需要高度弹性和可扩展性的大型应用非常有用。这也是许多基于云的公司采用微服务的原因之一。如果你正在构建小型的、部门级应用，或者用户群较小的应用，那么构建像微服务这样的分布式模型所带来的复杂性可能会产生超过其价值的费用。

## 3.2.4 数据事务与一致性

当你开始考虑微服务时，需要仔细思考服务和服务消费者的数据使用模式。一个微服务封装并抽象了少量表，非常适合执行“操作性”任务，如对数据存储进行创建、添加以及执行简单（非复杂）查询。

如果你的应用需要在多个数据源之间进行复杂的数据聚合或转换，微服务的分布式特性将使这项工作变得困难。你的微服务将不可避免地承担过多责任，也可能容易出现性能问题。

## 3.3 开发者的故事：使用Spring Boot和Java构建微服务

在本节中，我们将探讨开发者在从O-stock领域模型构建许可微服务时的优先事项。

> 注意：我们在上一章中创建了许可服务的骨架。如果你没有按照那一章的代码清单操作，可以从 https://github.com/ihuaylupo/manning-smia/tree/master/chapter2 下载源代码。

在接下来的几节中，我们将：

- 1. 实现一个Spring Boot控制器类，用于映射端点以暴露许可服务的端点
- 2. 实现国际化，使消息可以适应不同的语言
- 3. 实现Spring HATEOAS，以提供足够的信息，使用户能够与服务器交互

### 3.3.1 构建微服务的入口：Spring Boot控制器

现在我们已经解决了构建脚本（参见第2章）并实现了一个简单的Spring Boot引导类，你可以编写第一个具有实际功能的代码了。这个代码就是你的控制器类。在Spring Boot应用中，控制器类暴露服务端点，并将传入的HTTP请求中的数据映射到处理请求的Java方法上。

#### 赋予它REST风格

本书中的所有微服务都遵循Richardson成熟度模型（http://mng.bz/JD5Z）。你将构建的所有服务都具有以下特征：

- - 使用HTTP/HTTPS作为服务的调用协议——HTTP端点暴露服务，HTTP协议携带进出服务的数据。
- - 将服务的行为映射到标准的HTTP动词——REST强调让服务将其行为映射到HTTP动词POST、GET、PUT和DELETE。这些动词映射到大多数服务中的CRUD功能。
- - 使用JSON作为进出服务的所有数据的序列化格式——这不是基于REST的微服务的硬性规定，但JSON已成为微服务提交和返回数据序列化的“通用语言”。你也可以使用XML，但许多基于REST的应用都使用JavaScript和JSON。JSON是基于JavaScript的Web前端和服务所消费数据进行序列化和反序列化的原生格式。
- - 使用HTTP状态码来传达服务调用的状态——HTTP协议使用丰富的状态码来表示服务的成功或失败。基于REST的服务利用这些HTTP状态码和其他基于Web的基础设施，如反向代理和缓存。这些可以相对容易地与你的微服务集成。

HTTP是Web的语言。使用HTTP作为构建服务的哲学框架是在云端构建服务的关键。

你可以在 `src/main/java/com/optimagrowth/license/controller/LicenseController.java` 中找到你的第一个控制器类。`LicenseController` 类暴露了四个HTTP端点，它们将映射到POST、GET、PUT和DELETE动词。

让我们浏览这个控制器类，看看Spring Boot如何提供一组注解，帮助你将暴露服务端点所需的工作量降至最低，并让你专注于构建服务的业务逻辑。我们将从查看基本的控制器类定义开始，此时还没有任何类方法。下面的清单显示了O-stock许可服务的控制器类。

### 清单 3.1 将 `LicenseServiceController` 标记为 Spring `RestController`

```java
package com.optimagrowth.license.controller;

import java.util.Random;

import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

import com.optimagrowth.license.model.License;

@RestController
@RequestMapping(value="/v1/organization/{organizationId}/license")
public class LicenseController {
}
```

![](img/3c6dc613426280ca3eafe43dea24391e_94_0.png)

我们将从 `@RestController` 注解开始探索。`@RestController` 是一个类级别的Java注解，告诉Spring容器这个Java类将用于基于REST的服务。这个注解会自动处理传入服务的数据的序列化，可以是JSON或XML（默认情况下，该类将返回的数据序列化为JSON）。与传统的Spring `@Controller` 注解不同，`@RestController` 不要求你从控制器类的方法中返回ResponseBody类。所有这些都由 `@RestController` 注解的存在来处理，该注解包含 `@ResponseBody` 注解。

> **为什么微服务使用JSON？**
> 
> 我们可以使用多种协议在基于HTTP的微服务之间发送和接收数据。但JSON已成为事实上的标准，原因如下：
> 
> - 与其他协议如基于XML的SOAP（简单对象访问协议）相比，JSON非常轻量。你可以在没有太多文本开销的情况下表达数据。

### 清单 3.2 探索许可模型

```java
package com.optimagrowth.license.model;

import lombok.Getter;
import lombok.Setter;
import lombok.ToString;

@Getter @Setter @ToString
public class License {
    private int id;
    private String licenseId;
    private String description;
    private String organizationId;
    private String productName;
    private String licenseType;
}
```

一个包含许可信息的普通Java对象（POJO）。

### Lombok

Lombok是一个小型库，允许我们减少项目中Java类中编写的样板Java代码量。Lombok会生成诸如getter和setter、字符串方法（如`toString`）、构造函数等代码。

在本书中，我们将在所有代码示例中使用Lombok，以使代码更具可读性，但不会深入探讨如何使用它。如果你有兴趣了解更多关于Lombok的信息，我们强烈推荐你阅读以下Baeldung.com文章：

- - https://www.baeldung.com/intro-to-project-lombok
- - https://www.baeldung.com/lombok-ide

如果你希望在Spring Tools Suite 4上安装Lombok，你必须下载并执行Lombok，并将其链接到IDE。

下面的清单显示了我们将使用的服务类，用于开发我们将在控制器类中创建的不同服务的逻辑。

### 清单 3.3 探索 LicenseService 类

```java
package com.optimagrowth.license.service;

import java.util.Random;

import org.springframework.stereotype.Service;
import org.springframework.util.StringUtils;

import com.optimagrowth.license.model.License;
```

