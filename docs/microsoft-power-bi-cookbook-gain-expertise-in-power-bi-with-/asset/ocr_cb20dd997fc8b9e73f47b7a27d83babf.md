```
{"{SalesTerritoryRegion}"}),
    DupCity = Table.DuplicateColumn(ExpandSalesTerritory, "City", "City1"),
    DupZip = Table.DuplicateColumn(DupCity, "PostalCode", "Zip"),
    Address = Table.CombineColumns(DupZip,
        {"AddressLine1", "City1", "StateProvinceCode"},
        Combiner.CombineTextByDelimiter(", ", QuoteStyle.None),"Address"),
    FullAddress = Table.CombineColumns(Address,{"Address", "Zip"},
        Combiner.CombineTextByDelimiter(" ", QuoteStyle.None),"Full Address")
    in
    FullAddress
```

+   5. Click **Close & Apply** from the ribbon of the **Home** tab.
+   6. In Power BI Desktop, select the **City** column in the **Customers** table and assign a **Data category** of **City**.
+   7. Select the **Full Address** column in the **Customers** table and assign a **Data category** of **Address**.

# How to Create Geospatial Mapping with ArcGIS Maps

To implement this recipe, use the following steps:

+   1. Apply a page-level filter using the **SalesTerritoryRegion** column in the **Customers** table, and select **Southwest** only
+   2. In the **Visualizations** pane, select the **ArcGIS Maps for Power BI** visualization and then expand the visualization's size to the entire page
+   3. From the **Customers** table, add the **Full Address** column for **Location**, the **CustomerKey** column for **Size**, and the **YearlyIncome** column for **Color**
+   4. In the **Format** pane, expand **Location type** and choose **United States**

![](bbox=[0.2, 0.65, 0.865, 0.785])

+   5. In the upper left of the ArcGIS map visualization, select **Expand map tools**