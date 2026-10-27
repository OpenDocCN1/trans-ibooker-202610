+   2. Create an Import mode data source query called AdWorksDW. This query should be similar to the following:
+   3. Isolate this query in a query group called Data Sources.
+   4. Right-click AdWorksDW and choose Reference, select the FactFinance table in the data preview area, and rename this query FactFinance.
+   5. Move the FactFinance query to the Other Queries group.

```
let
    Source = Sql.Database("localhost\MSSQLSERVERDEV",
    "AdventureWorksDW2019")
in
    Source
```

For additional details on performing these steps, see the Managing Queries and Data Sources recipe in this chapter.

# How to Select Column Data Types

To implement this recipe, perform the following steps:

+   1. Use the Table.TransformColumnTypes function to revise the type of the numeric integer FinanceKey column to text. Note that the column header for the FinanceKey column is initially prefaced with a 123 icon and changes to an ABC icon. Also, note that the row values are initially left justified and italicized and after transformation to text are aligned to the right and not italicized.
+   2. Add a numeric column from a source column stored as text using the Number.FromText and Table.AddColumn functions.

```
let
    Source = AdWorksDW,
    dbo_FactFinance = Source[{Schema="dbo",Item="FactFinance"}][Data],
    FinanceKeyText =
        Table.TransformColumnTypes(
            dbo_FactFinance, {{"FinanceKey", Text.Type}}
        )
in
    FinanceKeyText
```

```
FinanceKeyNum =
    Table.AddColumn(
        FinanceKeyText, "FinanceKeyNum",
        each Number.FromText([FinanceKey]),Int64.Type
    )
```