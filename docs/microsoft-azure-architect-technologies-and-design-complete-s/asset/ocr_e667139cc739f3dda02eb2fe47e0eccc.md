looks a lot like a GUID. This is the piece of information that uniquely identifies your AI logs. The Instrumentation Key is a necessary, required piece of information for configuring AI logging from within application code. In an ASP.NET Core application, you wire the two together in a configuration file; it resembles something similar to the following:

```
{
  "ApplicationInsights": {
    "InstrumentationKey": "appInsightsInstrumentationKey"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  }
}
```

Then you install and configure the Microsoft.ApplicationInsights.AspNetCore NuGet package to create logs into the AI service as desired. This is what is truly meant by “application code monitoring” because the logging capability is really inside of the custom code running on the Azure platform. Next, take a look at the navigation menu items available for AI, and you will find one named Logs. Click Logs, and you will have the same query interface as you see in Figure 9.10; however, the tables you will see are those specific to AI, as shown in Figure 9.11. On the Overview blade for AI, you will see an Application Dashboard link. Click it, and you will see some impressive default information. See Figure 9.12.

# FIGURE 9.12 Application Insights—Application Dashboard

![](bbox=[0.13, 0.55, 0.85, 0.84])