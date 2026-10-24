

```
this.httpClient = new HttpClient();
}

public async Task<HealthCheckResult> CheckHealthAsync(
    HealthCheckContext context,
    CancellationToken cancellationToken)
{
    await using var conn = new SqlConnection(...);
    var result = await conn.QuerySingleAsync<int>("SELECT 1");
    return result == 1
        ? HealthCheckResult.Healthy()
        : HealthCheckResult.Degraded();
}
}
```

请注意，`MsSqlStartupHealthCheck` 实现了 `IHealthChecks`，这意味着它可以很好地融入之前创建的 `AddAdditionStartupHealthChecks`。微服务可以使用 `MicroserviceNET.Monitoring` 包和 `MsSqlStartupHealthCheck`。

## 清单 11.6 使用微服务平台注册健康检查

```
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services
            .AddBasicHealthChecks()
            .AddAdditionStartupHealthChecks<MsSqlStartupHealthCheck>();
    }
}
```

微服务可以通过利用所创建的应用构建器扩展，在标准化的监控端点中公开这些检查。

## 清单 11.7 添加标准化的监控端点

```
public void Configure(IApplicationBuilder app)
{
    app.UseRouting();
    app.UseKubernetesHealthChecks();
    app.UseEndpoints(endpoints => endpoints.MapControllers());
}
```

我们已经创建了微服务平台的第一版，它有助于标准化我们的微服务日志、追踪和暴露监控端点。要使用该平台，微服务需要安装两个 NuGet 包——`MicroserviceNET.Logging` 和 `MicroserviceNET.Monitoring`——并在 `Program.cs` 中使用宿主构建器扩展来设置日志记录，在 `Startup` 类中使用服务集合和应用构建器上的扩展来设置监控端点。

在开发微服务系统的过程中，你很可能会发现更多适合在平台中实现的横切关注点。其中一些可以很好地融入 ASP.NET 的中间件管道中，这正是下一节的主题。

## 11.5 ASP.NET 管道

我们在第 1 章中简要提到了中间件，但现在我们该仔细看看了。ASP.NET 允许我们放入一些中间件，这些中间件会在每个请求时执行。每个请求依次流经每一部分中间件。这些中间件一起构成了请求流经的管道。

图 11.6 中的设置是一个包含一段中间件的管道。ASP.NET 定义了管道各部分之间通信所用的接口。这是一个所有管道部分都实现的统一接口：每一段中间件都实现该接口。通过拥有一个统一的接口，你可以按需组合管道。你可以向其中添加更多中间件，可以移除部分中间件，也可以调换它们的顺序。因为它们都使用相同的接口，所以可以根据需要重新排列。

![](img/608267d7803dc57a1a896ad121f37868_276_0.png)

图 11.6 管道由中间件构建而成，这些中间件通过统一的标准化接口进行通信。

当请求和响应通过中间件时，中间件可以读取它们甚至修改它们。中间件之间使用的接口是 ASP.NET 定义的 `RequestDelegate`：

```
public delegate Task RequestDelegate(HttpContext context);
```

这个委托允许中间件检查和修改 `HttpContext`，并且只要中间件知道下一段中间件，就可以将其传递给管道中的下一段中间件，正如 11.3 和 11.6 所示。

`RequestDelegate` 是一种委托类型，我们可以选择用 lambda 函数来实现它，这是一种实现中间件的轻量级方式。不过有一个小复杂点：中间件应该是中间件管道的一部分，因此它需要知道管道中的下一段中间件。我们用一个小间接层来解决：创建一个 lambda，它接收下一段中间件，并返回一个实现 `RequestDelegate` 的 lambda。让我们看一下代码：

```
next =>
  ctx =>
  {
```

下一段中间件

内部 lambda 就是中间件。

```
System.Console.WriteLine("Got request");
return next(ctx);
}
```

让我们分解这段代码。首先，`next` 的类型是 `RequestDelegate`，并且应当是管道中的下一段中间件。其次，`ctx` 的类型是 `HttpContext`。这意味着内部 lambda 接收一个 `HttpContext` 作为参数，并返回一个 `Task`：

