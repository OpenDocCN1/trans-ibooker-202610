

![](img/3c6dc613426280ca3eafe43dea24391e_418_0.png)

**图 B.4** 隐式授权类型用于基于浏览器的单页应用（SPA）JavaScript 应用。

直接从用户的客户端（通常是 Web 浏览器）发起。在图 B.4 中，会发生以下活动。请注意，编号与图中的编号对应：

1. JavaScript 应用程序的所有者向 O-stock OAuth2 服务器注册应用程序。所有者提供一个应用程序名称和一个回调 URL，OAuth2 访问令牌将经由该回调 URL 重定向给用户。
2. JavaScript 应用程序调用 OAuth2 服务。JavaScript 应用程序必须提供预先注册的应用程序名称。然后，OAuth2 服务器强制用户进行身份验证。
3. 如果用户成功通过身份验证，O-stock OAuth2 服务不会返回令牌，而是将用户重定向回 JavaScript 应用程序所有者在步骤 1 中注册的页面。在重定向回用户的 URL 中，OAuth2 身份验证服务会将 OAuth2 访问令牌作为查询参数传递。
4. 应用程序接收传入的请求，并运行一个 JavaScript 脚本，该脚本解析 OAuth2 访问令牌并将其存储（通常作为 cookie 存储）。
5. 每次调用受保护资源时，OAuth2 访问令牌都会被提交给被调用的服务。
6. 被调用的服务验证 OAuth2 令牌，并检查用户是否有权执行其尝试执行的活动。

关于 OAuth2 隐式授权类型，需要记住几点。首先，隐式授权是唯一一种将 OAuth2 访问令牌直接暴露给公共客户端（Web 浏览器）的授权类型。

对于授权码授权类型，客户端应用程序接收由托管该应用程序的应用程序服务器返回的授权码，然后用户通过出示该授权码获得 OAuth2 访问令牌。返回的 OAuth2 令牌永远不会直接暴露给用户的浏览器。对于客户端凭证授权类型，授权发生在两个基于服务器的应用程序之间；而对于密码授权类型，请求服务的应用程序和服务都由同一组织拥有且受信任。

隐式授权类型生成的 OAuth2 令牌更容易受到攻击和滥用，因为这些令牌可供浏览器访问。浏览器中运行的任何恶意 JavaScript 都可以访问 OAuth2 访问令牌，并代表你调用你为其获取 OAuth2 令牌的服务，本质上是在冒充你。因此，隐式授权类型的 OAuth2 令牌应是短期有效的（1–2 小时）。由于 OAuth2 访问令牌存储在浏览器中，OAuth2 规范（以及 Spring Cloud Security）不支持刷新令牌的概念，即令牌可以自动续期。

## 令牌如何刷新

当 OAuth2 访问令牌签发后，它在有限时间内有效，最终会过期。当令牌过期时，调用应用程序（和用户）需要与 OAuth2 服务重新进行身份验证。但在大多数 OAuth2 授权流程中，OAuth2 服务器会同时签发访问令牌和刷新令牌。客户端可以将刷新令牌提交给 OAuth2 身份验证服务，该服务会验证刷新令牌，然后签发新的 OAuth2 访问令牌。我们来看图 B.5，并逐步了解令牌刷新流程。

在图 B.5 中，会发生以下操作。编号与图中的编号对应：

1. 用户登录 O-stock，此时已经通过 OAuth2 服务完成身份验证。用户正在愉快地工作，但不幸的是，他们的令牌过期了。
2. 下一次用户尝试调用服务（例如组织服务）时，O-stock 应用程序会将过期令牌传递给组织服务。
3. 组织服务尝试通过 OAuth2 服务验证令牌，OAuth2 服务返回 HTTP 状态码 401（未授权）和 JSON 负载，指示该令牌不再有效。组织服务向调用服务返回 HTTP 401 状态码。
4. O-stock 应用程序从组织服务收到 401 HTTP 状态码和 JSON 负载。然后，O-stock 应用程序使用刷新令牌调用 OAuth2 身份验证服务。OAuth2 身份验证服务验证刷新令牌，然后发回新的访问令牌。

## 附录 C：监控微服务

由于微服务是分布式且细粒度（小）的，它们为我们的应用程序引入了一种单体应用程序中不存在的复杂性。微服务架构需要高度的运维成熟度，而监控成为其管理的关键部分。如果研究服务监控，我们会发现大多数人都认为它是一个基本过程，但监控到底是什么？我们将监控定义为分析、收集和存储数据的过程，例如应用程序、平台和系统事件指标等，它帮助我们在 IT 环境中可视化故障模式。

只有故障吗？有必要澄清，故障是监控之所以基本的最明显原因之一，但不是唯一原因。微服务性能也被认为是另一个原因，并且确实在我们的应用中起着关键作用。我们不能将性能描述为“运行中”或“宕机”这样的二元概念。服务架构可以以某种降级状态运行，这可能会影响一个或多个服务的性能。在以下各节中，我们将向你展示如何使用 Spring Boot Actuator、Micrometer、Prometheus 和 Grafana 等技术来监控我们的 Spring Boot 微服务。那么，让我们开始吧。

### C.1 Spring Boot Actuator 监控简介

Spring Boot Actuator 是一个库，它以相当简单的方式为我们的 REST API 提供监控和管理工具。它通过组织和公开一系列 REST 端点来实现这一目的，这些端点允许我们访问不同的监控信息以检查我们的服务状态。换句话说，Spring Boot Actuator 提供了开箱即用的运维端点，帮助我们在了解和管理服务健康状况。

要使用 Spring Actuator，我们需要遵循两个简单步骤。第一步是在 pom.xml 文件中包含 Maven 依赖项，第二步是启用我们将在应用程序中使用的端点。让我们仔细看看这两个步骤。

#### C.1.1 添加 Spring Boot Actuator

要将 Spring Boot Actuator 包含到我们的微服务中，我们需要将以下依赖项添加到当前正在处理的微服务的 pom.xml 中：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

#### C.1.2 启用 Actuator 端点

设置 Spring Boot Actuator 是一个直接的过程。只需将依赖项添加到微服务中，我们就有了一系列可供使用的端点。每个端点都可以启用或禁用，并可以通过 HTTP 或 JMX 暴露。图 C.1 显示了默认启用的所有 Actuator 端点。要启用特定端点，我们只需使用以下格式的属性：

```properties
management.endpoint.<id>.enabled= true or false
```

例如，要禁用 beans 端点，我们需要添加以下属性：

```properties
management.endpoint.beans.enabled = false
```

在本书中，我们为所有微服务启用了所有默认的 Actuator 端点，但你可以根据需求自由进行更改。请记住，你始终可以通过使用 Spring Security 为 HTTP 端点添加安全性。以下代码显示了我们的许可服务和组织服务的 Spring Boot Actuator 配置：

```properties
management.endpoints.web.exposure.include=*
management.endpoints.enabled-by-default=true
```

使用此配置，如果我们在 Postman 中输入 http://localhost:8080/actuator 端点，我们将看到一个类似图 C.1 所示的列表，其中包含 Spring Boot Actuator 暴露的所有端点。

