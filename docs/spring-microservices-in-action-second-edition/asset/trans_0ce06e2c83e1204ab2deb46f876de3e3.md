

使用 `FilterUtils.setCorrelationId()` 方法时，当我们想向 HTTP 请求头中添加一个值，可以使用 `ServerWebExchange.Builder.mutate()` 方法。此方法返回一个构建器，通过 `ServerWebExchangeDecorator` 包装交换对象来修改其属性，并返回被修改的值或将其委派回此实例。要测试此调用，我们可以调用我们的组织或许可服务。一旦调用提交，我们应该在控制台中看到一条日志消息，记录传入的 correlation ID 随着过滤器的流动：

```text
gatewayserver_1      | 2020-04-14 22:31:23.835 DEBUG 1 --- [or-http-epoll-3]
c.o.gateway.filters.TracingFilter       : tmx-correlation-id generated in
tracking filter: 735d8a31-b4d1-4c13-816d-c31db20afb6a.
```

如果在控制台上看不到该消息，只需将以下清单中显示的代码行添加到网关服务器的 `bootstrap.yml` 配置文件中。然后重新构建并执行你的微服务。

**Listing 8.11  Logger configuration in the Gateway service bootstrap.yml file**

```yaml
//Some code removed for conciseness
logging:
  level:
    com.netflix: WARN
    org.springframework.web: WARN
    com.optimagrowth: DEBUG
```

如果使用 Docker，可以在父 `pom.xml` 所在的根目录中执行以下命令：

```bash
mvn clean package dockerfile:build
docker-compose -f docker/docker-compose.yml up
```

## 8.6  Using the correlation ID in the services

现在我们已经确保 correlation ID 已添加到流经网关的每个微服务调用中，我们希望确保

- Correlation ID 可供被调用的微服务轻松访问。
- 微服务可能进行的任何下游服务调用也会将 correlation ID 传播到下游调用。

为此，我们将为每个微服务构建三类类：`UserContextFilter`、`UserContext` 和 `UserContextInterceptor`。这些类将协同工作以读取传入 HTTP 请求的 correlation ID（以及我们稍后添加的其他信息），将其映射到可被应用程序业务逻辑轻松访问和使用的类，然后确保 correlation ID 被传播到任何下游服务调用。图 8.12 演示了我们将如何为许可服务构建这些不同组件。

![](img/3c6dc613426280ca3eafe43dea24391e_261_0.png)

Figure 8.12 Using a set of common classes so that the correlation ID can be propagated to downstream service calls

让我们逐步了解图 8.12 中发生的情况：

- 1. 当通过网关向许可服务发出调用时，Tracking-Filter 会为进入网关的任何调用在传入的 HTTP 头中注入一个 correlation ID。
- 2. `UserContextFilter` 类，一个自定义 HTTP ServletFilter，将 correlation ID 映射到 `UserContext` 类。`UserContext` 类将值存储在线程中以供调用后期使用。

### 8.6.1 UserContextFilter

```java
throws IOException, ServletException {

HttpServletRequest httpServletRequest = (HttpServletRequest)
										servletRequest;

UserContextHolder.getContext()
	.setCorrelationId(
		httpServletRequest.getHeader(UserContext.CORRELATION_ID));
UserContextHolder.getContext().setUserId(
	httpServletRequest.getHeader(UserContext.USER_ID));
UserContextHolder.getContext().setAuthToken(
	httpServletRequest.getHeader(UserContext.AUTH_TOKEN));
UserContextHolder.getContext().setOrganizationId(
	httpServletRequest.getHeader(UserContext.ORGANIZATION_ID));

logger.debug("UserContextFilter Correlation id: {}",
	UserContextHolder.getContext().getCorrelationId());

filterChain.doFilter(httpServletRequest, servletResponse);
}
// Not showing the empty init and destroy methods
}
```

Retrieves the correlation ID from the header and sets the value in the `UserContext` class

最终，`UserContextFilter` 将把你感兴趣的 HTTP 头值映射到 Java `UserContext` 类。

### 8.6.2 UserContext: Making the HTTP headers easily accessible to the service

