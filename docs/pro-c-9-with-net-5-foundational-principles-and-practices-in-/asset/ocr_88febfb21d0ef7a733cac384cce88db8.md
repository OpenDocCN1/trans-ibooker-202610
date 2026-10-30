# The Program.cs File

Open the Program.cs class in the AutoLot.Api application, and examine the contents, shown here for reference:

```
namespace AutoLot.Api
{
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateHostBuilder(args).Build().Run();
        }

        public static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
                .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder.UseStartup<Startup>();
                });
    }
}
```

The CreateDefaultBuilder() method compacts the most typical application setup into one method call. It configures the app (using environment variables and appsettings JSON files), it configures the default logging provider, and it sets up the dependency injection container. This setup is provided by the ASP.NET Core templates for API- and MVC-style applications.

The next method (ConfigureWebHostDefaults()) is also a meta-method that adds support for Kestrel, IIS, and additional configuration. The final step is to set the application-specific configuration class, which in this example (and by convention) is named Startup. The final step is to use the Run() method to activate the web host.

In addition to the WebHost, the preceding code also creates the IConfiguration instance and adds it into the dependency injection container.

# The Startup.cs File

The Startup class configures how the application will handle HTTP requests and responses, configures any needed services, and adds services to the dependency injection container. The class name can be anything, as long as it matches the UseStartup<T>() line in the configuration of the CreateHostBuilder() method, but the convention is to name the class Startup.

# Available Services for Startup

The startup process needs access to framework and environmental services and values, and these are injected into the class by the framework. There are five services available to the Startup class for configuring the application, listed in Table 29-11.