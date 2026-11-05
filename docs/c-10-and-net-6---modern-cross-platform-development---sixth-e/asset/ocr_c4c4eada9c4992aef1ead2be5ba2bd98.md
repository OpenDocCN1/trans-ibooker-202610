1. Open the Program.cs file and note that it uses the top-level program feature (so there is a hidden Program class with a Main method). This file can be considered to be divided into four important sections from top to bottom.

> .NET 5 and earlier ASP.NET Core project templates used a Startup class to separate these parts into separate methods but with .NET 6, Microsoft encourages putting everything in a single Program.cs file.

2. The first section imports some namespaces, as shown in the following code:

```
using Microsoft.AspNetCore.Identity; // IdentityUser
using Microsoft.EntityFrameworkCore; // UseSqlServer, UseSqlite
using Northwind.Mvc.Data; // ApplicationDbContext
```

> Remember that by default, many other namespaces are imported using the implicit usings feature of .NET 6 and later. Build the project and then the globally imported namespaces can be found in the following path: obj\Debug\net6.0\Northwind.Mvc.GlobalUsings.g.cs.

3. The second section creates and configures a web host builder. It registers an application database context using SQL Server or SQLite with its database connection string loaded from the appsettings.json file for its data storage, adds ASP.NET Core Identity for authentication and configures it to use the application database, and adds support for MVC controllers with views, as shown in the following code:

```
var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
var connectionString = builder.Configuration
    .GetConnectionString("DefaultConnection");

builder.Services.AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(connectionString)); // or UseSqlite

builder.Services.AddDatabaseDeveloperPageExceptionFilter();

builder.Services.AddDefaultIdentity<IdentityUser>(options =>
    options.SignIn.RequireConfirmedAccount = true)
    .AddEntityFrameworkStores<ApplicationDbContext>();

builder.Services.AddControllersWithViews();
```