+   3. Disable load on the AdWorksDW query and place it into a **Data Sources** query group
+   4. Right-click the AdWorksDW query and choose **Reference**
+   5. Rename the query **Internet Sales** and choose the **FactInternetSales** table
+   6. Repeat steps 4 and 5, creating a **Dates** query that points to the **DimDates** table, a **Customers** query that points to the **DimCustomer** table, and a **Sales Territories** query that points to the **DimSalesTerritory** table
+   7. Create a **Products** query using the following code:
+   8. Move the **Internet Sales** query to a **Facts** query group, and the **Dates**, **Customers**, and **Products** queries to a **Dimensions** query group
+   9. In the **Other Queries** group, create a blank query called **Calculations**
+   10. Select **Close & Apply** from the ribbon of the **Home** tab
+   11. Switch to the **Model** view and create a relationship between the **DateKey** column in the **Dates** table, and the **OrderDateKey** in the **Internet Sales** table
+   12. Create a relationship between the **CustomerKey** columns in the **Customers** table and the **Internet Sales** table
+   13. Create a relationship between the **ProductKey** columns in the **Products** table and the **Internet Sales** table
+   14. Create a relationship between the **SalesTerritoryKey** columns in the **Sales Territories** table and the **Internet Sales** table
+   15. Save your work

```
let
    Source = AdWorksDW,
    Navigation = Source{[Schema="dbo",Item="DimProduct"]}[Data],
    Subcategory =
        Table.ExpandRecordColumn(
            Navigation, "DimProductSubcategory",
            {"EnglishProductSubcategoryName", "DimProductCategory"},
            {"Product Subcategory", "DimProductCategory"}
        ),
    Category =
        Table.ExpandRecordColumn(
            Subcategory, "DimProductCategory",
            {"EnglishProductCategoryName"}, {"Product Category"}
        )
in
    Category
```