
+   4. Duplicate Page 1
+   5. Select the matrix visualization on `Duplicate of Page 1` and replace the columns with the Name column from the `Time Intelligence Group`

The numbers in both matrices are identical.

## See also

+   - Calculation groups in Analysis Services tabular models: https://bit.ly/3t1JRYd
+   - DAX `SELECTEDMEASURE` function: https://bit.ly/3sv5XWN

## Conclusion

This chapter provided recipes for preparing a data model to support robust date intelligence and for authoring custom date intelligence measures. Date intelligence provides a broad set of data modeling patterns and functions that enable analysis across time periods. The measures created for date intelligence can be used in combination with one another to support growth and variance measures and are often utilized as KPIs in Power BI dashboards. When properly implemented, date intelligence dramatically expands the analytical power of a data model and simplifies report and dashboard development.

## Parameterizing Power BI Solutions

With the foundation of a Power BI deployment in place, components of the data retrieval and report design processes—as well as the user experience—can be parameterized to deliver greater flexibility for both technology services and users. For example, query parameters can isolate and restrict data sources to support changing source systems, templates can enable parameterized report development against pre-defined metadata, and M and DAX functions can deliver custom integration and analytical capabilities.

The recipes in this chapter cover both standard parameterization features and techniques in Power BI, as well as more advanced custom implementations. Examples of parameterizing data sources, queries, user-defined functions, and reports further express the power of the M language and its integration with other Power BI Desktop features. Additional examples, such as URL-based parameter filters, a dedicated forecasting or what-if? tool, and user selection parameter tables, utilize both the transformation and analytical features of Power BI to empower users with greater control over the analysis and visualization of Power BI data models.

In this chapter, we will cover the following recipes:

+   - Filtering reports dynamically
+   - Leveraging query parameters
+   - Working with templates
+   - Converting static queries to dynamic functions
+   - Capturing user selections with parameter tables
+   - Forecasting with what-if analysis

## Technical requirements

The following are required to complete the recipes in this chapter:

+   - Power BI Desktop
+   - SQL Server with the AdventureWorksDW2019 database installed. This database and the instructions for installing it are available here: http://bit.ly/20VQfG7
+   - Files for this chapter can be downloaded from the following GitHub repository: https://github.com/geckler/PowerBICookbook2ndEdition

## Filtering reports dynamically

In addition to the report filter options in Power BI Desktop covered in *Chapter 4, Authoring Power BI Reports*, filters can also be applied to published Power BI reports via the URL string. Rather than multiple, dedicated reports and report pages with distinct filter conditions, URL links with unique query strings can leverage a single published report in the Power BI service. Additionally, URL links can be embedded within a dataset such that a published report can expose links to other reports with a pre-defined filter condition.

In this recipe, two URL strings are created to demonstrate single and multiple filter parameter syntax. The second example creates a URL string for each row of the Product dimension table via an M query and exposes this dynamic link in a report visual.

## Getting ready

To prepare for this recipe, follow these steps:

+   1. Download CH7_R1.pbix from the following GitHub repository: https://github.com/PacktPublishing/Microsoft-Power-BI-Cookbook-Second-Edition
+   2. Open CH7_R1.pbix in Power BI Desktop.
+   3. Edit the AdWorksDW query and change the Source line to point to your SQL server with the AdventureWorksDW2019 database installed.
+   4. Publish the report to the Power BI service.

## How to filter reports dynamically

To implement this recipe, perform the following steps:

+   1. Open the report in the Power BI service.
+   2. Replace the query string in the URL with the following and press Enter:
+   `?filter=Products/ProductCategory eq 'Bikes'`
+   3. To apply multiple URL filter parameters, separate the column filters with an and operator, as in the following example:
+   `?filter=Products/ProductCategory eq 'Bikes' and Dates/CalendarYear eq 2013`
+   4. Copy the report URL, without including the query string (the portion after and including the ?).
+   5. Open CH7_R1.pbix in Power BI Desktop
+   6. Edit the Products query in Power Query Editor.
+   7. Add a column called Product URL to the Products table by using Custom Column in the ribbon of the Add Column tab, with a format similar to the following:
+   `"https://app.powerbi.com/groups/[...]/reports/[...]/[...] ?filter=Products/ProductName eq '' & [ProductName] & ''"`

> Note that [...] will be replaced with your unique IDs.

+   8. Close and apply the query changes to apply them to the model.
+   9. Change the Data category for the Product URL column in the Products table to Web URL.
+   10. Add a second page to the report.
+   11. Create a Table visualization with the ProductName column from the Products table, the Total Net Sales measure from the Calculations table, and the Product URL column from the Products table.
+   12. With the Table visualization selected, use the Format pane, expand the Values section, and toggle the URL icon to On.
+   13. Save and publish the report.
+   14. Open the report in the Power BI service, navigate to Page 2, and click on one of the URL icons for a product.

## How it works...

URL filters can be applied to any column in the data model that is of a text or numeric data type. The column does not have to be visible in the Fields list or used in one of the Filtering field wells in the Report view to be used in a URL filter.

The syntax is <Report URL>?filter=Table/Column eq 'value'. The table and field names (without spaces) are case sensitive, and the 'value' must be enclosed in single quotes for text values. Do not use single quotes for numeric values, however.

Table and column names used in URL filters must not contain spaces. In the event that you want to have a friendly name for report users for columns, you can duplicate a friendly name column, remove the spaces from the duplicate column name, and then hide the duplicate column name from the report view. In this way, you can have a column for use in reports and a duplicate column that can be used as URL filters. When using duplicate columns, given the additional resources required of the new column(s), try to limit the columns to those with few distinct values. Additionally, a single hidden column with no spaces can be created based on the concatenation of multiple columns to simplify the URL strings.

## There's more...

A report visual from a custom URL with a query string can be pinned to a dashboard, and the dashboard tile will reflect the filter condition following refreshes. However, by default, selecting the pinned dashboard tile will navigate to the unfiltered source report. The custom URL can be associated with a dashboard tile to control the dashboard navigation, as demonstrated in Figure 7.1:

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_332_0.png)

Figure 7.1: Custom URL link for a Power BI dashboard tile

## See also

+   Filter a report using query string parameters in the URL: https://bit.ly/3swEJPF

## How to leverage query parameters

To implement this recipe, perform the following steps:

+   1. Create a blank query called CurrentDate and disable the loading of the query by right-clicking the query and then deselecting **Enable load**:
+   
+   ```m
+   let
+       Source = #date(2014,1,28)
+   in
+       Source
+   ```
+   2. From the **Home** tab of **Power Query Editor**, click on the **Manage Parameters** dropdown and select **New Parameter**:

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_334_0.png)

Figure 7.2: New parameter created for filtering fact table queries

+   3. Give the query the name **Days Prior to Current Date**, a data type of **Decimal Number**, and, for this example, enter a list of suggested values. Values outside this suggested list can also be applied to the parameter when necessary.
+   4. Based on the **List of values**, enter a **Default Value** and **Current Value**.
