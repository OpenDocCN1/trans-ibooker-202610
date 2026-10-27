# Getting ready

To prepare for this recipe, follow these steps:

+   1. Download `Employees.xlsx` from the following GitHub repository: https://github.com/PacktPublishing/Microsoft-Power-BI-Cookbook-Second-Edition.
+   
+   2. Open a **Power BI Desktop** file locally and access **Power Query Editor** by clicking on **Transform Data** in the ribbon of the **Home** tab.
+   
+   3. Create a query named AdWorksDW similar to the following:
+   
+   ```m
+   let
+       Source = Sql.Database("localhost\\MSSQLSERVERDEV", "AdventureWorksDW2019")
+   in
+       Source
+   ```
+   
+   4. Disable the load on the AdWorksDW query and place it in a Data Sources query group.
+   
+   5. Create an Employees query that references the AdWorksDW query and imports the DimEmployee table:
+   
+   ```m
+   let
+       Source = AdWorksDW,
+       dbo_DimEmployee = Source{[Schema="dbo",Item="DimEmployee"]}[Data],
+       FullName =
+           Table.CombineColumns(
+               dbo_DimEmployee,
+               {"FirstName", "LastName"},
+               Combiner.CombineTextByDelimiter(" ", QuoteStyle.None),
+               "EmployeeName"
+           )
+   in
+       FullName
+   ```
+   
+   6. Place the Employees query into a query group called Dimensions and disable the load.

# How to convert static queries

To implement this recipe, perform the following steps:

+   1. Create an M query called EmployeeKeysAdHoc in the **Other Queries** group that retrieves the employee IDs from the Excel workbook:
+   
+   ```m
+   let
+       Source = Excel.Workbook(File.Contents("C:\\Employees.xlsx"), null, true),
+       Employees_Sheet = Source{[Item="Employees",Kind="Sheet"]}[Data],
+   ```