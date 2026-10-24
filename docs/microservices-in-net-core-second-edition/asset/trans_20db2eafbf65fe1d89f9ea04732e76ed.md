

public class TestController_should : IDisposable
{
    private readonly IHost host;
    private readonly HttpClient sut;

    public class TestController : ControllerBase
    {
        [HttpGet("/")]
        public OkResult Get() => Ok();
    }

    public TestController_should()
    {
        this.host = new HostBuilder()
            .ConfigureWebHost(host =>
            {
                host.ConfigureServices(x =>
                    x.AddControllersByType(typeof(TestController)))
                .Configure(x =>
                {
                    x.UseRouting()
                    .UseEndpoints(opt => opt.MapControllers())
                    .UseTestServer()
                })
                .Start();
            });
        this.sut = this.host.GetTestClient();
    }

    [Fact]
    public async Task respond_ok_to_request_to_root()
    {
        var actual = await this.sut.GetAsync("/");
        Assert.Equal(HttpStatusCode.OK, actual.StatusCode);
    }

    public void Dispose()
    {
        this.host?.Dispose();
        this.sut?.Dispose();
    }
}

![](img/608267d7803dc57a1a896ad121f37868_207_0.png)

![](img/608267d7803dc57a1a896ad121f37868_207_1.png)

![](img/608267d7803dc57a1a896ad121f37868_207_2.png)

![](img/608267d7803dc57a1a896ad121f37868_207_3.png)

![](img/608267d7803dc57a1a896ad121f37868_207_4.png)

![](img/608267d7803dc57a1a896ad121f37868_207_5.png)

![](img/608267d7803dc57a1a896ad121f37868_207_6.png)

关于这个测试，有几点需要注意：

-   测试类 `TestController_should` 实现了 `IDisposable` 接口。xUnit 运行器能识别 `IDisposable`，并在运行 Fact 之后调用 `Dispose` 方法。在 dispose 方法中，host 和 sut 会被释放，从而确保所有资源都被清理干净。如果 host 没有被释放，测试将一直挂起，直到测试运行器超时。
-   在 `TestController_should` 构造函数中，我们像生产代码一样创建了一个 Web 主机。这正是我们可以针对被测端点配置一切的地方。在这个测试中，我们使用了与生产代码类似的设置，只不过我们使用了测试服务器，这可以确保请求在进程内完成。

在配置主机的过程中，我们使用了一个自定义扩展方法 `AddControllersByType`，我接下来会展示这个方法。通过使用 `AddControllersByType`，我们确保主机只拥有一个控制器：`TestController`。因此，该测试可以相当精确地控制主机上哪些端点可用。

## 命名约定

我的测试遵循以下命名约定：

-   我的测试针对一个名为 `sut`（即 *被测系统* system under test）的对象进行操作。在前面的测试中，`sut` 是一个 `HttpClient` 对象，我用它来向端点发出调用。
-   我根据被测对象来命名测试类——在这个示例测试中是 `TestController`——后面再加上 `_should`。
-   我根据正在测试的场景和预期结果来命名 Fact 方法。我用下划线分隔 Fact 方法名称中的单词，并尽量确保它们与外围类名组合后能构成一个句子。例如，在这个测试中，将类名和 Fact 方法名拼接起来并把下划线替换为空格，就得到 “TestController should respond okay to requests to root.”（TestController 应当对根路径的请求返回正常响应）。

你是否喜欢这些约定纯属个人品味问题。我恰好喜欢它们，但它们对于编写好的测试绝非必需。

你可以使用 `dotnet test` 运行前面的测试；它会在内存中执行并给你良好的覆盖率，因为对 `sut.GetAsync("/")` 的调用会执行真实的 ASP.NET 管道，包括 `TestController` 中端点的实现。字符串参数 “/” 是发出假请求的相对 URL。在 8.3.1 节中，我们将介绍如何为这些单元测试建立项目，以及如何用 `dotnet test` 运行它们。

在 `TestController_should` 构造函数中，我们使用了自定义扩展方法 `AddControllersByType`，它的原理是覆盖 MVC 决定哪些类型是控制器的方式。这是通过创建 MVC 的 `ControllerFeatureProvider` 的自定义版本，并用自定义版本替换 MVC 默认的 `ControllerFeatureProvider` 来实现的。MVC 使用 `ControllerFeatureProvider` 来决定哪些类型是控制器，因此，用自定义版本覆盖它后，我们就可以决定 MVC 将哪些类型视为控制器，从而决定哪些端点属于测试范围。下面的清单展示了其实现，其中第一部分是自定义的 `ControllerFeatureProvider`，第二部分是扩展方法。

