```
if [Title] = null and [MiddleName] = null
    then [FirstName] & " " & [LastName]
    else if [Title] = null
        then [FirstName] & " " & Text.Range([MiddleName],0,1) & ". " & [LastName]
    else
        [Title] & " " & [FirstName] & " " & Text.Range([MiddleName],0,1) & ". " & [LastName]
)
```

+   3. Add variables that allow the expression to support the comparison between the current system date and the BirthDate column.

```
let
    CurrentDate = DateTime.Date(DateTime.LocalNow()),
    CurrentYear = Date.Year(CurrentDate),
    CurrentMonth = Date.Month(CurrentDate),
    CurrentDay = Date.Day(CurrentDate),

    Source = AdWorksDw,
```

+   4. Use the Table.AddColumn function to create Customer Year, Customer Month, and Customer Day columns based upon the BirthDate column.

```
AddCustomerYear =
    Table.AddColumn(
        NameFormatTble, "Customer Year", each Date.Year([BirthDate]),
        Int64.Type
    ),
    AddCustomerMonth =
        Table.AddColumn(
            AddCustomerYear, "Customer Month", each Date.Month([BirthDate]),
            Int64.Type
        ),
    AddCustomerDay =
        Table.AddColumn(
            AddCustomerMonth, "Customer Day", each Date.Day([BirthDate]),
            Int64.Type
        )
```