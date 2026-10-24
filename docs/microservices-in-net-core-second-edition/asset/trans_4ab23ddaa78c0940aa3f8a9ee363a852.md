

```csharp
private bool ShouldSendNotification(dynamic eventData)
{
    // decide if notification should be sent based on business rules
}

private Task SendNotification(dynamic eventData)
{
    // use HttpClient to send command to notification microservice
}
```

事件处理代码的具体实现会因具体情况而异——这正是业务逻辑被实现的地方。

有了这些，一个批次就可以被处理了。最后一步是配置 Kubernetes 以每小时一次的频率调用程序，这通过以下 YAML 完成，该 YAML 在 Kubernetes 中定义了一个 CronJob，每小时调用一次我们的事件消费者。

## Listing 7.7  Kubernetes manifest for the LoyaltyProgram event consumer

```yaml
apiVersion: batch/v1beta1
kind: CronJob
metadata:
  name: loyalty-program-consumer
spec:
  schedule: "*/1 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
            - name: loyalty-program
              image: microservicesindotnetregistry1.azurecr.io/loyalty-program:1.0.2
              imagePullPolicy: IfNotPresent
              env:
                - name: STARTUPDLL
                  value: "consumer/EventConsumer.dll"
          restartPolicy: Never
  concurrencyPolicy: Forbid
```

正如你所见，我们已经实现了慢速重试，因为这就是我们的事件消费者工作的方式。

## 7.3.4  Logging all unhandled exceptions

最后，我们将把注意力转向忠诚度计划微服务的 HTTP API。如前所述，你希望对系统中出现的所有错误保持良好的日志记录。这意味着你应该记录微服务控制器中抛出的任何未处理异常。事实上，忠诚度计划微服务已经这样做了。ASP.NET 默认管道的一部分是捕获所有其他未处理的异常并
将其记录到标准输出。要亲眼看到这一点，你可以在 UsersController 中添加一个新端点，该端点只是抛出一个 NotImplementedException：

## Listing 7.8 An endpoint that always throws an exception

```
[HttpGet("fail")]
public IActionResult Fail() => throw new NotImplementedException();
```

然后使用 dotnet run 运行忠诚度计划微服务并调用 https://localhost:5001/users/fail 上的新端点。控制台中的输出将包含由 NotImplementedException 引起的错误日志，类似于以下内容。

## Listing 7.9 Errors are logged to standard out

```
dotnet run
info: Microsoft.Hosting.Lifetime[0]
    Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
    Now listening on: http://localhost:5000
info: Microsoft.Hosting.Lifetime[0]
    Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
    Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
    Content root path: C:\Users\chors\Documents\horsdal3\code\Chapter07\
        LoyaltyProgram\LoyaltyProgram
fail: Microsoft.AspNetCore.Server.Kestrel[13]
    Connection id "0HM2KQ4LI56GR", Request id "0HM2KQ4LI56GR:00000001":
        An unhandled exception was thrown by the application.
System.NotImplementedException: The method or operation is not implemented.
    at LoyaltyProgram.Users.UsersController.Fail() in C:\Users\chors\
Documents\horsdal3\code\Chapter07\LoyaltyProgram\LoyaltyProgram\Users\
UsersController.cs:line 13
    at lambda_method(Closure , Object , Object[] )
    at Microsoft.Extensions.Internal.ObjectMethodExecutor.Execute(
Object target, Object[] parameters)
    at Microsoft.AspNetCore.Mvc.Infrastructure.ActionMethodExecutor.
SyncActionResultExecutor.Execute(IActionResultTypeMapper mapper,
ObjectMethodExecutor executor, Object controller, Object[] arguments)
    at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.
InvokeActionMethodAsync()
    at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.
Next(State& next, Scope& scope, Object& state, Boolean& isCompleted)
    at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.
InvokeNextActionFilterAsync()
--- End of stack trace from previous location where exception was thrown ---
    at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.
Rethrow(ActionExecutedContextSealed context)
    at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.
Next(State& next, Scope& scope, Object& state, Boolean& isCompleted)
    at Microsoft.AspNetCore.Mvc.Infrastructure.ControllerActionInvoker.
InvokeInnerFilterAsync()
```

Start of the error log

```
--- End of stack trace from previous location where exception was thrown ---
   at Microsoft.AspNetCore.Mvc.Infrastructure.ResourceInvoker.<
      InvokeFilterPipelineAsync>g__Awaited|19_0(ResourceInvoker invoker, Task lastTask, State next, Scope scope, Object state, Boolean isCompleted)
   at Microsoft.AspNetCore.Mvc.Infrastructure.ResourceInvoker.<InvokeAsync>
      g__Awaited|17_0(ResourceInvoker invoker, Task task, IDisposable scope)
   at Microsoft.AspNetCore.Routing.EndpointMiddleware.<Invoke>
      g__AwaitRequestTask|6_0(Endpoint endpoint, Task requestTask,
      ILogger logger)
   at Microsoft.AspNetCore.Server.Kestrel.Core.Internal.Http.HttpProtocol.
      ProcessRequests[TContext](IHttpApplication`1 application)
```

在第 10 章中，我们将回到日志记录并了解如何配置它并使其更有用，但就目前而言，我们很满意所有异常都被记录下来。

## 7.3.5 Deploying to Kubernetes

我们已经在忠诚度计划微服务周围的所有协作中实现了健壮性模式，但我们并没有改变协作风格——查询仍然是查询，命令仍然是命令，事件仍然是事件。此外，微服务之间的依赖关系仍然相同：忠诚度计划微服务仍然订阅来自特价优惠微服务的事件并仍然向通知微服务发送命令。这意味着部署到 Kubernetes 不需要改变，因此与我们在第 5 章中看到的完全相同。

至此，围绕忠诚度计划微服务的协作的健壮性措施的实现已经完成。有了这些相当简单的措施，协作在生产负载下很可能会更加健壮。

## Summary

- 由于微服务之间存在大量通信，你必须预期某些通信会失败。微服务优雅地处理此类故障对于系统的健壮性至关重要。
- 你应该在微服务中设计健壮性，以便故障不会在系统中传播并最终成为错误。
- 协作的客户端负责在出现故障时使通信健壮。
- 你应该拥有良好的日志，并且在需要调查生产问题时易于访问和搜索。中央日志微服务应接收所有日志消息并提供访问权限。
- 使通信健壮的最重要策略是重试和断路器模式。
- Polly 使设置和使用重试策略以及断路器变得容易。
- 默认情况下 ASP.NET 将错误记录到标准输出。现成的日志产品支持从所有微服务收集这些日志并将其集中化。

# Writing tests for microservices

- **This chapter covers**
- Writing good automated tests
- Understanding the test pyramid and how it applies to microservices
- Testing microservices from the outside
- Writing fast, in-process tests for endpoints
- Using `Microsoft.AspNetCore.TestHost` for integration and unit tests

到目前为止，我们已经编写了一些微服务并在它们之间设置了协作。实现是好的，但我们还没有为它们编写任何测试。随着我们编写越来越多的微服务，如果没有良好的自动化测试，开发系统将变得难以管理。本章的前半部分，我将讨论你需要为每个单独的微服务测试什么。然后我们将深入代码，首先查看在隔离中测试端点，然后查看如何像从另一个微服务发送请求一样测试完整的微服务，但使用 `Microsoft.AspNetCore.TestHost` 库中的 `TestServer` 在进程内进行测试。

