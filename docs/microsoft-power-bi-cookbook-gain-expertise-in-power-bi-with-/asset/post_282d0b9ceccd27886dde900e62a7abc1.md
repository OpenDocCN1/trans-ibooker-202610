
## How it works...

The **Reseller Margin %** measure includes three lines of comments to describe the business definitions of the measure's components. Comments can also be added per line via the -- and // characters and Power BI applies green color coding to this text. Embedding comments is recommended for both complex measures with multiple components and simple measures, which form the foundation of many other measures.

The **ShippedSales** variable filters the **Reseller Sales** fact table to exclude the unshipped sales order lines and this table is used as a filter parameter in the **NetSalesShipped** variable. The existing **Reseller Gross Sales** and **Reseller Discount Amount** measures are referenced, but the **ProductCost** variable is explicitly defined against the **Reseller Sales** fact table (shipped or not). Though significantly longer than alternative DAX expressions for the same calculation, the use of variables and comments eliminates (or reduces) the need to review other measures to understand the logic and source columns.

For the **Reseller Gross Sales (Custom)** measure, variables are declared for each of the tables to be filtered and a comment (**Trailing 10 Days**) is inserted to help explain the **DateHistory** variable. The variables are invoked as filter parameters to CALCULATE, and so the **Reseller Gross Sales** measure reflects this modified filter context. The same functional result can be achieved by defining all the filtering logic within CALCULATE, but this would make the expression less readable and thus more difficult to support.

## There's more...

DAX Formatter can be used within DAX Studio to align parentheses with their associated functions. Long, complex DAX measures can be copied from Power BI Desktop into DAX Studio to be formatted. To use DAX Studio to format queries, open DAX Studio, copy and paste the query from Power BI Desktop into the main window, and then click on **Format Query** in DAX Studio. Copy and paste the query from DAX Studio and replace the expression in Power BI Desktop with the formatted expression.

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_533_0.png)

Figure 11.16: DAX Formatter in DAX Studio used to format a year-to-date measure

DAX authoring in Power BI Desktop also supports parentheses highlighting, but DAX Formatter isolates functions to individual lines and indents inner function parameters. Without the function isolation and indentation provided by DAX Formatter, complex expressions are often wide and difficult to interpret or troubleshoot.

## See also

- Improve Power BI Performance by Optimizing DAX: https://bit.ly/3uqUs4x

## Improving Data Load Speeds with Incremental Refresh

Incremental refresh is a feature originally released for Power BI Premium capacities but has become a feature for Pro licenses as well. Prior to incremental refresh, Power BI only had a single mode of operation when refreshing datasets, full load. In other words, the existing data in the dataset was removed and entirely replaced each time the dataset refreshed. The full load refresh process could take a long time when dealing with large fact tables with millions of rows. Incremental refresh solves this problem by only refreshing new and changed data within the dataset. Since incremental refresh is relatively new, older datasets are likely still using the full load process and thus might benefit from being retrofitted with incremental refresh.

This recipe demonstrates how to set up and configure incremental refresh in Power BI.

### Getting ready

To prepare for this recipe, follow these steps:

- 1. Open a Power BI Desktop file locally and access the **Power Query Editor** by clicking on **Transform Data** in the ribbon of the **Home** tab.
- 2. Create a query named AdWorksDW similar to the following:

```
```powerquery
let
    Source = Sql.Database("localhost\\MSSQLSERVERDEV", "AdventureWorksDW2019")
in
    Source
```
```

- 3. Disable load on the AdWorksDW query and place it in a **Data Sources** query group.
- 4. Right-click the AdWorksDW query and choose **Reference**.
- 5. Rename the query **Reseller Sales** and choose the **FactResellerSales** table.
- 6. Move the **Reseller Sales** query to a **Facts** query.
- 7. In the **Other Queries** query group, create a blank query called **Calculations**.
- 8. Choose **Close & Apply** from the ribbon of the **Home** tab.
- 9. Save your work.

- 12. Click **Close & Apply**
- 13. In **Data** view, sort the **OrderDate** column in descending order and note that rows for **11/29/2013 12:00:00 AM** are not present
- 14. While still in **Data** view, right-click the **Reseller Sales** table and select **Incremental refresh**
- 15. In the **Incremental refresh** dialog, toggle **Incremental refresh** to **On** for the **Reseller Sales** table and set **Store rows...** to **30 Years** and **Refresh rows...** to **3 Days**, as shown in the following image:

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_536_0.png)

Figure 11.18: Incremental refresh dialog settings

- 16. Click the **Apply all** button
- 17. Create a simple Card visualization for the report that displays the **Latest OrderDate**
- 18. Publish the report to the Power BI service
- 19. Open the workspace where the report was published in the Power BI service
- 20. Open the **Settings** for the dataset
- 21. If necessary, add the data source to any gateway cluster
- 22. Open the workspace where the report was published in the Power BI service
- 23. Click the **Refresh now** icon for the dataset
- 24. Open the **Settings** for the dataset and investigate the **Refresh history**
- 25. Wait for the dataset to refresh and then open the report to verify that the **Latest OrderDate** is now **11/29/2013 12:00:00 AM**

## How it works...

Incremental refresh uses the reserved parameter names **RangeStart** and **RangeEnd** in executing incremental refresh. While initially set during the configuration of incremental refresh, these parameters are thereafter updated by the Power BI service to determine which rows within the dataset to refresh incrementally.

When configuring the filter rows for incremental refresh, it is important that the two conditions for **RangeStart** and **RangeEnd** do not both include an equal to (=) clause as this may result in duplicate data. Only configure one of the conditions to include an equal to (=) clause in order to avoid such circumstances.

Upon first refreshing the data in the service, Power BI refreshes the entire dataset per the **Store rows where column is in the last** setting in the **Incremental refresh** dialog. In other words, during the first refresh of a dataset configured for incremental refresh, the refresh process functions like a full reload of all of the data. All rows within the store period are included in the dataset unless other filters apply. Subsequent refreshes only refresh the new data per the **Refresh rows where column is in the last** setting.

It is important to remember that once incremental refresh is configured, the Power BI file can no longer be exported from the service. Thus, it is imperative that the original Power BI desktop file be preserved in the event that changes need to be made.

## There's more...

There are two optional incremental refresh settings in the **Incremental refresh** dialog. These optional settings are the following:

- Detect data changes
- Only refresh complete days

The **Detect data changes** setting can be used to further optimize the data refreshed by incremental refresh. A column of type **Date/Time** must be used for this setting and this column should not be the same column used for the **RangeStart** and **RangeEnd** parameters. The Power BI service evaluates the maximum value of this column when refreshing and if the value has not changed since the previous refresh then no data is refreshed within the dataset.

The **Only refresh complete days** setting is used to prevent partial days from being included in data refreshes. For example, in the event that the refresh operation detects that a day is incomplete, such as a scheduled refresh at 4 AM, then that day's data is not included in the refresh operation. This can be useful for organizations that only want complete daily information in their datasets for reporting purposes.
