

- Single() 返回与查询条件和任何排序子句匹配的第一条记录。如果未指定排序，则返回的记录基于数据库顺序。如果没有记录或匹配的记录多于一条，则抛出异常。
- SingleOrDefault() 的行为与 Single() 匹配，只是如果没有记录匹配查询，方法会返回该类型的默认值（null）。
- Last() 返回与查询条件和任何排序子句匹配的最后一条记录。如果未指定排序，则抛出异常。如果没有记录返回，则抛出异常。
- LastOrDefault() 的行为与 Last() 匹配，只是如果没有记录匹配查询，方法会返回该类型的默认值（null）。

所有方法也可以接受一个 Expression<Func<T, bool>>（lambda）来过滤结果集。这意味着你可以将 Where() 表达式放在 First()/Single() 方法的调用中。以下语句是等价的：

```
Context.Customers.Where(c=>c.Id < 5).First();
Context.Customers.First(c=>c.Id < 5);
```

由于单记录 LINQ 语句的立即执行，ToQueryString() 方法不可用。所列出的查询转换是通过使用 SQL Server Profiler 提供的。

## Using First

使用 First() 和 FirstOrDefault() 的无参数形式时，将返回第一条记录（基于数据库顺序或任何之前的排序子句）。

以下测试基于数据库顺序获取第一条记录：

```
[Fact]
public void GetFirstMatchingRecordDatabaseOrder()
{
    //Gets the first record, database order
    var customer = Context.Customers.First();
    Assert.Equal(1, customer.Id);
}
```

前面的 LINQ 查询被转换为以下内容：

```
SELECT TOP(1) [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [dbo].[Customers] AS [c]
```

以下测试基于“姓氏、名字”顺序获取第一条记录：

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

前面的 LINQ 查询被转换为以下内容：

```
SELECT TOP(1) [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [dbo].[Customers] AS [c]
ORDER BY [c].[LastName], [c].[FirstName]
```

以下测试断言如果使用 First() 时没有匹配项，则会抛出异常：

```
[Fact]
public void FirstShouldThrowExceptionIfNoneMatch()
{
    //Filters based on Id. Throws due to no match
    Assert.Throws<InvalidOperationException>(() => Context.Customers.First(x => x.Id == 10));
}
```

前面的 LINQ 查询被转换为以下内容：

```
SELECT TOP(1) [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [dbo].[Customers] AS [c]
WHERE [c].[Id] = 10
```

注意 Assert.Throws() 是一种特殊的断言语句。它期望代码在表达式中抛出异常。如果没有抛出异常，断言将失败。

使用 FirstOrDefault() 时，如果没有返回数据，结果不是异常，而是一个 null 记录。

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

前面的 LINQ 查询被转换为与之前相同的内容：

```
SELECT TOP(1) [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [dbo].[Customers] AS [c]
WHERE [c].[Id] = 10
```

## Using Last

使用 Last() 和 LastOrDefault() 的无参数形式时，将返回最后一条记录（基于任何之前的排序子句）。

以下测试基于“姓氏、名字”顺序获取最后一条记录：

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

EF Core 会反转 order by 子句，然后取 top(1) 来获取结果。以下是执行的查询：

```
SELECT TOP(1) [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [dbo].[Customers] AS [c]
ORDER BY [c].[LastName] DESC, [c].[FirstName] DESC
```

## Using Single

概念上，Single()/SingleOrDefault() 与 First()/FirstOrDefault() 相同。主要区别是 Single()/SingleOrDefault() 返回 Top(2) 而不是 Top(1)，并且如果数据库返回两条记录则抛出异常。

以下测试检索 Id == 1 的单条记录：

```
[Fact]
public void GetOneMatchingRecordWithSingle()
{
    //Gets the first record, database order
    var customer = Context.Customers.Single(x => x.Id == 1);
    Assert.Equal(1, customer.Id);
}
```

前面的 LINQ 查询被转换为以下内容：

```
SELECT TOP(2) [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [dbo].[Customers] AS [c]
WHERE [c].[Id] = 1
```

如果没有记录返回，Single() 会抛出异常。

```
[Fact]
public void SingleShouldThrowExceptionIfNoneMatch()
{
    //Filters based on Id. Throws due to no match
    Assert.Throws<InvalidOperationException>(() => Context.Customers.Single(x => x.Id == 10));
}
```

前面的 LINQ 查询被转换为以下内容：

```
SELECT TOP(2) [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [dbo].[Customers] AS [c]
WHERE [c].[Id] = 10
```

使用 Single() 或 SingleOrDefault() 且返回多于一条记录时，会抛出异常。

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

前面的 LINQ 查询被转换为以下内容：

```
SELECT TOP(2) [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [dbo].[Customers] AS [c]
```

使用 SingleOrDefault() 时，如果没有数据返回，结果不是异常，而是一个 null 记录。

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

前面的 LINQ 查询被转换为以下内容：

```
SELECT TOP(1) [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [dbo].[Customers] AS [c]
WHERE [c].[Id] = 10
```

## Global Query Filters

回想一下，Car 实体上有一个全局查询过滤器，用于过滤掉 IsDrivable 为 false 的汽车。

```
modelBuilder.Entity<Car>(entity =>
{
    entity.HasQueryFilter(c => c.IsDrivable);
    ...
});
```

打开 CarTests.cs 类并添加以下测试（除非另有具体说明，否则下一节中的所有测试都在 CarTests.cs 类中）：

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

此外，回想一下我们在数据初始化过程中创建了 10 辆汽车，其中一辆被设置为不可驾驶。当查询执行时，全局查询过滤器会被应用，并执行以下 SQL：

```
SELECT [i].[Id], [i].[Color], [i].[IsDrivable], [i].[MakeId], [i].[PetName], [i].[TimeStamp]
FROM [dbo].[Inventory] AS [i]
WHERE [i].[IsDrivable] = CAST(1 AS bit)
```

> 注意 全局查询过滤器在加载相关实体以及使用 FromSqlRaw() 和 FromSqlInterpolated() 时也会被应用。这些内容将在稍后介绍。

### Disable the Query Filters

要为查询中的实体禁用全局查询过滤器，请将 IgnoreQueryFilters() 方法添加到 LINQ 查询中。这会禁用查询中所有实体的所有过滤器。如果有多个实体具有全局查询过滤器，并且某些实体的过滤器是必需的，则必须将它们添加到 LINQ 语句的 Where() 方法中。

将以下测试添加到 CarTests.cs 类，该测试禁用查询过滤器并返回所有记录：

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

