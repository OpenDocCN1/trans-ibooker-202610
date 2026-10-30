
- Single() returns the first record that matches the query condition and any ordering clauses. If no ordering is specified, then the record returned is based on database order. If no records or more than one record matches the query, then an exception is thrown.
- The SingleOrDefault() behavior matches Single() except that if no records match the query, the method returns the default value for the type (null).
- Last() returns the last record that matches the query condition and any ordering clauses. If no ordering is specified, an exception is thrown. If no record is returned, an exception is thrown.
- The LastOrDefault() behavior matches Last() except that if no records match the query, the method returns the default value for the type (null).

All of the methods can also take an Expression<Func<T, bool>> (a lambda) to filter the result set. This means that you can place the Where() expression inside the call for the First()/Single() methods. The following statements are equivalent:

```
Context.Customers.Where(c=>c.Id < 5).First();
Context.Customers.First(c=>c.Id < 5);
```

Because of the immediate execution of the single record LINQ statements, the ToQueryString() method isn’t available. The listed query translations are provided by using SQL Server Profiler.

## Using First

When using the parameterless form of First() and FirstOrDefault(), the first record (based on database order or any preceding ordering clauses) will be returned.

The following test gets the first record based on database order:

```
[Fact]
public void GetFirstMatchingRecordDatabaseOrder()
{
    //Gets the first record, database order
    var customer = Context.Customers.First();
    Assert.Equal(1, customer.Id);
}
```

The preceding LINQ query gets translated into the following:

```
SELECT TOP(1) [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [dbo].[Customers] AS [c]
```

The following test gets the first record based on “last name, first name” order:

```
[Fact]
public void GetFirstMatchingRecordNameOrder()
{
    //Gets the first record, lastname, first name order
    var customer = Context.Customers
        .OrderBy(x => x.PersonalInformation.LastName)
        .ThenBy(x => x.PersonalInformation.FirstName)
        .First();
    Assert.Equal(1, customer.Id);
}
```

The preceding LINQ query gets translated into the following:

```
SELECT TOP(1) [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [dbo].[Customers] AS [c]
ORDER BY [c].[LastName], [c].[FirstName]
```

The following test asserts that an exception is thrown if there isn't a match when using First():

```
[Fact]
public void FirstShouldThrowExceptionIfNoneMatch()
{
    //Filters based on Id. Throws due to no match
    Assert.Throws<InvalidOperationException>(() => Context.Customers.First(x => x.Id == 10));
}
```

The preceding LINQ query gets translated into the following:

```
SELECT TOP(1) [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [dbo].[Customers] AS [c]
WHERE [c].[Id] = 10
```

Note Assert.Throws() is a special type of assert statement. It is expecting an exception to be thrown by the code in the expression. If an exception doesn't get thrown, the assertion fails.

When using FirstOrDefault(), instead of an exception, the result is a null record when no data is returned.

```
[Fact]
public void FirstOrDefaultShouldReturnDefaultIfNoneMatch()
{
    //Expression<Func<Customer, bool>> is a lambda expression
    Expression<Func<Customer, bool>> expression = x => x.Id == 10;
    //Returns null when nothing is found
    var customer = Context.Customers.FirstOrDefault(expression);
    Assert.Null(customer);
}
```

The preceding LINQ query gets translated into the same as the previous:

```
SELECT TOP(1) [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [dbo].[Customers] AS [c]
WHERE [c].[Id] = 10
```

## Using Last

When using the parameterless form of Last() and LastOrDefault(), the last record (based on any preceding ordering clauses) will be returned.

The following test gets the last record based on “last name, first name” order:

```
[Fact]
public void GetLastMatchingRecordNameOrder()
{
    //Gets the last record, lastname desc, first name desc order
    var customer = Context.Customers
        .OrderBy(x => x.PersonalInformation.LastName)
        .ThenBy(x => x.PersonalInformation.FirstName)
        .Last();
    Assert.Equal(4, customer.Id);
}
```

EF Core reverses the order by statements and then takes the top(1) to get the result. Here is the executed query:

```
SELECT TOP(1) [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [dbo].[Customers] AS [c]
ORDER BY [c].[LastName] DESC, [c].[FirstName] DESC
```

## Using Single

Conceptually, Single()/SingleOrDefault() works the same as First()/FirstOrDefault(). The main difference is that Single()/SingleOrDefault() returns Top(2) instead of Top(1) and throws an exception if two records are returned from the database.

The following test retrieves the single record where Id == 1:

```
[Fact]
public void GetOneMatchingRecordWithSingle()
{
    //Gets the first record, database order
    var customer = Context.Customers.Single(x => x.Id == 1);
    Assert.Equal(1, customer.Id);
}
```

The preceding LINQ query gets translated into the following:

```
SELECT TOP(2) [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [dbo].[Customers] AS [c]
WHERE [c].[Id] = 1
```

Single() throws an exception if no records are returned.

```
[Fact]
public void SingleShouldThrowExceptionIfNoneMatch()
{
    //Filters based on Id. Throws due to no match
    Assert.Throws<InvalidOperationException>(() => Context.Customers.Single(x => x.Id == 10));
}
```

The preceding LINQ query gets translated into the following:

```
SELECT TOP(2) [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [dbo].[Customers] AS [c]
WHERE [c].[Id] = 10
```

When using Single() or SingleOrDefault() and more than one record is returned, an exception is thrown.

```
[Fact]
public void SingleShouldThrowExceptionIfMoreThanOneMatch()
{
    // Throws due to more than one match
    Assert.Throws<InvalidOperationException>(() => Context.Customers.Single());
}
```

```
[Fact]
public void SingleOrDefaultShouldThrowExceptionIfMoreThanOneMatch()
{
    // Throws due to more than one match
    Assert.Throws<InvalidOperationException>(() => Context.Customers.SingleOrDefault());
}
```

The preceding LINQ queries get translated into the following:

```
SELECT TOP(2) [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [dbo].[Customers] AS [c]
```

When using SingleOrDefault(), instead of an exception, the result is a null record when no data is returned.

```
[Fact]
public void SingleOrDefaultShouldReturnDefaultIfNoneMatch()
{
    //Expression<Func<Customer, bool>> is a lambda expression
    Expression<Func<Customer, bool>> expression = x => x.Id == 10;
    //Returns null when nothing is found
    var customer = Context.Customers.SingleOrDefault(expression);
    Assert.Null(customer);
}
```

The preceding LINQ query gets translated into the following:

```
SELECT TOP(1) [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [dbo].[Customers] AS [c]
WHERE [c].[Id] = 10
```

## Global Query Filters

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

### Disable the Query Filters

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
