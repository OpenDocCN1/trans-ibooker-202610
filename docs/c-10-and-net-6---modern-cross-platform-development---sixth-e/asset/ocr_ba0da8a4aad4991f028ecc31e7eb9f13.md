12. In the Northwind.Common.DataContext.SqlServer project, add a project reference to the Northwind.Common.EntityModels.SqlServer project and add a package reference to the EF Core data provider for SQL Server, as shown in the following markup:

```
<ItemGroup>
  <PackageReference
    Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="6.0.0" />
</ItemGroup>

<ItemGroup>
  <ProjectReference Include=
    "..\Northwind.Common.EntityModels.SqlServer\Northwind.Common
.EntityModels.SqlServer.csproj" />
</ItemGroup>
```

13. In the Northwind.Common.DataContext.SqlServer project, delete the Class1.cs class file.

14. Build the Northwind.Common.DataContext.SqlServer project.

15. Move the NorthwindContext.cs file from the Northwind.Common.EntityModels.SqlServer project/folder to the Northwind.Common.DataContext.SqlServer project/folder.

16. In NorthwindContext.cs, remove the compiler warning about the connection string.

17. Add a class named NorthwindContextExtensions.cs, and modify its contents to define an extension method that adds the Northwind database context to a collection of dependency services, as shown in the following code:

```
using Microsoft.EntityFrameworkCore; // UseSqlServer
using Microsoft.Extensions.DependencyInjection; // IServiceCollection

namespace Packt.Shared;

public static class NorthwindContextExtensions
{
    /// <summary>
    /// Adds NorthwindContext to the specified IServiceCollection. Uses the SqlServer database provider.
    /// </summary>
    /// <param name="services"></param>
    /// <param name="connectionString">Set to override the default.</param>
    /// <returns>An IServiceCollection that can be used to add more services.</returns>
    public static IServiceCollection AddNorthwindContext(
        this IServiceCollection services, string connectionString =
        "Data Source=.;Initial Catalog=Northwind;"
        + "Integrated Security=true;MultipleActiveResultsets=true;")
    {
```