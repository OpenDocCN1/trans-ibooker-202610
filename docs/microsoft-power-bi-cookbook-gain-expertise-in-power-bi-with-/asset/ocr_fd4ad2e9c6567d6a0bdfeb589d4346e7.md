+   3. Isolate this query in a query group called **Data Sources**.
+   4. Right-click AdworksDW and choose **Reference**, select the **FactFinance** table in the data preview area, and rename this query **FactFinance**.
+   5. Move the **FactFinance** query to the **Other Queries** group.

For additional details on performing these steps, see the *Managing Queries and Data Sources* recipe in this chapter.

# How to Profile Source Data

To implement this recipe, perform the following steps:

+   1. Select the **FactFinance** query.
+   2. In the **View** tab, check the box for **Column profile** in the **Data Preview** area of the ribbon.
+   3. Select a column to view the column statistics similar to those in *Figure 2.44*:

![](bbox=[0.2, 0.4, 0.87, 0.61])

Figure 2.44: Column profile information

# How it works

By default, the M query engine pulls the first 1,000 rows of data from the specified data source. This is the data used in the data preview area. The default of using only the first 1,000 rows of data can be changed by clicking on **Column profiling based on top 1000 rows** in the status bar in the bottom right and choosing **Column profiling based on the entire data set**. Statistics are collected on each column in the data source. When the **Column profile** feature is enabled, selecting a column causes column statistics and value distribution to display in the bottom half of the data preview area, including alongside this error counts, the number of distinct and unique values, minimum values, maximum values, and additional information that depends upon the data type of the column, as shown in *Figure 2.44*.