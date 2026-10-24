
The plan to implement the monitoring endpoints is as follows:

- 1. Add the `/health/live` endpoint to the ASP.NET pipeline.
- 2. Create the health check for the shopping cart database.
- 3. Add the `/health/startup` endpoint to the pipeline.

Let’s get started.

## 9.3.1 Implementing the /health/live monitoring endpoint

To implement the `/health/live` endpoint, we take advantage of ASP.NET’s health checks, which allows us to easily add monitoring endpoints. First we need to add a health check to the service collection. Next we will connect that health check to the `/health/live` endpoint. We will call the health check LivenessHealthCheck and add to the service collection by changing the ConfigureServices method in Startup to this.

### Listing 9.1 Adding the LivenessHealthCheck to the service collection

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck(
            "LivenessHealthCheck",
            () => HealthCheckResult.Healthy(),
            tags: new []{ "liveness"});
    services.AddControllers();
}

// Annotations:
// Add facilities supporting health checks.
// Give the health check a name.
// Add the LivenessHealthCheck.
// Implement the health check to always respond 200 OK.
// Tag the health check to make it easier to find later.
```

This code simply adds a health check that always responds 200 OK to the service collection. Let’s connect that to the `/health/live` endpoint by adding health/live as a health check to the ASP.NET pipeline by changing the Configure method in Startup to this:

```
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    app.UseHttpsRedirection();
    app.UseHealthChecks("/health/live",
        new HealthCheckOptions
        {
            Predicate = x => x.Tags.Contains("liveness")
        });
    app.UseRouting();
    app.UseEndpoints(endpoints => endpoints.MapControllers());
}

// Annotations:
// Add /health/live as a monitoring endpoint.
// Use all health checks tagged with liveness.
// Configure how to perform the health check for /health/live.
```

This adds `/health/live` as an endpoint that accepts HTTP GET requests to the pipeline. When a requests to `/health/live` comes in, all health checks tagged with “liveness” in the service collection are performed, and if they all succeed, the response will be 200 OK. In this case, there is only one health check tagged with “liveness,” but we could add more if need be. This gives a nice, modular way to combine different health checks into monitoring endpoints.

This is all we need to implement the /health/live endpoint. Now let’s turn to the /health/startup endpoint, which follows the same pattern with just one extra piece added.

## 9.3.2 Implementing the /health/startup monitoring endpoint

With the liveness health check in place, we can turn to implementing the startup health check. This a little bit more involved since we will check if the microservice can reach the database in the startup health check. To do that, we will first implement the database check in a class implementing the ASP.NET interface IHealthCheck and then use that class to define another health check. The following listing shows the implementation of the database check.

### Listing 9.2 Class implementing a database health check

```
namespace ShoppingCart
{
    using System.Data.SqlClient;
    using System.Threading;
    using System.Threading.Tasks;
    using Dapper;
    using Microsoft.Extensions.Diagnostics.HealthChecks;

    public class DbHealthCheck : IHealthCheck
    {
        public async Task<HealthCheckResult> CheckHealthAsync(
            HealthCheckContext context,
            CancellationToken cancellationToken)
        {
            await using var conn =
                new SqlConnection("Data Source=localhost;
Initial Catalog=ShoppingCart;
User Id=SA; Password=yourStrong(!)Password");

            var result = await conn.QuerySingleAsync<int>("SELECT 1");
            return result == 1
                ? HealthCheckResult.Healthy()
                : HealthCheckResult.Degraded();
        }
    }
```

Implement IHealthCheck.

Open database connection.

If the query result is 1, the health check is successful.

Perform simple query.

This class implements the IHealthCheck interface, which defines just one method—CheckHealthAsync—which is called whenever the health check is executed. The return value from CheckHealthAsync indicates whether the check was successful.

The rest of the implementation of the /health/startup endpoint is similar to the implementation of the /health/live endpoint and consists of adding the health check to the service collection and using it in an endpoint in the pipeline. To implement that we change the Startup class to the following.

### Listing 9.3 The full Startup class with both monitoring endpoints

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

And this is a successful response:

```
HTTP/1.1 200 OK
Content-Type: text/plain

Healthy
```

Finally, the following is a failure response:

```
HTTP/1.1 503 Service Unavailable
Content-Type: text/plain

Unhealthy
```

This concludes the implementation of the monitoring endpoints. Later, we will return to these endpoints and look at how to configure Kubernetes to use them.

## 9.4 *Implementing structured logging*

As discussed, having structured logs raises the value of the logs, mainly by making them more searchable. In this section we will configure the shopping cart to use Serilog to make the log output structured. There are three small steps to making the logs structured:

- 1. Adding the Serilog NuGet package and a helper package to the shopping cart microservice
- 2. Configuring the .NET logger to use Serilog behind the scenes
- 3. Changing the log output to a JSON format, which is more machine-friendly

Once these three steps are performed, we will have logs that lend themselves to being picked up from the Kubernetes logs and shipped to a logging product of your choice. (I will not show how to ship the logs, since that becomes more specific to the logging product you choose, and each vendor has the documentation needed to set up the log shipping.)

First, we will add two NuGet packages to the shopping cart microservice:

- 1. Serilog.Sinks.ColoredConsole: The library that allows us to do structured logging to the console
- 2. Serilog.AspNetCore: A helper package for using .NET's logging and Serilog together

Once these two packages have been added to the shopping cart microservice we can configure the .NET logger to use Serilog with the following code in Program.cs.

### Listing 9.4 Using Serilog as the backend for the .NET logger

```
namespace ShoppingCart
{
    using Microsoft.AspNetCore.Hosting;
    using Microsoft.Extensions.Hosting;
    using Serilog;
```
