# Combining and Merging Queries

The full power of Power BI's querying capabilities is in the integration of distinct queries representing different data sources via its merge and append transformations. Retrieval processes that consolidate files from multiple network locations or integrate data from multiple data sources can be developed efficiently and securely. Additionally, the same join types and data transformation patterns SQL and ETL developers are familiar with can be achieved with the M language. This recipe provides examples of combining sources into a single query and leveraging the table join functions of M to support common transformation scenarios.

# Getting ready

To follow along with this recipe, you can use the Merge Queries and Append Queries icons on the Home tab of the Power Query Editor to generate the join expressions used in this recipe. However, as joining queries is fundamental to the retrieval process, it is recommended to learn how to use the Table.Join, Table.NestedJoin, and Table.Combine functions.

To get ready for this recipe, import the DimCustomer and FactCallCenter tables from the AdventureWorksDW2019 database by doing the following:

+   1. Open Power BI Desktop and choose Transform data from the ribbon of the Home tab to open the Power Query Editor.
+   2. Create an Import mode data source query called AdWorksDW. This query should be similar to the following:
+   3. Isolate this query in a query group called Data Sources.
+   4. Right-click AdworksDW and choose Reference, select the DimEmployee table in the data preview area, and rename this query DimEmployee.
+   5. Repeat step 4 but choose the FactInternetSales table and name this query Sales2011. Filter the OrderDate column to be between January 1, 2011 and December 31, 2011.

```
let
    Source = Sql.Database("localhost\MSSQLSERVERDEV", "AdventureWorksDW2019")
in
    Source
```

```
Let
    Source = AdWorksDW,
    dbo_FactInternetSales = Source{[Schema="dbo",Item="FactInternetSales"]}[Data],
    #"Filtered Rows" =
        Table.SelectRows(
```