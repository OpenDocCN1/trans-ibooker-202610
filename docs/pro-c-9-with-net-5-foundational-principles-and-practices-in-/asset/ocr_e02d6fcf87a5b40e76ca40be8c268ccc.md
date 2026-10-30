# Eager Loading with Split Queries

When a LINQ query contains a lot of includes, there can be a negative performance impact. To resolve this situation, EF Core 5 introduced split queries. Instead of executing a single query, EF Core will split the LINQ query into multiple SQL queries and then wire up all of the related data. For example, the previous query can be expected as multiple SQL queries by adding AsSplitQuery() into the LINQ query, like this:

```
var query = Context.Makes.AsSplitQuery()
    .Include(x => x.Cars.Where(x=>x.Color == "Yellow")).ToList();
```

The queries that are executed are shown here:

```
SELECT [m].[Id], [m].[Name], [m].[TimeStamp]
FROM [dbo].[Makes] AS [m]
ORDER BY [m].[Id]
```

```
SELECT [t].[Id], [t].[Color], [t].[MakeId], [t].[PetName], [t].[TimeStamp], [m].[Id]
FROM [dbo].[Makes] AS [m]
INNER JOIN (
    SELECT [i].[Id], [i].[Color], [i].[MakeId], [i].[PetName], [i].[TimeStamp]
    FROM [dbo].[Inventory] AS [i]
    WHERE [i].[Color] = N'Yellow'
) AS [t] ON [m].[Id] = [t].[MakeId]
ORDER BY [m].[Id]
```

There is a downside to using split queries: if the data changes between executing the queries, then the data returned will be inconsistent.

# Explicit Loading

Explicit loading is loading data along a navigation property after the core object is already loaded. This process involves executing an additional database call to get the related data. This can be useful if your application selectively needs to get the related records and not pull all of the related records perhaps based on some user action.

The process starts with an entity that is already loaded and using the Entry() method on the derived DbContext. When querying against a reference navigation property (e.g., getting the Make information for a car), use the Reference() method. When querying against a collection navigation property, use the Collection() method. The query is deferred until Load(), ToList(), or an aggregate function (e.g., Count(), Max()) is executed.

The following examples show how to get the related Make data as well as any Orders for a Car record:

```
//Get the Car record
var car = Context.Cars.First(x => x.Id == 1);
//Get the Make information
Context.Entry(car).Reference(c => c.MakeNavigation).Load();
//Get any orders the Car is related to
Context.Entry(car).Collection(c => c.Orders).Query().IgnoreQueryFilters().Load();
```