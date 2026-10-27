# Getting ready

To prepare for this recipe, follow these steps:

+   1. Open a Power BI Desktop file locally and access the **Power Query Editor** by clicking on **Transform Data** in the ribbon of the **Home** tab.
+   2. Create a query named **AdWorksDW**, similar to the following:
+   
+   ```m
+   let
+       Source = Sql.Database("localhost\\MSSQLSERVERDEV", "AdventureWorksDW2019")
+   in
+       Source
+   ```
+   
+   3. Disable load on the **AdWorksDW** query and place it into a **Data Sources** query group.
+   4. Right-click the **AdWorksDW** query and choose **Reference**.
+   5. Rename the query **Sales Territories**, choose the **DimSalesTerritory** table, and move to the **Other Queries** query group.
+   6. Create a **Products** query using the following code:
+   
+   ```m
+   let
+       Source = AdWorksDW,
+       Navigation = Source{[Schema="dbo",Item="DimProduct"]}[Data],
+       Subcategory =
+           Table.ExpandRecordColumn(
+               Navigation, "DimProductSubcategory",
+               {"EnglishProductSubcategoryName", "DimProductCategory"},
+               {"Product Subcategory", "DimProductCategory"}
+           ),
+       Category =
+           Table.ExpandRecordColumn(
+               Subcategory, "DimProductCategory",
+               {"EnglishProductCategoryName"}, {"Product Category"}
+           )
+   in
+       Category
+   ```
+   
+   7. Move the **Sales Territories** and **Products** queries to the **Other Queries** query group.
+   8. Select **Close & Apply** from the ribbon of the **Home** tab.
+   9. Save your work.