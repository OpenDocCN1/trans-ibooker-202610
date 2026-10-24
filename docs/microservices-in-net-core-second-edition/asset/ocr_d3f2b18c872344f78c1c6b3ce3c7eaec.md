check to the service collection and using it in an endpoint in the pipeline. To implement that we change the Startup class to the following.

# Listing 9.3 The full Startup class with both monitoring endpoints

```
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddHealthChecks()
            .AddCheck<DbHealthCheck>(
                nameof(DbHealthCheck),
                tags: new [] { "startup" })
            .AddCheck(
                "LivenessHealthCheck",
                () => HealthCheckResult.Healthy(),
                tags: new [] { "liveness" });
        services.AddControllers();
    }

    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        app.UseHttpsRedirection();
        app.UseHealthChecks(
            "/health/startup",
            new HealthCheckOptions
            {
                Predicate = x => x.Tags.Contains("startup")
            });
        app.UseHealthChecks(
            "/health/live",
            new HealthCheckOptions
            {
                Predicate = x => x.Tags.Contains("liveness")
            });
        app.UseRouting();
        app.UseEndpoints(endpoints => endpoints.MapControllers());
    }
}

Use the DbHealthCheck class to implement a health check.
Tag the health check with "startup."
Add /health/startup as a health check endpoint.
Use the new health check in /health/startup.
```

With this in place, you can run the shopping cart microservice with dotnet and test the monitoring endpoints. A request to the /health/live endpoint looks like this:

```
GET https://localhost:5001/health/live
```

A successful response looks as follows:

```
HTTP/1.1 200 OK
Content-Type: text/plain

Healthy
```

Here’s a request to the /health/startup endpoint:

```
GET https://localhost:5001/health/startup
```