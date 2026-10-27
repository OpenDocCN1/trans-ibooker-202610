# Getting ready

To prepare for this recipe, we will create a query from a database, which will serve as the source for other queries via the standard Get Data and Power Query Editor experience described in the previous recipe. To create this query, perform the following steps:

+   1. Open Power BI Desktop.
+   2. If you have already connected to your SQL Server, you can find the connection under Recent sources on the Home tab. Otherwise, on the Home tab, select Get Data from the ribbon, and choose SQL Server.
+   3. Select a table or view, and click on Transform Data to import the data.
+   4. The Power Query Editor window will launch and a preview of the data will appear. In this example, we have chosen the DimEmployee table from the AdventureWorksDW2019 database on our local SQL Server instance MSSQLSERVERDEV. The full code of the query can be viewed in the Advanced Editor window but is also shown below.
+   5. Copy just the Source line (in bold in the previous step).
+   6. Close the Advanced Editor window by clicking the Cancel button.
+   7. Remain in the Power Query Editor window.

```
let
    Source = Sql.Database("localhost\MSSQLSERVERDEV",
    "AdventureWorksDW2019"),
    dbo_DimEmployee = Source[{Schema="dbo",Item="DimEmployee"}][Data]
in
    dbo_DimEmployee
```

# How to Manage Queries and Data Sources

In this example, a separate data source connection query is created and utilized by individual queries. By associating many individual queries with a single (or a few) data source queries, it is easy to change the source system or environment, such as when switching from a Development environment to a User Acceptance Testing (UAT) environment. We will then further separate out our data source queries and our data load queries using query groups. To start isolating our data source queries from our data load queries, follow these steps:

+   1. Create a new, blank query by selecting New Source from the ribbon of the Home tab and then select Blank Query.