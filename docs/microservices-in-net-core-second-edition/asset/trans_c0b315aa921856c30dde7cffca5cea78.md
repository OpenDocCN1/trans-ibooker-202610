

将第 9 章的配置代码以及该代码使用的 NuGet 包迁移到 MicroserviceNET.Logging 库中。我会在这里重复该代码，但有关代码的解释请参考第 9 章。
向 MicroservicesNET.Logging 库添加 NuGet 包 Microsoft.Extensions.Hosting、Serilog.AspNetCore、Serilog.Sinks.ColoredConsole 和 Serilog.Enrichers.Span。然后在 MicroserviceNET.Logging 项目中添加一个 HostBuilderExtensions.cs 文件，并将以下代码放入其中，该代码将 Serilog 配置为记录器并使用跟踪信息丰富所有日志。

## Listing 11.1 Logger configuration from chapter 9

```
namespace MicroserviceNET.Logging
{
    using Microsoft.Extensions.Hosting;
    using Serilog;
    using Serilog.Enrichers.Span;
    using Serilog.Formatting.Json;

    public static class HostBuilderExtensions
    {
        public static IHostBuilder UseLogging(this IHostBuilder builder) =>
            builder.UseSerilog((context, logger) =>
            {
                logger
                    .Enrich.FromLogContext()
                    .Enrich.WithSpan();
                if (context.HostingEnvironment.IsDevelopment())
                    logger.WriteTo.ColoredConsole(
                        outputTemplate: "{Timestamp:yyyy-MM-dd HH:mm:ss} {TraceId} { Level:u3} {Message}{NewLine}{Exception}");
                else
                    logger.WriteTo.Console(new JsonFormatter());
            });
    }
```

这就是 MicroserviceNET.Logging 项目中所需的所有代码。这将日志配置代码从购物车微服务中提取出来，并将其放置在一个方便的扩展方法 UseLogging 中，我们可以从使用该平台的每个微服务的 Program.cs 文件中调用它。

## Listing 11.2 Using UseLogging in a Program.cs

```
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;
using MicroserviceNET.Logging

CreateHostBuilder(args).Build().Run();

static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
    .UseLogging()
```

```
```csharp
.ConfigureWebHostDefaults(
    webBuilder => { webBuilder.UseStartup<Startup>(); });
```
```

要从 MicroserviceNET.Logging 库创建 NuGet 包，可以使用 dotnet。转到项目文件夹——即包含 MicroserviceNET.Logging .csproj 文件的文件夹——并运行以下命令：

```
```powershell
PS> dotnet pack --configuration Release
```
```

这将在 bin/Release 中创建一个名为 MicroserviceNET.Logging.1.0.0.nupkg 的 NuGet 包。该包包含编译为程序集的 MicroserviceNET.Logging 项目的代码。此外，该包需要我们在 MicroserviceNET.Logging 中安装的三个 Serilog 包和 Microsoft.Extensions.Hosting 包，这意味着每当此包安装到项目中时，这四个包也会被安装。

该包已准备好安装并在任意数量的微服务中使用。

## 11.4.2 Creating a package with monitoring endpoints

在本节中，我们将以相同的方式创建另一个 NuGet 包。它将包含 Kubernetes 期望的监控端点的健康检查代码。创建新包有三个步骤：

- 创建一个名为 `MicroserviceNET.Monitoring` 的新类库。
- 添加第 9 章中的健康检查代码。
- 使用 `dotnet pack` 构建包。

一旦你创建了一个名为 MicroserviceNET.Monitoring 的新类库，你应该向其中添加这些 NuGet 包，因为它们被健康检查代码使用：

- `Microsoft.Extensions.DependencyInjection`
- `Microsoft.Extensions.Diagnostics.HealthChecks`
- `Microsoft.AspNetCore.Http.Abstractions`
- `Microsoft.AspNetCore.Diagnostics.HealthChecks`

添加健康检查有两个步骤。首先，它们被添加到服务集合中，然后添加到应用程序构建器中。第一部分我们放在 MicroserviceNET.Monitoring 库中的一个新文件 ServiceCollectionExtensions.cs 中。我们从第 9 章的购物车代码中提取添加健康检查到服务集合的代码，并补充了一些添加更多健康检查的便捷方法。

```
```csharp
namespace MicroserviceNET.Monitoring
{
    using Microsoft.Extensions.DependencyInjection;
    using Microsoft.Extensions.Diagnostics.HealthChecks;

