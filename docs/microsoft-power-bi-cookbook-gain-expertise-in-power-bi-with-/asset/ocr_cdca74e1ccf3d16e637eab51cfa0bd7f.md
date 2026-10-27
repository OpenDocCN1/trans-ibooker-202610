```
),
YearMonthWeekColIndex =
    Table.AddIndexColumn(YearMonthWeekColSort, "YearMonthWeekIndex",1,1),
JoinedDateTable =
    Table.NestedJoin(
        DatesInterim2,{"Year", "Month Number", "Week of Year"},
        YearMonthWeekColIndex, {"Year", "Month Number", "Week of Year"},
        "Year-Mo-Wk Index", JoinKind.Inner
    ),
IndexColumnAdded =
    Table.ExpandTableColumn(
        JoinedDateTable,
        "Year-Mo-Wk Index", {"YearMonthWeekIndex"},{"Year Month Week Number"}
    )
in
    IndexColumnAdded
```

The Table.AddIndexColumn function is applied to the sorted DatesInterim2 table to create the sequential column, YearMonthWeekColIndex. The DatesInterim2 query is then joined to the table containing this new column. Finally, the joined table is expanded using the Table.ExpandTableColumn function.

The Year-Mo column (that is, 2016-Dec) can now be sorted by the Year Month Number column in the data model and DAX measures can reference the Year Month Number column to apply date intelligence filter conditions, such as the trailing six months.

# There's more...

The Table.AddIndexColumn function used in this recipe is not currently supported in DirectQuery mode for SQL Server, and thus the full M query from the date dimension M query section of this recipe cannot be used in DirectQuery data models.

Since the data access queries defining the dimension and fact tables in DirectQuery models are executed at runtime, these queries should be as simple as possible. For example, these queries should avoid joins, derived tables, subqueries, data type conversions, case statements, and so on. Simple, performant, or optimized queries are especially important for the queries used to access the largest tables of the model. To ensure sufficient performance in Power BI with DirectQuery models, large source tables of DirectQuery models should be optimized for read performance with features such as the columnstore index of SQL Server and table partitions.