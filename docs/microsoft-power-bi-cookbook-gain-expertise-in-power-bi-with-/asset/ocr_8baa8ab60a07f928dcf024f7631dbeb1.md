# Building a Power BI Data Model

```
```powerquery
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
```

+   9. Move the **Reseller Sales**, **Internet Sales**, and **Currency Rates** queries to a query group called **Facts** and the **Dates**, **Products**, and **Customers** queries to a query group called **Dimensions**.
+   
+   10. Create four blank queries called **Reseller Measures**, **Pricing Measures**, **Customer Measures**, and **Currency Measures** in the **Other Queries** group.
+   
+   11. Click **Close & Apply** in the ribbon of the **Home** tab to load the tables to the data model.
+   
+   12. Create the relationships shown in *Figure 3.25*.

| Active | From: Table (Column) | To: Table (Column) |
|---|---|---|
| ☑ | Currency Rates (DateKey) | Dates (DateKey) |
| ☑ | Internet Sales (CustomerKey) | Customers (CustomerKey) |
| ☐ | Internet Sales (DueDateKey) | Dates (DateKey) |
| ☑ | Internet Sales (OrderDateKey) | Dates (DateKey) |
| ☑ | Internet Sales (ProductKey) | Products (ProductKey) |
| ☐ | Internet Sales (ShipDateKey) | Dates (DateKey) |
| ☐ | Reseller Sales (DueDateKey) | Dates (DateKey) |
| ☑ | Reseller Sales (OrderDateKey) | Dates (DateKey) |
| ☑ | Reseller Sales (ProductKey) | Products (ProductKey) |
| ☐ | Reseller Sales (ShipDateKey) | Dates (DateKey) |

Figure 3.25: Model relationships

+   13. All of these relationships' cross-filtering directions should be **Single**, except for the **Internet Sales to Customers** relationship, which should be set to **Both**.
+   
+   14. Switch to the **Data** view and hide the single columns in the **Reseller Measures**, **Pricing Measures**, **Customer Measures**, and **Currency Measures** tables.
+   
+   15. Right-click on the **Date** table and choose **Mark as date table** using **FullDateAlternateKey** as the **Date** column.