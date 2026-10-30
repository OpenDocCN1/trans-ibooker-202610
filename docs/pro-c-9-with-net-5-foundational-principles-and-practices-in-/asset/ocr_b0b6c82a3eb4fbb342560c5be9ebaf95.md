The generated query for the first InlineData test is listed here:

```
DECLARE @__color_0 nvarchar(4000) = N'Black';
SELECT [c].[Color], [c].[FirstName], [c].[IsDrivable], [c].[LastName], [c].[Make], [c].[PetName]
FROM [dbo].[CustomerOrderView] AS [c]
WHERE [c].[Color] = @__color_0
```

# Sort Records

The OrderBy() and OrderByDescending() methods set the sort(s) for the query, ascending and descending, respectively. If subsequent sorts are required, use the ThenBy() and ThenByDescending() methods. Sorting is shown in the following test:

```
[Fact]
public void ShouldSortByLastNameThenFirstName()
{
    //Sort by Last name then first name
    var query = Context.Customers
        .OrderBy(x => x.PersonalInformation.LastName)
        .ThenBy(x => x.PersonalInformation.FirstName);
    var qs = query.ToQueryString();
    var customers = query.ToList();
    //if only one customer, nothing to test
    if (customers.Count <= 1) { return; }
    for (int x = 0; x < customers.Count - 1; x++)
    {
        var pi = customers[x].PersonalInformation;
        var pi2 = customers[x + 1].PersonalInformation;
        var compareLastName = string.Compare(pi.LastName,
            pi2.LastName, StringComparison.CurrentCultureIgnoreCase);
        Assert.True(compareLastName <= 0);
        if (compareLastName != 0) continue;
        var compareFirstName = string.Compare(pi.FirstName,
            pi2.FirstName, StringComparison.CurrentCultureIgnoreCase);
        Assert.True(compareFirstName <= 0);
    }
}
```

The preceding LINQ query gets translated into the following:

```
SELECT [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [Dbo].[Customers] AS [c]
ORDER BY [c].[LastName], [c].[FirstName]
```