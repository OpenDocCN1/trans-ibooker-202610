# # Configuring our Service to Access Azure Storage

Essentially, this needs to represent a connection string to the storage account. You can
easily find the correct value for this in the Azure portal, under Access keys:

![](bbox=[0.105, 0.196, 0.895, 0.526])

We could simply copy this and paste it directly into the code by assigning the
__connectionString variable to it. However, instead, we're going to use
ConfigurationBuilder. Before we can make a start on this, we'll need some NuGet
packages:

```
```
Install-Package Microsoft.Extensions.Configuration -ProjectName
PhotoStorage.WindowsService

Install-Package Microsoft.Extensions.Configuration.Json -ProjectName
PhotoStorage.WindowsService
```
```

This should allow us to build our configuration using JSON files, so the next step is to
create one. We'll add a file called appsettings.json to our project: