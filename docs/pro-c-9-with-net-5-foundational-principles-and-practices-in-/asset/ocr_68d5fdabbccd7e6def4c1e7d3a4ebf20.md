# Reverse Sort Records

The Reverse() method reverses the entire sort order, as demonstrated in the next test:

```
[Fact]
public void ShouldSortByFirstNameThenLastNameUsingReverse()
{
    //Sort by Last name then first name then reverse the sort
    var query = Context.Customers
        .OrderBy(x => x.PersonalInformation.LastName)
        .ThenBy(x => x.PersonalInformation.FirstName)
        .Reverse();
    var qs = query.ToQueryString();
    var customers = query.ToList();
    //if only one customer, nothing to test
    if (customers.Count <= 1) { return; }

    for (int x = 0; x < customers.Count - 1; x++)
    {
        var pi1 = customers[x].PersonalInformation;
        var pi2 = customers[x + 1].PersonalInformation;
        var compareLastName = string.Compare(pi1.LastName,
        pi2.LastName, StringComparison.CurrentCultureIgnoreCase);
        Assert.True(compareLastName >= 0);
        if (compareLastName != 0) continue;
        var compareFirstName = string.Compare(pi1.FirstName,
        pi2.FirstName, StringComparison.CurrentCultureIgnoreCase);
        Assert.True(compareFirstName >= 0);
    }
}
```

The preceding LINQ query gets translated into the following:

```
SELECT [c].[Id], [c].[TimeStamp], [c].[FirstName], [c].[FullName], [c].[LastName]
FROM [Dbo].[Customers] AS [c]
ORDER BY [c].[LastName] DESC, [c].[FirstName] DESC
```

# Retrieve a Single Record

There are three main methods for returning a single record with a query: First()/FirstOrDefault(), Last()/LastOrDefault(), and Single()/SingleOrDefault(). While all three return a single record, their approaches all differ. The three methods and their variants are detailed here:

+   - First() returns the first record that matches the query condition and any ordering clauses. If no ordering is specified, then the record returned is based on database order. If no record is returned, an exception is thrown.

+   - The FirstOrDefault() behavior matches First() except that if no records match the query, the method returns the default value for the type (null).