    public static class ServiceCollectionExtensions
    {
```
```

```
private const string Liveness = "liveness";
private const string Startup = "startup";

public static IServiceCollection AddBasicHealthChecks(
    this IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck("BasicStartupHealthCheck",
            () => HealthCheckResult.Healthy(), tags: new[] {Startup})
        .AddCheck("BasicLivenessHealthCheck",
            () => HealthCheckResult.Healthy(), tags: new[] {Liveness});

    return services;
}

public static IServiceCollection AddAdditionStartupHealthChecks<T>(
    this IServiceCollection services) where T : class, IHealthCheck
{
    services.AddHealthChecks().AddCheck<T>(nameof(T), tags: new[] {
        Startup});
    return services;
}

public static IServiceCollection AddAdditionLivenessHealthChecks<T>(
    this IServiceCollection services) where T : class, IHealthCheck
{
    services.AddHealthChecks().AddCheck<T>(nameof(T), tags: new[] {
        Liveness});
    return services;
}
}
```

添加一个基本的启动检查和一个基本存活检查。

使用 startup 进行标记以便后续使用。

使用 liveness 进行标记以便后续使用。

允许添加更多启动检查。

允许添加更多存活检查。

这只是为启动和存活注册非常基础的——空的——健康检查。检查被标记为 startup 或 liveness，这将在我们将健康检查添加到应用程序构建器时使用。还有两个方法——AddAdditionStartupHealthChecks 和 AddAdditionLivenessHealthChecks——使注册额外的检查变得容易，并将它们添加到启动或存活健康端点。

第二步是将健康检查添加到应用程序构建器。我们向库中添加另一个文件。将其命名为 ApplicationBuilderExtensions 并填入以下代码。

## Listing 11.4 Convenient extension method for adding health endpoints

```
namespace MicroserviceNET.Monitoring
{
    using Microsoft.AspNetCore.Builder;
    using Microsoft.AspNetCore.Diagnostics.HealthChecks;

    public static class ApplicationBuilderExtensions
    {
```

```
public static IApplicationBuilder
UseKubernetesHealthChecks(this IApplicationBuilder app) =>
    app
        .UseHealthChecks("/health/startup",
            new HealthCheckOptions
            {
                Predicate = x => x.Tags.Contains("startup")
            })
        .UseHealthChecks("/health/live",
            new HealthCheckOptions
            {
                Predicate = x => x.Tags.Contains("liveness")
            });
```

添加我们在 Kubernetes 清单中使用的启动端点。

添加所有标记为 startup 的健康检查。

添加我们在 Kubernetes 清单中使用的存活端点。

添加所有标记为 liveness 的健康检查。

健康检查上的标签用于确定它们属于哪个端点。这确保基本健康检查以及任何额外的健康检查都被添加到启动和存活端点。

现在我们准备好使用 dotnet pack 构建 NuGet 包并开始使用 MicroserviceNET.Monitoring 包。它将帮助我们确保使用它的所有微服务都公开相同的监控端点。

不过，在构建 NuGet 包之前，还有一个决定需要做出。在第 9 章中，我们还创建了一个在启动期间运行并检查数据库连接是否可以建立的 DbHealthCheck。如果这与你几乎所有的微服务都相关，你也可以将其添加到 MicroserviceNET.Monitoring 中。如果它只与一些微服务相关，我们就进入了一个灰色地带：如果足够多的微服务使用 SQL Server 数据库，我们可以向我们的微服务平台添加另一个 NuGet 包并将 DbHealthCheck 添加到其中。然后微服务可以引入这个第三个包。

作为提醒，我将重复 DbHealthCheck 的代码，重命名为 MsSqlStartupHealthCheck 以使用法更明确。

## Listing 11.5 Startup health check that uses an SQL Server database

```
namespace MicroserviceNET.Monitoring
{
    using System.Data.SqlClient;
    using System.Net.Http;
    using System.Threading;
    using System.Threading.Tasks;
    using Dapper;
    using Microsoft.Extensions.Diagnostics.HealthChecks;
    using Microsoft.Extensions.Logging;

    public class MsSqlStartupHealthCheck : IHealthCheck
    {
        private readonly HttpClient httpClient;

        public MsSqlStartupHealthCheck(ILoggerFactory logger)
        {
```

新名称表示为 MS SQL server

