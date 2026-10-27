# See also

+   - ArcGIS for Power BI documentation: `https://bit.ly/3VtMckC`

# Democratizing Artificial Intelligence

Recent additions to Power BI's visualization library help democratize artificial intelligence by making machine learning intuitive and easy to use by end users. The **Key influencers** and **Decomposition tree** visualizations covered in this chapter, as well as other visuals like **Smart narrative** and **Q&A**, leverage machine learning algorithms to analyze visualization data in real time to bring artificial intelligence insights to the masses. Machine learning is the process of analyzing data based purely on mathematical algorithms in order to identify patterns, key metrics, clusters, anomalies, and key categorizations.

This recipe demonstrates how to configure and use the artificial intelligence capabilities of the **Key influencers** and **Decomposition tree** visualizations.

# Getting ready

To prepare for this recipe, follow these steps:

+   1. Open a Power BI Desktop file locally and access the **Power Query Editor** by clicking on **Transform Data** in the ribbon of the **Home** tab.
+   2. Create a query named AdWorksDW similar to the following:

```
```powerquery
let
    Source = Sql.Database("localhost\\MSSQLSERVERDEV", "AdventureWorksDW2019")
in
    Source
```
```

+   3. Disable load on the AdWorksDW query and place it into a **Data Sources** query group.
+   4. Right-click the AdWorksDW query and choose **Reference**.
+   5. Rename the query **Internet Sales** and choose the **FactInternetSales** table.
+   6. Repeat steps 4 and 5, creating a **Dates** query that points to the **DimDates** table, and a **Sales Territories** query that points to the **DimSalesTerritory** table.
+   7. Create a **Customers** query using the following code:

```
```powerquery
let
    Source = AdWorksDW,
    dbo_DimCustomer = Source{[Schema="dbo",Item="DimCustomer"]}[Data],
    ExpandGeography = Table.ExpandRecordColumn(dbo_DimCustomer, "DimGeography",
```
```