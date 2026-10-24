
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

The specifics of the event-handling code will differ with every specific situation—that’s where the business logic is implemented.

With this in place, one batch can be handled. The final piece is configuring Kubernetes to call the program on a schedule of once an hour, which is done with this YAML, which defines a CronJob in Kubernetes that calls our event consumer every hour.

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

As you can see, we already implemented slow-paced retry, because that is how our event consumers work.

## 7.3.4  Logging all unhandled exceptions

Finally, we’ll turn our attention to the HTTP API of the loyalty program microservice. As stated earlier, you want to keep good logs of everything that goes wrong in the system. That means you should log any unhandled exceptions thrown in the controllers in the microservices. In fact, the loyalty program microservice already does this. Part of the default ASP.NET pipeline is to catch all otherwise unhandled exceptions and
log them to standard out. To see this in action, you can add a new endpoint to the UsersController that simply throws a NotImplementedException:

## Listing 7.8 An endpoint that always throws an exception

```
[HttpGet("fail")]
public IActionResult Fail() => throw new NotImplementedException();
```

Then run the loyalty program microservice with dotnet run and call the new endpoint at https://localhost:5001/users/fail. The output in the console will include an error log caused by the NotImplementedException similar to this.

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

In chapter 10, we will return to logging and see how to configure it and make it even more useful, but for now, we are happy with having all exceptions logged.

## 7.3.5 Deploying to Kubernetes

We have implemented robustness patterns in all the collaborations around the loyalty program microservice, but we haven’t changed the collaboration styles—queries are still queries, commands are still commands, and events are still events. Moreover, the dependencies between the microservices are still the same: the loyalty program microservice still subscribes to events from the special offers microservice and still sends commands to the notifications microservice. This means that the deployment to Kubernetes does not have to change and is therefore exactly the same as we saw in chapter 5.

This concludes the implementation of robustness measures in collaborations around the loyalty program microservice. With these fairly simple measures in place, the collaborations are likely to be a good deal more robust under production load.

## Summary

- Due to the amount of communication between microservices, you must expect some communication to fail. It’s vital for the robustness of the system that your microservices handle such failures gracefully.
- You should design robustness into your microservices so that failures don’t propagate through the system and eventually become errors.
- The client side of a collaboration is responsible for making communication robust in the face of failures.
- You should have good logs that are easy to access and search through when you need to investigate production problems. A central log microservice should receive all log messages and provide access to them.
- The most important strategies for making communications robust are the retry and circuit breaker patterns.
- Polly makes it easy to set up and use retry policies as well as circuit breakers.
- By default ASP.NET logs errors to standard out. The off-shelf logging products support gathering up these logs from all the microservices and centralizing them.

# Writing tests for microservices

- **This chapter covers**
- Writing good automated tests
- Understanding the test pyramid and how it applies to microservices
- Testing microservices from the outside
- Writing fast, in-process tests for endpoints
- Using `Microsoft.AspNetCore.TestHost` for integration and unit tests

Up to this point, we’ve written a few microservices and set up collaborations between some of them. The implementations are fine, but we haven’t written any tests for them. As we write more and more microservices, developing systems without good automated tests becomes unmanageable. In the first half of this chapter, I’ll discuss what you need to test for each individual microservice. Then we’ll dive into code, looking first at testing endpoints in isolation, and then at testing a complete microservice as if you were sending it requests from another microservice, but in-process using the `TestServer` from the `Microsoft.AspNetCore.TestHost` library.