清单 8.2 控制哪些控制器属于测试范围的扩展方法

namespace LoyaltyProgramUnitTests
{
    using System;
    using System.Linq;
    using System.Reflection;
    using Microsoft.AspNetCore.Mvc.Controllers;
    using Microsoft.Extensions.DependencyInjection;

    public class FixedControllerProvider : ControllerFeatureProvider
    {
        private readonly Type[] controllerTypes;

        public FixedControllerProvider(params Type[] controllerTypes)
        {
            this.controllerTypes = controllerTypes;
        }

        protected override bool IsController(TypeInfo typeInfo)
        {
            return this.controllerTypes.Contains(typeInfo);
        }
    }

    public static class MvcBuilderExtensions
    {
        public static IMvcBuilder AddControllersByType(
            this IServiceCollection services,
            params Type[] controllerTypes)
            =>
            services
                .AddControllers()
                .ConfigureApplicationPartManager(mgr =>
                {
                    mgr.FeatureProviders.Remove(
                        mgr.FeatureProviders.First(f =>
                            f is ControllerFeatureProvider));
                    mgr.FeatureProviders.Add(
                        new FixedControllerProvider(controllerTypes));
                });
    }
}

-   控制器提供程序的自定义实现
-   覆盖用于识别控制器的方法。
-   扩展方法
-   让服务集合感知控制器。
-   访问功能提供程序。
-   移除默认的控制器提供程序功能。
-   添加自定义的控制器提供程序功能。

我们也会在后面的测试中使用这个扩展，把它作为控制测试范围的便捷方式。

在本章剩余部分中，我们将在代码层面工作，为忠诚度计划微服务实现单元测试和服务级测试。当你在第 5 章实现忠诚度计划时，它并没有事件提要，但对于这些示例，你将添加一个可供其他微服务订阅的事件提要。

## 8.3 使用 Microsoft.AspNetCore.TestHost 编写单元测试

在本节中，你将为实现忠诚度计划微服务中的端点编写一些单元测试。在第 5 章中，你看到忠诚度计划有三个命令和查询端点：

-   一个 HTTP GET 端点，URL 形式为 “/users/{userId}”，返回用户的表示形式。
-   一个 HTTP POST 端点，指向 “/users/”，它期望请求正文中包含用户的表示形式，然后将该用户注册到忠诚度计划中。
-   一个 HTTP PUT 端点，URL 形式为 “/users/{userId}”，它期望请求正文中包含用户的表示形式，然后更新一个已注册的用户。

让我们为这些端点编写测试。忠诚度计划微服务还有一个事件提要，你也将为其编写一个测试。你不会为忠诚度计划中的端点和事件提要编写全面的测试——只需足以了解如何针对 MVC 端点编写测试即可。在以下小节中，你将完成以下工作：

-   建立一个测试项目，用于存放忠诚度计划微服务的单元测试。
-   编写使用 Microsoft.AspNetCore.TestHost 来测试忠诚度计划中端点的测试，并让微服务中的代码使用真实数据库。你将编写三个这样的测试，分别针对以下三项功能：
    -   一个尝试读取不存在用户的测试。
    -   一个创建用户并读回该用户的测试。
    -   一个修改用户并读回该用户的测试。
-   编写同样使用 Microsoft.AspNetCore.TestHost 来测试某个端点、但通过在被测端点中注入模拟数据库来限制范围的测试。这些测试将测试微服务中的事件提要。

完成后，你将学会如何针对 MVC 端点编写单元测试——既包括带真实数据库的情况，也包括不带真实数据库的情况。

### 8.3.1 建立单元测试项目

在开始编写测试之前，你需要一个项目来存放它们。为此，请在 LoyaltyProgram 项目旁边创建一个新项目，并将其命名为 LoyaltyProgramUnitTests。你可以使用 IDE 或 dotnet cli 通过命令 `dotnet new xunit -n LoyaltyProgramUnitTests` 创建测试项目。你的解决方案应类似于这样：

```
C:.
  Ch08.sln
  Dockerfile
  loyalty-program.yaml
  EventConsumer
    EventConsumer.csproj
    Program.cs
  LoyaltyProgram
    appsettings.Development.json
```

