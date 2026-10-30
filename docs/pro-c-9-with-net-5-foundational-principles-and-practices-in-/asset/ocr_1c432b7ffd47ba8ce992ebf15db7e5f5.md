```
{
    AutoCreateSqlTable = false,
    SchemaName = schema,
    TableName = tableName,
};
if (hostingContext.HostingEnvironment.IsDevelopment())
{
    sqlOptions.BatchPeriod = new TimeSpan(0, 0, 0, 1);
    sqlOptions.BatchPostingLimit = 1;
}
loggerConfiguration
    .Enrich.FromLogContext()
    .Enrich.WithMachineName()
    .WriteTo.File(
        path: "ErrorLog.txt",
        rollingInterval: RollingInterval.Day,
        restrictedToMinimumLevel: logLevel,
        outputTemplate: OutputTemplate)
    .WriteTo.Console(restrictedToMinimumLevel: logLevel)
    .WriteTo.MSSqlServer(
        connectionString: connectionString,
        sqlOptions,
        restrictedToMinimumLevel: level,
        columnOptions: ColumnOptions);
});
return builder;
}
```

With everything in place, it’s time to swap out default logger for Serilog.

# App Settings Updates

The Logging section of all the app settings files (appsettings.json, appsettings.development.json, and appsettings.production) for the AutoLot.Api and AutoLot.Dal projects must be updated with new logging information as well as adding the application name.

Open the appsettings.json files and update the JSON to the following, making sure to use the correct project name for the ApplicationName node and updating the connection string to match your configuration:

```
//appsettings.json
{
  "Logging": {
    "MSSqlServer": {
      "schema": "Logging",
      "tableName": "SeriLogs",
      "restrictedToMinimumLevel": "Warning"
    }
  },
  "ApplicationName": "AutoLot.Api",
  "AllowedHosts": "*"
}
```