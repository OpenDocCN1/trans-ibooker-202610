# Global Query Filters

Recall that there is a global query filter on the Car entity to filter out any cars where IsDrivable is false.

```
modelBuilder.Entity<Car>(entity =>
{
    entity.HasQueryFilter(c => c.IsDrivable);
    ...
});
```

Open the CarTests.cs class and add the following test (all of the tests in the next sections are in the CarTests.cs class unless specifically mentioned otherwise):

```
[Fact]
public void ShouldReturnDrivableCarsWithQueryFilterSet()
{
    IQueryable<Car> query = Context.Cars;
    var qs = query.ToQueryString();
    var cars = query.ToList();
    Assert.NotEmpty(cars);
    Assert.Equal(9, cars.Count);
}
```

Also, recall that we create 10 cars in the data initialization process, and one of them is set to be nondrivable. When the query is executed, the global query filter is applied, and the following SQL is executed:

```
SELECT [i].[Id], [i].[Color], [i].[IsDrivable], [i].[MakeId], [i].[PetName], [i].[TimeStamp]
FROM [dbo].[Inventory] AS [i]
WHERE [i].[IsDrivable] = CAST(1 AS bit)
```

> Note Global query filters are also applied when loading related entities and when using FromSqlRaw() and FromSqlInterpolated(). These will be covered shortly.

# Disable the Query Filters

To disable global query filters for the entities in a query, add the IgnoreQueryFilters() method to the LINQ query. This disables all filters on all entities in the query. If there is more than one entity with a global query filter and some of the entities’ filters are required, they must be added to the LINQ statement’s Where() methods.

Add the following test to the CarTests.cs class, which disables the query filter and returns all records:

```
[Fact]
public void ShouldGetAllOfTheCars()
{
    IQueryable<Car> query = Context.Cars.IgnoreQueryFilters();
    var qs = query.ToQueryString();
    var cars = query.ToList();
    Assert.Equal(10, cars.Count);
}
```