# Administering and Monitoring Power BI

+   2. Create a blank query called **PBIAuditLog** that returns the full file path based on the parameters; disable the load:

![](bbox=[0.201, 0.124, 0.866, 0.263])

Figure 10.22: PBIAuditLog file path query and other parameters in Query Editor

+   3. Create a new query called **O365PBIAuditLog**:

```
```powerquery
let
    Source = Csv.Document(File.Contents(PBIAuditLog),
        [Delimiter=",", Columns=13, Encoding=65001,
        QuoteStyle=QuoteStyle.Csv]),
    RemoveTopRows = Table.Skip(Source,1),
    PromoteHeaders = Table.PromoteHeaders(RemoveTopRows,
        [PromoteAllScalars=true]),
    ApplyDateType = Table.TransformColumnTypes(PromoteHeaders,
        {{"CreationDate", type datetime}}),
    AddCreationDateColumn = Table.AddColumn(ApplyDateType,
        "CreationDateOnly", each DateTime.Date([CreationDate]), type date),
    AddLocalTime = Table.AddColumn(AddCreationDateColumn, "LocalCreationDate",
        each [CreationDate] + #duration(0,USEasternDSTOffset,0,0)),
    AddLocalCreationDateColumn = Table.AddColumn(AddLocalTime,
        "LocalCreationDateOnly", each DateTime.Date([LocalCreationDate]), type date),
    ParseJSON = Table.TransformColumns(AddLocalCreationDateColumn,
        {{"AuditData", Json.Document}})
in
    ParseJSON
```
```

+   4. Add steps to expand the **Audit** column and set the appropriate data types for the expanded columns.

+   5. Create or reuse **Server** and **Database** parameters to create a query called **SQLAgentHistory** that retrieves the agent data:

![](bbox=[0.198, 0.817, 0.869, 0.868])

Figure 10.23: SQL Server Agent History view exposed in the "AdminProd" query