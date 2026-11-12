
- 3. You can have Visual Studio create your controller for you but, again, let's roll our own and pick **Empty**:

![](img/762492a8287f369cc1ac4621a212f193_269_0.png)

The new controller inherits from `ControllerBase`. This is because the `Controller` class (which itself inherits from `ControllerBase`) adds some functionality for binding views that relates only to an MVC controller. You could change this to inherit from `Controller` and it would work fine (although you would be including some functionality that you won't need). You'll notice that the new controller is decorated as an `ApiController`. Again, this is optional, but it adds some basic validation for you.

Your initial `controller` method should, therefore, look like this:

```csharp
[Route("api/[controller]")]
[ApiController]
public class StockController : ControllerBase
{
}
```

- 4. So that we can get it working, let's add some very basic code to have it return something:

```csharp
[HttpGet]
public string Get()
{
    return "test";
}
```

- 5. Now, we need to plug in some middleware for the controller to be found. In startup.cs, change the ConfigureServices method, as follows:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllers();
}
```

This is an extension method that tells the framework that you will be using services related to controller and controller mapping for the web API, so it will add authorization and CORS, for example.

![](img/762492a8287f369cc1ac4621a212f193_270_0.png)

In previous versions of .NET Core, you would add .UseMvc or .UseMvcCore here; however, in .NET Core 3, the framework expects you to add only the parts that you need. This means that your code will never include middleware that isn't necessary. Clearly, the downside is that there's slightly more work to do initially to set up the API.

- 6. In the Configure method, tell the application to use the services that we added earlier:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    app.UseRouting();
    app.UseAuthorization();
    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });
}
```

- 7. Now, you should be able to run the application and navigate to the controller; the exact address of this depends on your port, but it may, for example, be as follows:

https://localhost:44371/api/stock

Now that we have a controller, we can create our stock functionality. We have two requirements: checking stock levels and updating stock levels. We’ll need to persist this information as well, so we’ll use EF Core.

## Reading stock levels

Let’s look at how we might persist this to a SQL Server database using Entity Framework Core:

- 1. Because we’re persisting all this to a database, we’ll install Entity Framework Core in our API:

```
Install-Package Microsoft.EntityFrameworkCore.SqlServer
Install-Package Microsoft.EntityFrameworkCore.Tools
```

This installs both the Entity Framework libraries and tools that are required.

- 2. The next step is to create our model; that is, create a map of the database in a C# class. Your model could look something like this:

```csharp
public class Product
{
    public int Id { get; set; }
    public string Description { get; set; }
    public int StockCount { get; set; }
}
```

It should sit somewhere visible to your application. I’ve added mine to a subfolder called Models.

> **TIP**: For larger applications, it can make sense to take all of the data access logic and move it to its own library, but since this is a very small project, the extra overhead is probably not warranted at this time. If you maintain an abstraction between the data access and the business logic, moving the code later should be a trivial task. By default, Entity Framework Core uses any fields suffixed with `Id` to create a primary key. In our case, `Id` will be treated as a primary key.

Database performance, indexes, and keys are beyond the scope of this chapter and book; however, if you decide to extend this project, it is very likely that, for any volume of data, you would need to consider such things.

- 3. The next step is to create a data context. This is effectively a mapping class to tell EF Core which classes to use and map to your database. Ours needs to look like this at present:

```csharp
public class StockContext : DbContext
{
    public StockContext(DbContextOptions<StockContext> options)
        : base(options) { }

    public DbSet<Product> Products { get; set; }
}
```

There are two important parts to this class: declaring `DbSet`, which tells Entity Framework Core what we'd like to persist, and what it inherits from, that is, `DbContext` (its constructor).

- 4. Finally, we need to tell ASP.NET Core that we want to use Entity Framework Core, where to find `DbContext`, and where to find the database. This is all located inside `ConfigureServices`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvcCore();

    var connection =
        @"Server=(localdb)\MSSQLLocalDb;Database=StockCheckerDB;Trusted_Connection=True;ConnectRetryCount=0";
    services.AddDbContext<StockContext>(options =>
    options.UseSqlServer(connection));
}
```

> > You may wish to move the connection string into the config file and use SQL Server security rather than a trusted connection.

- 5. Now that we've configured our data, we'll create a migration to update the database to reflect our model. In the Package Manager Console, type the following:

```
Add-Migration InitialMigration
```

This should create a new folder, called Migrations, and two new files in your project. If you have a quick look at the migration file, you'll see that it consists of two functions: Up, which tells EF what to do when you migrate forward, and Down, which should revert those changes.

> > Although this is generated code, it is not continually generated, which means that you can change it if you wish. Be aware that if you change Up, but not Down, you may find that you can't revert a migration, or worse, that reverting the migration leaves you in a new state: neither new nor old.

- 6. The next step is to update the database (which just runs the migration):

```
Update-Database
```

Now that our database exists, is accessible, and is up to date, we'll need our controller function to access that data.

You could simply access the DataContext from anywhere in the application; however, this would cause problems for unit testing; that is, if your controller function directly accesses the database, it's difficult to test a unit of functionality. Further, if you decided to replace Entity Framework with another data access method at a later time, this would make it much more difficult.

In order to solve these issues, we'll inject the dependency into our controller. Let's see how:

- 1. In order to inject the dependency into our controller, the first step is to abstract our DbContext class into an interface:

```csharp
public interface IDbContext
{
    DbSet<Product> Products { get; set; }
}
```

You might be wondering why we would create an interface and not pass the class in directly. In fact, there is nothing preventing this; however, what this would mean is that we would always need to pass in a class of the DbContext type. Creating an interface means that we can replace our DbContext class with a dummy class, or even a completely different class that implements the same interface.

This may seem like abstraction for the sake of it, but consider how you would write a unit test for any method that referenced this DbContext.

For our project, we will simply pass the DbContext around; however, the best practice is to completely abstract the data access, so rather than passing in DbContext, you may pass in an IDataAccess class, which in turn accepts the IDbContext. This means that, should you decide to replace EF Core with another ORM, you would simply change the implementation of this class.

- 2. Now that we have an interface, we can inject that into our controller:
  `public StockController(IDbContext dbContext)`

If you’re using Visual Studio, pressing Ctrl+. on DbContext will give you the opportunity to create and populate a field in the class, saving you from adding the class-level variable:

![](img/762492a8287f369cc1ac4621a212f193_274_0.png)

- 3. Finally, we’ll return data from the database when the Get method is called:
  `[HttpGet("{id}")]`
  `public ActionResult<int> Get(int id)`
