
```csharp
/// </summary>
/// <param name="services"></param>
/// <param name="relativePath">Set to override the default of ".."</param>
/// <returns>An IServiceCollection that can be used to add more services.</returns>
public static IServiceCollection AddNorthwindContext(
    this IServiceCollection services, string relativePath = "..")
{
    string databasePath = Path.Combine(relativePath, "Northwind.db");

    services.AddDbContext<NorthwindContext>(options =>
        options.UseSqlite($"Data Source={databasePath}")
    );

    return services;
}
```

12. Build the two class libraries and fix any compiler errors.

## Creating a class library for entity models using SQL Server

To use SQL Server, you will not need to do anything if you already set up the Northwind database in Chapter 10, Working with Data Using Entity Framework Core. But you will now create the entity models using the dotnet-ef tool:

- 1. Use your preferred code editor to create a new solution/workspace named PracticalApps.
- 2. Add a class library project, as defined in the following list:
  - 1. Project template: Class Library / classlib
  - 2. Workspace/solution file and folder: PracticalApps
  - 3. Project file and folder: Northwind.Common.EntityModels.SqlServer
- 3. In the Northwind.Common.EntityModels.SqlServer project, add package references for the SQL Server database provider and EF Core design-time support, as shown in the following markup:

```xml
<ItemGroup>
    <PackageReference
        Include="Microsoft.EntityFrameworkCore.SqlServer"
        Version="6.0.0" />
    <PackageReference
        Include="Microsoft.EntityFrameworkCore.Design"
Version="6.0.0">
<PrivateAssets>all</PrivateAssets>
<IncludeAssets>runtime; build; native; contentfiles; analyzers;
buildtransitive</IncludeAssets>
</PackageReference>
</ItemGroup>
```

- 4. Delete the Class1.cs file.
- 5. Build the project.
- 6. Open a command prompt or terminal for the Northwind.Common.EntityModels.SqlServer folder.
- 7. At the command line, generate entity class models for all tables, as shown in the following commands:

```
dotnet ef dbcontext scaffold "Data Source=.;Initial Catalog=Northwind;Integrated Security=true;" Microsoft.EntityFrameworkCore.SqlServer --namespace Packt.Shared --data-annotations
```

Note the following:

- The command to perform: dbcontext scaffold
- The connection strings. "Data Source=.;Initial Catalog=Northwind;Integrated Security=true;"
- The database provider: Microsoft.EntityFrameworkCore.SqlServer
- The namespace: --namespace Packt.Shared
- To use data annotations as well as the Fluent API: --data-annotations

8. In Customer.cs, add a regular expression to validate its primary key value to only allow uppercase Western characters, as shown highlighted in the following code:

```
[Key]
[StringLength(5)]
[RegularExpression("[A-Z]{5}")]
public string CustomerId { get; set; } = null!;
```

9. In Customer.cs, make the CustomerId and CompanyName properties required.

10. Add a class library project to the solution/workspace, as defined in the following list:

- 1. Project template: Class Library / classlib
- 2. Workspace/solution file and folder: PracticalApps
- 3. Project file and folder: Northwind.Common.DataContext.SqlServer

11. In Visual Studio Code, select Northwind.Common.DataContext.SqlServer as the active OmniSharp project.

12. In the Northwind.Common.DataContext.SqlServer project, add a project reference to the Northwind.Common.EntityModels.SqlServer project and add a package reference to the EF Core data provider for SQL Server, as shown in the following markup:

```
<ItemGroup>
  <PackageReference
    Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="6.0.0" />
</ItemGroup>

<ItemGroup>
  <ProjectReference Include=
    "..\Northwind.Common.EntityModels.SqlServer\Northwind.Common.EntityModels.SqlServer.csproj" />
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
services.AddDbContext<NorthwindContext>(options =>
    options.UseSqlServer(connectionString));

    return services;
}
}
```

18. Build the two class libraries and fix any compiler errors.

![](img/c63b00dca7192cc58cfb9e1f1e64f483_595_0.png)

Good Practice: We have provided optional arguments for the AddNorthwindContext method so that we can override the hardcoded SQLite database filename path or the SQL Server database connection string. This will allow us more flexibility, for example, to load these values from a configuration file.

## Practicing and exploring

Explore this chapter's topics with deeper research.

## Exercise 13.1 – Test your knowledge

- 1. .NET 6 is cross-platform. Windows Forms and WPF apps can run on .NET 6. Can those apps therefore run on macOS and Linux?
- 2. How does a Windows Forms app define its user interface, and why is this a potential problem?
- 3. How can a WPF or UWP app define its user interface, and why is this good for developers?

## Exercise 13.2 – Explore topics

Use the links on the following page to learn more detail about the topics covered in this chapter:

https://github.com/markjprice/cs10dotnet6/blob/main/book-links.md#chapter-13---introducing-practical-applications-of-c-and-net

## Summary

In this chapter, you have been introduced to some of the app models and workloads that you can use to build practical applications using C# and .NET.

You have created two to four class libraries to define an entity data model for working with the Northwind database using either SQLite or SQL Server or both.

In the following six chapters, you will learn the details about how to build the following:

- Simple websites using static HTML pages and dynamic Razor Pages.
- Complex websites using the Model-View-Controller (MVC) design pattern.
- Web services that can be called by any platform that can make an HTTP request and client websites that call those web services.
- Blazor user interface components that can be hosted on a web server, in the browser, or on hybrid web-native mobile and desktop apps.
- Services that implement remote procedure calls using gRPC.
- Services that implement real-time communication using SignalR.
- Services that provide easy and flexible access to an EF Core model.
- Serverless nano services hosted in Azure Functions.
- Cross-platform native mobile and desktop apps using .NET MAUI.

## 14

## Building Websites Using ASP.NET Core Razor Pages

This chapter is about building websites with a modern HTTP architecture on the server side using Microsoft ASP.NET Core. You will learn about building simple websites using the ASP.NET Core Razor Pages feature introduced with ASP.NET Core 2.0 and the Razor class library feature introduced with ASP.NET Core 2.1.

This chapter will cover the following topics:

- Understanding web development
- Understanding ASP.NET Core
- Exploring ASP.NET Core Razor Pages
- Using Entity Framework Core with ASP.NET Core
- Using Razor class libraries
- Configuring services and the HTTP request pipeline

## Understanding web development

Developing for the web means developing with Hypertext Transfer Protocol (HTTP), so we will start by reviewing this important foundational technology.

## Understanding HTTP

To communicate with a web server, the client, also known as the user agent, makes calls over the network using HTTP. As such, HTTP is the technical underpinning of the web. So, when we talk about websites and web services, we mean that they use HTTP to communicate between a client (often a web browser) and a server.
