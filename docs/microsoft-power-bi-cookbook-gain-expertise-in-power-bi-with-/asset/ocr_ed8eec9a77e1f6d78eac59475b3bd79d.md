# Configuring Model Metadata

Metadata is information about information. There are many metadata settings in Power BI data models, such as column names, data types, object descriptions, and others. Four important metadata properties to configure for any column visible on the Power BI report canvas are the format, data category, default summarization, and the option to sort by column. The data formats should be consistent across data types and efficiently convey the appropriate level of detail. Data categories serve to enhance the data exploration experience by providing Power BI with information to guide its visualizations. Default summarization determines the aggregation, if any, to apply to the column when added to the report canvas. The ability to **Sort by Column** enables the displaying of the values in a column based on the order of a separate column. Although all of these settings are relatively simple to configure, careful attention to these properties helps to deliver higher-quality Power BI visualizations and a more user-friendly platform for self-service.

In this recipe, we demonstrate how to configure the format, data category, default summarization, and the **Sort by Column** option for columns and measures within a data model—and show the impact of these settings on visualizations.

# Getting ready

To prepare for this recipe, follow these steps:

+   1. Open Power BI Desktop.
+   2. Create an Import mode data source query called AdWorksDW. This query should be similar to the following:
+   ```
+   let
+       Source = Sql.Database("localhost\MSSQLSERVERDEV", "AdventureWorksDW2019")
+   in
+       Source
+   ```
+   3. Isolate this query in a query group called **Data Sources** and disable loading.
+   4. Right-click AdWorksDW and choose **Reference**.
+   5. Select the **FactFinance** table and name this query **General Ledger**.
+   6. Repeat steps 4 and 5 to create a **Dates** query that imports **DimDate** and a **Departments** query that imports **DimDepartmentGroup**.

```
let
    Source = Sql.Database("localhost\MSSQLSERVERDEV", "AdventureWorksDW2019")
in
    Source
```