```
options) : base(options)
{
}

public DbSet<Research> ResearchLinks { get; set; }
}
```

Let's break it down as follows:

+   • First, we have our Research class, which is our entity object representation. As we mentioned in the Developing a database design section, for each link, we will save the URL, the date, and a note. The ID field is standard practice for a database table that holds information.
+   • Our second class, ResearchContext, is a subclass of DbContext. This class will have an empty constructor taking DbContextOptions as a parameter and a DbSet<TEntity> property for our data collection.

I could give you a brief overview about DbSet<TEntity> here, but I'd rather let Visual Studio help us out. Let's look at the following screen:

![](bbox=[0.1, 0.47, 0.9, 0.59])

If you hover over DbSet, you'll get an informational popup with everything you need to know!

# # Configuring the services

In the Startup.cs class, in the ConfigureServices method, add the DbContext service with the following code:

```
string connection =
Configuration.GetConnectionString("LocalDBConnection");
services.AddDbContext<ResearchContext>(options =>
options.UseSqlServer(connection));
```

As you can see, we set a connection string variable from the configuration and then pass that in as an options parameter for SqlServer for our DbContext.