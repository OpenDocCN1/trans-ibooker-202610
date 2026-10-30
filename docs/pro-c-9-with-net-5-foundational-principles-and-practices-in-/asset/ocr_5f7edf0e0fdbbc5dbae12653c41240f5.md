```
[InlineData(3, 1)]
[InlineData(4, 2)]
[InlineData(5, 3)]
[InlineData(6, 1)]
public void ShouldGetTheCarsByMakeUsingFromSql(int makeId, int expectedCount)
{
    var entity = Context.Model.FindEntityType($"{typeof(Car).FullName}");
    var tableName = entity.GetTableName();
    var schemaName = entity.GetSchema();
    var cars = Context.Cars.FromSqlRaw($"Select * from {schemaName}.{tableName}")
        .Where(x => x.MakeId == makeId).ToList();
    Assert.Equal(expectedCount, cars.Count);
}
```

There are some rules when using the FromSqlRaw()/FromSqlInterpolated() methods: the columns returned from the SQL statement have to match the mapped columns on the model, all columns for the model must be returned, and related data cannot be returned.

# Aggregate Methods

EF Core also supports server-side aggregate methods (Max(), Min(), Count(), Average(), etc.). Aggregate methods can be added to the end of a LINQ query with Where() methods, or the filter expression can be contained in the aggregate method itself (just like First() and Single()). The aggregation executes on the server side, and the single value is returned from the query. Global query filters affect aggregate methods as well and can be disabled with IgnoreQueryFilters().

All of the SQL statements shown in this section were collected using SQL Server Profiler.

This first test (in CarTests.cs) simply counts all of the Car records in the database. Since the query filter is still active, the count returns nine cars.

```
[Fact]
public void ShouldGetTheCountOfCars()
{
    var count = Context.Cars.Count();
    Assert.Equal(9, count);
}
```

The executed SQL is shown here:SELECT COUNT(*)
FROM [dbo].[Inventory] AS [i]
WHERE [i].[IsDrivable] = CAST(1 AS bit)

By adding IgnoreQueryFilters(), the Count() method returns 10, and the where clause is removed from the SQL query.

```
[Fact]
public void ShouldGetTheCountOfCarsIgnoreQueryFilters()
{
    var count = Context.Cars.IgnoreQueryFilters().Count();
    Assert.Equal(10, count);
}
```

```
--Generated SQL
SELECT COUNT(*) FROM [dbo].[Inventory] AS [i]
```