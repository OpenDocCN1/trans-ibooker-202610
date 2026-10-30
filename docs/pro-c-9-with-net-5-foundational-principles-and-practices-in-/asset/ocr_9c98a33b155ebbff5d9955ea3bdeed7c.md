# The Options Pattern in ASP.NET Core

The options pattern provides access of the configured settings classes to other classes through dependency injection. The configuration classes can be injected into another class using one of the versions of IOptions<T>. There are several versions of this interface, as shown in Table 31-6.

Table 31-6. Some of the IOptions Interfaces

| Interface | Description |
|---|---|
| IOptionsMonitor<T> | Retrieves options and supports the following: notification of changes (with OnChange), configuration reloading, named options (with Get and CurrentValue), and selective options invalidation. |
| IOptionsMonitorCache<T> | Caches instances of T with support for full/partial invalidation/reload. |
| IOptionsSnapshot<T> | Recomputes options on every request. |
| IOptionsFactory<T> | Creates new instances of T. |
| IOptions<T> | Root interface. Doesn't support IOptionsMonitor<T>. Left in for backward compatibility. |

# Add the Dealer Information

A car site should show the dealer information, and that information should be customizable without having to redeploy the entire site. This will be accomplished using the options pattern. Start by updating the appsettings.json file to add the dealer information.

```
{
  "Logging": {
    "MSSqlServer": {
      "schema": "Logging",
      "tableName": "SeriLogs",
      "restrictedToMinimumLevel": "Warning"
    }
  },
  "ApplicationName": "AutoLot.Mvc",
  "AllowedHosts": "*",
  "DealerInfo": {
    "DealerName": "Skimedic's Used Cars",
    "City": "West Chester",
    "State": "Ohio"
  }
}
```

Next, we need to create a view model to hold dealer information. In the Models folder of the AutoLot.Mvc project, add a new class named DealerInfo.cs. Update the class to the following:

```
namespace AutoLot.Mvc.Models
{
  public class DealerInfo
  {
    public string DealerName { get; set; }
    public string City { get; set; }
  }
}
```