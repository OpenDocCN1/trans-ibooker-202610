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

Notice that MsSqlStartupHealthCheck implements IHealthChecks, which means it fits right into the AddAdditionStartupHealthChecks created. A microservice can use the MicroserviceNET.Monitoring package and the MsSqlStartupHealthCheck.

# Listing 11.6 Registering health checks using the microservice platform

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

The microservice can expose these checks in the standardized monitoring endpoints by leveraging the application builder extension created.

# Listing 11.7 Adding the standardized monitoring endpoints

```
public void Configure(IApplicationBuilder app)
{
    app.UseRouting();
    app.UseKubernetesHealthChecks();
    app.UseEndpoints(endpoints => endpoints.MapControllers());
}
```

We have created a first iteration of a microservice platform that helps standardize our microservice log, trace, and expose monitoring endpoints. To use the platform, the microservices need to install two NuGet packages—MicroserviceNET.Logging and MicroserviceNET.Monitoring—and use the host builder extension in Program.cs to set up the logging and extensions on service collection and application builder to set up the monitoring endpoints in the Startup class.