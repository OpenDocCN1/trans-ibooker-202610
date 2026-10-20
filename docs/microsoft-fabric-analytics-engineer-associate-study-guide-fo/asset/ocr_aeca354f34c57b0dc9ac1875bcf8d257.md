```
},
"queryPlanAsSteps": "
let
#"Sql.Database" = Sql.Database("<server address>", <database>,
[ CreateNavigationProperties = false,
EnableCrossDatabaseFolding = true ]),
#"Value.NativeQuery" = Value.NativeQuery(#"Sql.Database",
select [$Table].[DateID] as [DateID],
...
...
as [TotalAmount]
from [<database>].[dbo].[Trip] as [$Table])
in
#"Value.NativeQuery"",
"queryPlanStatus": "Succeeded",
"queryScript": "
let
Source = Sql.Databases("<server address>",
[CreateNavigationProperties=false,
EnableCrossDatabaseFolding=true]),
Database = Source{[Name = "<database>"]}[Data],
Table = Database{[Schema = "dbo", Item = "Trip"]}[Data]
In
Table
},
{
"queryName": "Geography",
"queryPlan": {
"nodes":
...
```

You may recognize the language used in this JSON object: Power Query. The let and in operators are the giveaways. This example shows the well-known Power Query engine making its way to the end user as the underlying query engine for this visual query editor. And if you want to, you can copy the Power Query statement, paste it to Dataflow Gen2, and continue your work there.