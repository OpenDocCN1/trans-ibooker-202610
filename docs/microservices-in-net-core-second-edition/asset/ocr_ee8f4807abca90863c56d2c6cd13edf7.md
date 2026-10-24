In the interest of focusing on the collaboration, I won’t show all the code in the loyalty program microservice. Rather, I’ll include the code for the HTTP API in the web process and the special offers event subscriber in the event-subscriber process.

# 5.2.1 Setting up a project for the loyalty program

The first thing to do in implementing the loyalty program microservice is to create an empty ASP.NET application and add MVC to it. You’ve already done this a couple of times—in chapters 1 and 2—so I won’t go over the details again here, but only remind you that your Startup.cs should look like this.

# Listing 5.2 Startup class with MVC

```
namespace LoyaltyProgram
{
    using Microsoft.AspNetCore.Builder;
    using Microsoft.AspNetCore.Hosting;
    using Microsoft.Extensions.DependencyInjection;

    public class Startup
    {
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddControllers();
        }

        public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
        {
            app.UseHttpsRedirection();
            app.UseRouting();
            app.UseEndpoints(endpoints => endpoints.MapControllers());
        }
    }
```

# 5.2.2 Implementing commands and queries

You now have a web project ready to host the implementations of the endpoints exposed by the loyalty program microservice. As listed earlier, these are the endpoints:

+   - An HTTP GET endpoint at URLs of the form “/users/{userId}” that responds with a representation of the user. This endpoint implements both queries in figure 5.3.
+   - An HTTP POST endpoint at “/users/” that expects a representation of a user in the body of the request and then registers that user in the loyalty program.
+   - An HTTP PUT endpoint at URLs of the form “/users/{userId}” that expects a representation of a user in the body of the request and then updates an already registered user.

You’ll implement the command endpoints first and then the query endpoint.