```
public static class Environments
{
    public static readonly string Development = "Development";
    public static readonly string Staging = "Staging";
    public static readonly string Production = "Production";
}
```

The HostEnvironmentEnvExtensions class provides extensions methods on the IHostEnvironment for working with the environment name property. Table 29-10 lists the convenience methods available.

# Table 29-10 The HostEnvironmentEnvExtensions Methods

| Method | Functionality Provided |
|---|---|
| IsProduction | Returns true if the environment variable is set to Production (case insensitive) |
| IsStaging | Returns true if the environment variable is set to Staging (case insensitive) |
| IsDevelopment | Returns true if the environment variable is set to Development (case insensitive) |
| IsEnvironment | Returns true if the environment variable matches the string passed into the method (case insensitive) |

These are some examples of using the environment setting:

+   - Determining which configuration files to load
+   - Setting debugging, error, and logging options
+   - Loading environment-specific JavaScript and CSS files

You will see each of these in action while building the AutoLot.Api and AutoLot.Mvc applications in the next two chapters.

# Application Configuration

Previous versions of ASP.NET used the web.config file to configure services and applications, and developers accessed the configuration settings through the System.Configuration class. Of course, all configuration settings for the site, not just application-specific settings, were dumped into the web.config file making it a (potentially) complicated mess.

ASP.NET Core introduces a greatly simplified configuration system. By default, it’s based on simple JSON files that hold configuration settings as name-value pairs. The default file for configuration is the appsettings.json file. The initial version of appsettings.json file (created by the ASP.NET Core web application and API service templates) simply contains configuration information for the logging as well as a setting to limit the hosts and is listed here:

```
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  }
}
```