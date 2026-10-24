

API Gateway 微服务和登录微服务是实现技术能力的服务的示例。此外，两者都是在许多不同系统中非常相似的技术能力：在领域驱动设计术语中，它们是通用子域的示例——这意味着它们是使用现成产品实现的理想候选者。登录微服务可以由任何身份提供者实现，包括 Azure AD、AWS Cognito、Duende Identity Server 和 Auth0 等产品。同样，API Gateway 可以由 Azure API Management、AWS API Gateway、Traefik 和 Istio 实现。

## 10.1.2 在微服务中授权用户

我们已经确定身份验证是在系统边缘启动的——由 API Gateway 启动。请求经过身份验证后，你就知道用户是谁，但你不知道用户是否被允许发出该请求。那是授权的问题。用户只能更新自己的兴趣。如果客户端代表用户 A 发送请求，试图更新用户 B 的兴趣，系统应拒绝该请求。

在微服务系统中，启动身份验证的边缘微服务——通常不是执行请求所针对的操作的微服务。图 10.4 再次展示了忠诚度计划。API Gateway 微服务接收来自客户端的请求，但忠诚度计划微服务负责跟踪用户兴趣。因此，忠诚度计划更新用户的兴趣。

![](img/608267d7803dc57a1a896ad121f37868_255_0.png)

图 10.4 授权应作为业务逻辑的一部分完成；忠诚度计划微服务负责授权对用户设置的更新。

请记住第 4 章中提到的内容，微服务主要按业务能力进行划分——一个业务能力由一个微服务处理。这包括授权，因为授权是该能力业务规则的一部分。这与让忠诚度计划微服务决定用户是否被允许更新兴趣是一致的。为此，用户身份需要在微服务之间传递。在忠诚度计划中，API Gateway 微服务必须在发送给忠诚度计划的请求中包含用户身份。用户身份可以通过多种方式包含在发送给忠诚度计划的请求中，具体取决于你希望 API Gateway 与其他微服务之间信任的程度。如果信任度低，用户身份可以以签名身份令牌的形式传递，忠诚度计划可以验证该令牌；如果信任度更高——用户身份可以以明文形式通过自定义 HTTP 头传递。在这两种情况下，API Gateway 对用户进行身份验证，忠诚度计划根据用户身份对用户进行授权。

## 10.2 实现安全的微服务到微服务通信

在本章的剩余部分，我们将研究围绕忠诚度计划实现安全性的示例。安全需求如下：

- 1. 在 API Gateway 中对用户进行身份验证。
- 2. 允许微服务访问用户身份。
- 3. 限制哪些微服务可以协作。

前两个需求可以通过 API Gateway 来满足。我不会详细介绍这一点，只提到你可以使用 Azure API Management 来实现这一点。第三个需求可以使用 Kubernetes 网络策略来满足，我们接下来将通过示例讨论这一点。

### 10.2.1 在忠诚度计划中访问用户身份

在忠诚度计划微服务中，我们假设 API Gateway 已配置为执行用户身份验证，并且用户身份以身份令牌的形式从 API Gateway 传递到忠诚度计划微服务，如图 10.5 所示

![](img/608267d7803dc57a1a896ad121f37868_257_0.png)

图 10.5 在 Azure API Management 中实现的 API Gateway 对用户进行身份验证，并将身份令牌传递给 Kubernetes 中的忠诚度计划微服务。

在以下各节中，你将学习如何在忠诚度计划中使用身份令牌，通过

- 使用 ASP.NET 身份验证中间件验证令牌
- 在 MVC 控制器中授权

第一步是添加身份验证中间件。我们通过将身份验证添加到服务集合，在 Startup 类中执行此操作。我只在这里概述解决方案，因为细节取决于确切的 API Gateway 配置，这超出了本章的范围。

在忠诚度计划中，我们可以将 Startup 中的 ConfigureServices 方法更改为如下所示。

### Listing 10.1 Configuration to use JWT bearer tokens by default

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
        .AddJwtBearer(x => x.TokenValidationParameters =
            new TokenValidationParameters
            {
                ValidAudience = ...,
                ValidIssuer = ...,
                IssuerSigningKey = ...,
            });
    services.AddControllers();
}
```

这概述了如何添加中间件，从传入请求的 Authorization 头中读取和验证 JWT。这与配置为在 Authorization 头中以 JWT 形式传递用户身份的 API Gateway 相符。

下一步是通过在 Startup 类中将授权添加到管道，使用户身份可供控制器使用，如下所示。

### Listing 10.2 Using authorization middleware to make identities available to controllers

```
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    app.UseHttpsRedirection();
    app.UseRouting();
    app.UseAuthorization();
    app.UseEndpoints(endpoints => endpoints.MapControllers());
}
```

这意味着控制器现在可以使用 Authorize 属性要求请求中存在身份令牌，并可以通过 ControllerBase 上的 User 属性访问用户身份。例如，为了保护用于更新现有用户的 PUT 端点，我们可以在 UsersController 中的 UpdateUser 方法上添加 Authorize 属性：

```
[HttpPut("{userId:int}")]
[Authorize(AuthenticationSchemes =
    JwtBearerDefaults.AuthenticationScheme)]
public LoyaltyProgramUser UpdateUser(
    int userId,
    [FromBody] LoyaltyProgramUser user) =>
    return RegisteredUsers[userId] = user;
```

这确保了所有对更新用户的 PUT 端点的请求都有有效的 JWT 令牌。但我们仍然不能确定是否更新了正确的用户，因为简单地使用作为路径一部分传递的用户 ID；如果此用户 ID 与 JWT 中的用户 ID 不同，一个用户就可以更新另一个用户。为了防止这种情况，我们必须

将其与 JWT 中的用户 ID 进行比较。JWT 中的值——或声明——可通过 ControllerBase 类上的 User 属性获得，UsersController 从该类继承。User 属性是一个 ClaimsPrincipal 对象，它有一个包含来自 JWT 的所有声明的声明集合。利用这一知识，我们可以确保用户只能授权更新自己的信息。

### Listing 10.3 Inspecting the claims on the user

```
[HttpPut("{userId:int}")]
[Authorize(AuthenticationSchemes = JwtBearerDefaults.AuthenticationScheme)]
public ActionResult<LoyaltyProgramUser> UpdateUser(
    int userId,
    [FromBody] LoyaltyProgramUser user)
{
    var hasUserId = int.TryParse(
        this.User.Claims.FirstOrDefault(c => c.Type == "userid")?.Value,
        out var userIdFromToken);
    if (!hasUserId || userId != userIdFromToken)
        return Unauthorized();
    return RegisteredUsers[userId] = user;
}
```

在声明集合中找到用户 ID。

将用户 ID 解析为 userIdFromToken。

检查令牌中的用户 ID 是否与路径中的用户 ID 匹配。

如果用户 ID 不匹配，则返回未经授权的响应。

这完成了忠诚度计划中 PUT 端点授权的实现。我们选择了一种解决方案，即 API Gateway 将带有用户身份的 JWT 传递给忠诚度计划微服务。如果信任级别更高，我们可能会选择以明文形式将身份从 API Gateway 发送到忠诚度计划。

有了授权后，我们现在将注意力转向限制哪些微服务被允许通信。

### 10.2.2 限制哪些微服务可以通信

请回顾图 10.1，忠诚度计划的协作者之一是特别优惠微服务。图 10.6 放大了这种协作。

![](img/608267d7803dc57a1a896ad121f37868_259_0.png)

图 10.6 忠诚度计划具有 API 和事件消费者；事件消费者使用特别优惠中的事件端点获取事件。

