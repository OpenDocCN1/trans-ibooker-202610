in
IndexColumnAdded

+   7. Disable loading on BaseDateTable and DatesInterim by right-clicking the queries and deselecting Enable Load. Only the Dates query needs to be loaded into the data model.

# How it works...

For the DatesInterim query, the CurrentDate, CurrentMonth, and CurrentYear expressions can be stored as separate queries. The DateTime.LocalNow function is used in dynamic M date logic, similar to the CURRENT_TIMESTAMP function used in SQL statements. A filter is applied via the Table.SelectRows function to only retrieve the last three years of dates, and conditional logic is used to populate dynamic Year Status and Month Status columns (for example, Current Year). The rest of the expressions use functions of the Date object class or concatenate columns.

Despite a minimal date table available in the source system (all that is required is a column containing dates), the M query generates a useful date dimension table for a model with many of the most common and important columns.

For the final Dates query, two additional columns (Calendar Year and Calendar Month Number) are used, although we could have performed a self-joining query with the Year and Month Number columns in the DatesInterim query. For example, if we rename the Dates query to DatesInterim2, we can create the final Dates table that adds a sequential index for Year Week Number as well:

```
let
    Source = DatesInterim2,
    YearMonthWeekCols =
        Table.Distinct(
            Table.SelectColumns(
                Source, {"Year", "Month Number", "Week of Year"}
            )
        ),
    YearMonthWeekColSort =
        Table.Sort(
            YearMonthWeekCols,
            {
                {"Year", Order.Ascending},
                {"Month Number", Order.Ascending},
                {"Week of Year", Order.Ascending}
            }
        )
```