```
ctx =>
{
    System.Console.WriteLine("Got request");
    return next(ctx);
}
```

`ctx` 的类型是 `HttpContext`。

`next` 是一个 `RequestDelegate`，因此它返回一个 `Task`。

既然我们已经确定了内部 lambda 实现了 `RequestDelegate`，我们就可以看到外部 lambda 如何用于构建管道。用一段中间件调用外部 lambda，会创建一个包含两段中间件的管道：先是内部 lambda 中的中间件，然后是传入的中间件：

```
RequestDelegate secondMiddleware = ...
var fisrstMiddleware = next =>
    ctx =>
    {
        System.Console.WriteLine("Got request");
        return next(ctx);
    }

RequestDelegate pipeline = firstMiddleware(secondMiddleware);
```

前面清单中的中间件

来自某处的中间件

管道也实现了 `RequestDelegate`。

管道本身也是一个 `RequestDelegate`，这一事实使得构建更长的管道以及组合管道成为可能。在启动期间，ASP.NET 会构建管道，以便在 HTTP 请求到来时准备好处理它们。

`HttpContext` 包含与 HTTP 请求和响应相关的所有内容。在请求侧，这包括所有请求头、请求路径和请求体。同样，在响应侧，`HttpContext` 包括响应头、状态码和响应体。有了所有这些访问权限，中间件可以对 HTTP 请求和响应做任何事情。

`RequestDelegate` 是一个简单的类型，但它实现了一个强大的概念。中间件管道既灵活又强大，我觉得它非常有用。接下来，我将讨论何时使用 ASP.NET 中间件，以及何时使用 MVC 的功能。

### 11.5.1 什么属于中间件？

ASP.NET 为你提供了一种构建中间件管道的方法，每个请求都会流经该管道。管道的不同部分可以响应请求并向响应写入内容。同样，MVC 让你可以处理请求并编写响应。使用 MVC，你主要在控制器的方法中处理请求，但也可以添加在控制器方法之前和之后执行的代码。你可以通过向 MVC 配置添加 MVC 过滤器来实现这一点。但什么该放在哪里？

为了决定在中间件中实现什么，以及在 MVC 过滤器中实现什么，你可以遵循几条简单的经验法则：

- * 处理横切关注点且旨在跨多个微服务使用的代码属于中间件。这些可重用的代码片段应该具有较少的依赖关系，这样它们就不会对使用它们的微服务强制施加太多技术选择。因此，你希望它们独立于 MVC。
- * 处理单个微服务的领域或业务规则的代码属于 MVC 控制器后面的应用程序代码。这类代码不依赖于 HTTP，因此不需要依赖 ASP.NET 或 MVC。将它放在单独组件中——例如领域模型——可以使业务和领域规则的实现保持干净和可读。
- * 以特定端点特有的方式处理 HTTP 请求和响应的代码属于 MVC 控制器。这段代码解释传入的 HTTP 请求，然后将控制权移交给领域模型。同时，这种解释与 HTTP 和端点的细节紧密耦合，使得控制器成为它的合适位置。
- * 处理横切某个微服务内所有端点、但不横切多个微服务的关注点的代码，通常属于中间件，但并不总是如此。关注点越技术性，我越倾向于中间件；关注点中的领域或业务逻辑越多，我越倾向于 MVC 控制器，或者在极少数情况下，使用 MVC 过滤器。

我已经介绍了中间件并讨论了它的用途。现在我们转向编写和测试中间件。

## 11.6 编写中间件

我们将介绍两种编写中间件并将其挂接到 ASP.NET 管道中的方法：

- * 作为 lambda——你已经在前面章节中看到了这种风格，我们将在下一节中在此基础上继续构建
- * 作为具有实现中间件签名的方法的类——在我们再次查看 lambda 风格之后，你将看到这种风格。

这两种风格是等价的，你可以很容易地在它们之间切换。为某段特定中间件选择哪一种，只是品味问题。

### 11.6.1 作为 lambda 的中间件

你已经看到了用 lambda 函数实现的中间件。在本节中，我将向你展示如何在 `Startup.cs` 文件中将基于 lambda 的中间件添加到 ASP.NET 管道中。

