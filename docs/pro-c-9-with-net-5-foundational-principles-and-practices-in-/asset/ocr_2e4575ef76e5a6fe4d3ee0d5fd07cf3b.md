Confirm that the project reference from AutoLot.Dal to AutoLot.Dal.Models is still in place. Add the following namespaces to MainWindow.xaml.cs:

```
using System.Linq;
using AutoLot.Dal.EfStructures;
using AutoLot.Dal.Repos;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
```

Add two module-level properties in MainWindow.cs to hold the instances of IConfiguration and ApplicationDbContext.

```
private IConfiguration _configuration;
private ApplicationDbContext _context;
```

Add a new method named GetConfigurationAndContext() to create those instances and call it from the constructor. The entire method is listed here:

```
private void GetConfigurationAndDbContext()
{
_configuration = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("appsettings.json", true, true)
    .Build();
var optionsBuilder =
    new DbContextOptionsBuilder<ApplicationDbContext>();
var connectionString =
    _configuration.GetConnectionString("AutoLot");
optionsBuilder.UseSqlServer(connectionString,
    sqlOptions => sqlOptions.EnableRetryOnFailure());
_context = new ApplicationDbContext(optionsBuilder.Options);
}
```

Add a new JSON file named appsettings.json to the project and set its build status to copy always. This can be done by right-clicking the file in Solution Explorer, selecting Properties, and then entering Copy always for the Copy To Output Directory setting. You can also add this to the project file to accomplish the same thing:

```
<ItemGroup>
    <None Update="appsettings.json">
        <CopyToOutputDirectory>Always</CopyToOutputDirectory>
    </None>
</ItemGroup>
```

Update the JSON file to the following (updating your connection string to match your environment):

```
{
    "ConnectionStrings": {
        "AutoLotFinal": "server=.,5433;Database=AutoLot;User Id=sa;Password=P@ssw0rd;"
    }
}
```