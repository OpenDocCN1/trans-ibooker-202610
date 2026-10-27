# See also

+   - Specify data categories in Power BI Desktop: http://bit.ly/2peNqPm
+   - Tag barcode fields to enable barcode-scan filtering in mobile apps: http://bit.ly/3re4dAV
+   - Add hyperlinks (URLs) to a table or matrix: http://bit.ly/39dxJ3r
+   - Display images in a table or matrix in a report: http://bit.ly/2Pkx1dE
+   - Sort by column in Power BI Desktop: http://bit.ly/2pFXhgh
+   - Chelsie Eiden's Duration: http://bit.ly/31KFUtf
+   - Custom format strings: http://bit.ly/3sj648B
+   - Use custom format strings in Power BI Desktop: http://bit.ly/3cXAW8o

# Hiding Columns and Tables

Data models must balance the competing demands of functionality, scope, and usability. As additional tables, columns, measures, and other structures are added to meet various analytical needs, a model can quickly become confusing to end users. Therefore, it is important to minimize the visibility of columns and tables to provide an intuitive interface.

In this recipe, we demonstrate how to hide columns and even entire tables from report consumers.

# Getting ready

To prepare for this recipe, follow these steps:

+   1. Open Power BI Desktop.
+   2. Create an Import mode data source query called AdWorksDW. This query should be similar to the following:
+   
+      ```
+      let
+          Source = Sql.Database("localhost\\MSSQLSERVERDEV", "AdventureWorksDW2019")
+      in
+          Source
+      ```
+   3. Isolate this query in a query group called **Data Sources** and disable loading.
+   4. Right-click AdWorksDW and choose **Reference**.
+   5. Select the **FactInternetSales** table and name this query **Internet Sales**.
+   6. Repeat steps 4 and 5 to create a **Departments** query that imports **DimDepartmentGroup** and an **Accounts** query that imports **DimAccount**.

```
```text
let
    Source = Sql.Database("localhost\\MSSQLSERVERDEV", "AdventureWorksDW2019")
in
    Source
```
```