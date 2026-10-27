11. Click on **Apply Changes** from the warning dialog. StartDate and the fact table queries impacted by the parameter change will both be refreshed.

# How it works...

In a production scenario, the CurrentDate query would not be hardcoded to a date and would be something similar to the following:

```
```powerquery
let
    Source = DateTime.Date(DateTime.LocalNow())
in
    Source
```
```

The StartDate query uses the parameter to calculate a date prior to the CurrentDate by a number of days selected from the list of values. Parameters can be referenced within queries, just like referencing other queries or steps in a query, in which the same rules also apply. When parameters include spaces in their names, referencing the parameter must be preceded by a hashbag, #, and double quotes, and suffixed by double quotes (#"Days Prior to Current Date"). Parameters that do not include spaces in their names can be referenced directly without the prefix or suffix.

The Internet Sales query uses the Table.SelectRows function in the RowFilter step to filter the Fact.InternetSales table using the OrderDate column to be greater than or equal to StartDate (a date value). It is important that the data type of the OrderDate column and the data type of the StartDate query match one another. For example, Date data types cannot be compared to DateTime data types.

Currently, parameters cannot be created or edited in the Power BI service. The parameter values configured when published will be used for each refresh.

When considering your own scenarios, identify candidates for query parameters, such as hardcoded date filters and dimension attributes with few distinct values (for example, department groups). Identify scenarios in which certain business users or teams require edit rights to a dataset (that is, source queries, model relationships, and measures), but only need a small, highly filtered model for self-service development.

# There's more...

Queries can be affected by multiple parameters. In addition, parameters can also use queries for their Suggested Values. To see how to use multiple parameters in a query, as well as how to use a query to provide Suggested Values for a parameter, do the following:

+   1. In **Power Query Editor**, create a query called **SalesTerritoryGroupsList** that returns the distinct **Sales Territory Groups** from the **DimSalesTerritory** table and disable loading of the query: