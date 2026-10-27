
- 3. Disable load on the AdWorksDW query and place it into a **Data Sources** query group
- 4. Right-click the AdWorksDW query and choose **Reference**
- 5. Rename the query **Internet Sales** and choose the **FactInternetSales** table
- 6. Repeat steps 4 and 5, creating a **Dates** query that points to the **DimDates** table, a **Customers** query that points to the **DimCustomer** table, and a **Sales Territories** query that points to the **DimSalesTerritory** table
- 7. Create a **Products** query using the following code:

```let
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

- 8. Move the **Internet Sales** query to a **Facts** query group, and the **Dates**, **Customers**, and **Products** queries to a **Dimensions** query group
- 9. In the **Other Queries** group, create a blank query called **Calculations**
- 10. Select **Close & Apply** from the ribbon of the **Home** tab
- 11. Switch to the **Model** view and create a relationship between the **DateKey** column in the **Dates** table, and the **OrderDateKey** in the **Internet Sales** table
- 12. Create a relationship between the **CustomerKey** columns in the **Customers** table and the **Internet Sales** table
- 13. Create a relationship between the **ProductKey** columns in the **Products** table and the **Internet Sales** table
- 14. Create a relationship between the **SalesTerritoryKey** columns in the **Sales Territories** table and the **Internet Sales** table
- 15. Save your work

## Applying Advanced Analytics and Custom Visuals

```
Average Customer Orders =
AVERAGEX(DISTINCT(Customers[CustomerAlternateKey]),[Internet Sales Orders])
```

```
Average Days Since Last Purchase =
AVERAGEX(DISTINCT(Customers[CustomerAlternateKey]),[Days Since Last Purchase])
```

- 6. Create a simple **Table** visualization using the three measures created plus the **Customers (RFM)** column and **CustomerAlternateKey** (count) from the **Customers** table, as well as the **Internet Net Sales** measure

| Customers (RFM) | Internet Net Sales | Count of CustomerAlternateKey | Average Customer Sales | Average Customer Orders | Average Days Since Last Purchase |
|---|---:|---:|---:|---:|---:|
| Cluster1 | $7,856,813 | 14072 | $558 | 1.29 | 175 |
| Cluster2 | $1,201,140 | 410 | $2,930 | 1.00 | 852 |
| Cluster3 | $20,300,724 | 4002 | $5,073 | 2.28 | 172 |
| **Total** | **$29,358,677** | **18484** | **$1,588** | **1.50** | **189** |

Figure 9.29: Average Customer Measures used with the Customers (RFM) clusters

- 7. On a new page, create a Scatter chart with the **Average Days Since Last Purchase** measure as the **X Axis**, the **Average Customer Sales** measure as the **Y Axis**, and the **Customers (RFM)** column from the **Customers** table as **Legend**

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_438_0.png)

Figure 9.30: Clusters visualized in a scatter chart by Internet Net Sales and Days Since Last Purchase

A potential use case or action based on these clusters is to focus marketing efforts on converting the **Cluster1** customers, who have purchased recently, to higher-value **Cluster3** customers. Also, **Cluster2** customers may have already committed to a new bike supplier or, more optimistically, may have purchased a bike two to three years ago, and may not be aware of what bike-related accessories and clothing are available.

### How it works

Feature engineering is a common practice in data science, in which new columns are added to a dataset to produce more accurate models. The new columns often contain built-in logic, and features (columns) are added, removed, and modified iteratively based on the models produced. Identify measures that add the most value to the algorithm by representing the dimensions in different ways. The three measures used to support the clustering in this example follow the RFM technique, identifying the recency, frequency, and monetary (value) of the customer's purchase history. Adding measures (feature engineering) that cover each component of RFM is useful for various marketing and customer attrition analyses.

Following the RFM technique, the **Days Since Last Purchase** measure identifies the recency, **Internet Sales Orders** measures the frequency, and **Internet Net Sales** measures the monetary value. The **Last Purchase Date** measure is an intermediary measure created to support the **Days Since Last Purchase** measure. **Days Since Last Purchase** uses the **Last Purchase Date** measure and a **Today** measure as parameter inputs to the DATEDIFF function. In a production scenario, the **Today** measure would use the TODAY function instead of a hard-coded DATE.

Three clusters were created in this example. These clusters are represented by a column added to the **Customers** table of the data model with the name provided in the **Clusters** dialog, **Customers (RFM)**. The cluster column is identified in the **Fields** list with two overlapping square shapes, and an **Edit clusters** option is available by either right-clicking the column or selecting the ellipsis next to the column.

For the analysis measures, **Average Customer Sales**, **Average Customer Orders**, and **Average Days Since Last Purchase**, the **AVERAGEX** function is used to iterate over the unique customer keys provided by the **DISTINCT** function to compute the customer-specific value (sales, orders, and days since purchase) and then return the average of the measures from each cluster.

The Power BI clustering feature uses the K-Means algorithm to determine the optimal number of clusters to create. Currently, a cluster is limited to 15 measures and one dimension; an error message is returned if these limits are exceeded.

### There's more

In addition to the standard Power BI clustering from this recipe, additional custom visuals for clustering are available in the AppSource, including **Clustering** and **Clustering with Outliers**. Both these custom visuals are built with the R statistical programming language.

Like the table visual from the example, clusters can also be automatically created from a Scatter chart visual. These clusters are limited to two input measures (X and Y) but the clusters are automatically added to the **Details** field. To see how this works, do the following:

- 1. On a new page, create a Scatter chart visualization using the **Internet Net Sales** measure as the **X Axis**, the **Internet Sales Orders** measure as the **Y Axis**, and the **EnglishProductName** column from the **Products** table as **Details**
- 2. Select the scatter chart visualization, click the ellipsis (**...**), and choose **Automatically find clusters**

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_440_0.png)

Figure 9.31: Clusters automatically added to the legend of a Scatter chart

### See also

- Apply clustering techniques: https://bit.ly/3gWvKK3

## Forecasting and Anomaly Detection

Standard Power BI report and dashboard visualizations are great tools to support descriptive and diagnostic analytics of historical or real-time data, but ultimately organizations need predictive and prescriptive analytics to help guide decisions involving future outcomes. Power BI Desktop provides a time series forecasting tool with built-in predictive modeling capabilities that enables report authors to quickly create custom forecasts, evaluate the accuracy of these forecasts, and build intuitive visualizations that blend actual or historical data with the forecast.