`UserContext` 类保存由我们的微服务处理的单个服务客户端请求的 HTTP 头值。它由 getter/setter 方法组成，用于从 `java.lang.ThreadLocal` 检索和存储值。以下清单显示了 `UserContext` 类的代码，你可以在 `/licensing-service/src/main/java/com/optimagrowth/license/utils/UserContext.java` 中找到它。

**Listing 8.13 Storing the HTTP header values inside the UserContext class**

```java
//Removed the imports for conciseness
@Component
public class UserContext {
    public static final String CORRELATION_ID = "tmx-correlation-id";
    public static final String AUTH_TOKEN     = "tmx-auth-token";
    public static final String USER_ID        = "tmx-user-id";
    public static final String ORGANIZATION_ID = "tmx-organization-id";

    private String correlationId = new String();
    private String authToken = new String();
    private String userId = new String();
    private String organizationId = new String();
}
```

### 8.6.3 UserContextInterceptor

```java
public ClientHttpResponse intercept(
        HttpRequest request, byte[] body,
        ClientHttpRequestExecution execution) throws IOException {
    HttpHeaders headers = request.getHeaders();
    headers.add(UserContext.CORRELATION_ID,
            UserContextHolder.getContext().
            getCorrelationId());
    headers.add(UserContext.AUTH_TOKEN,
            UserContextHolder.getContext().
            getAuthToken());

    return execution.execute(request, body);
}
```

![](img/3c6dc613426280ca3eafe43dea24391e_265_0.png)

要使用 `UserContextInterceptor`，我们需要定义一个 `RestTemplate` bean，然后向其添加 `UserContextInterceptor`。为此，我们将在 `LicenseServiceApplication` 类中定义自己的 `RestTemplate` bean。你可以在 `/licensing-service/src/main/java/com/optimagrowth/license/` 中找到此类的源代码。以下清单显示了添加到此 `RestTemplate` 的方法。

**Listing 8.16  Adding the UserContextInterceptor to the RestTemplate class**

```java
@LoadBalanced
@Bean
public RestTemplate getRestTemplate(){
    RestTemplate template = new RestTemplate();
    List interceptors = template.getInterceptors();
    if (interceptors==null){
        template.setInterceptors(Collections.singletonList(
                new UserContextInterceptor()));
    }else{
        interceptors.add(new UserContextInterceptor());
        template.setInterceptors(interceptors);
    }

    return template;
}
```

![](img/3c6dc613426280ca3eafe43dea24391e_265_1.png)

有了此 bean 定义，每当我们使用 `@Autowired` 注解并将 `RestTemplate` 注入到一个类中时，我们将使用清单 8.16 中创建的、附加了 `UserContextInterceptor` 的 `RestTemplate`。

## Log aggregation, authentication, and more

现在我们已经将 correlation ID 传递给每个服务，可以追踪事务在其流经调用所涉及的所有服务中的过程。为此，你需要确保每个服务都记录到一个中央日志聚合点，该点将来自所有服务的日志条目捕获到一个单一位置。日志聚合服务中捕获的每个日志条目都将与一个 correlation ID 相关联。

一旦我们实现了 ResponseFilter，我们就可以启动我们的服务并使用它调用许可或组织服务。服务完成后，你将在调用的 HTTP 响应头中看到一个 `tmx-correlation-id`，如图 8.13 所示。

![](img/3c6dc613426280ca3eafe43dea24391e_267_0.png)

Figure 8.13 The tmx-correlation-id added to the response headers and sent back to the service client

你也可以在控制台中看到日志消息（图 8.14），其中记录了传入的 correlation ID，`e3f6a72b-7d6c-41da-ac12-fb30fcd1e547`，它在 pre- 和 post- 过滤器中流动。

到目前为止，我们所有的过滤器示例都处理在服务客户端调用被路由到目标目的地之前和之后对其进行操作。现在我们知道如何创建 Spring Cloud Gateway，让我们继续下一章，该章描述了如何使用 Keycloak 和 OAuth2 来保护我们的微服务。

![](img/3c6dc613426280ca3eafe43dea24391e_267_1.png)

Figure 8.14 Logger output that shows the pre-filter data, the organization service processing data, and the post-filter data

