
By default, **Whole number**, **Decimal number**, and **Fixed decimal number** are set to **Sum as the default summarization**. For numeric columns with default summarization applied, the Fields list applies a summation symbol and aggregates the column's values according to the default summarization setting when used in a report visualization. The same symbol is applied regardless of the default summarization configured (for example, **Average** and **Count**).

Simple DAX measures can deliver all **default summarization** options (**Sum, Min, Max, Average, Count**, and **Count Distinct**). Additionally, measure names such as **Count of Products** can help eliminate confusion regarding the default summarization icon. Many BI professionals prefer to replace fact columns with measures for this reason. Simple DAX expressions are used to perform the various aggregations or summarizations required in the data model; the fact columns are then hidden. When all fact columns are hidden, associated measures display at the top of the **Fields** list. Additionally, using measures allows business logic to be applied, such as including or excluding certain values.

### There's more...

While serving as an intern at Microsoft, Chelsie Eiden solved a long-standing issue with Power BI involving duration by implementing custom format strings in the September 2019 release of Power BI Desktop. Since the release of Power BI Desktop, there was always an issue aggregating durations and displaying them in the format of *HH:MM:SS*. This was the result of the model engine not including a true **Duration** data type, unlike the M query engine. You could convert durations to seconds to aggregate it, but you could not display aggregated durations that properly aggregated as numbers and displayed in the *hours, minutes, seconds* format in a visual. The reason was that there was no numeric formatting option for duration, and using the DAX FORMAT function turned a number into text. Custom format strings solved this issue by allowing the column to remain as a number, but also allowing the creation of a custom format for display purposes. Custom format strings can be applied to a column or measure using the **Properties** pane of the **Model** view, as shown in *Figure 3.18*.

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_148_0.png)

Figure 3.18: Custom format strings

DAX can be used to assist with sorting issues; for example, a DAX-calculated column can be created to support sorting of the **Calendar Yr-Mo** column with values such as *2016-Jan*. The DAX formula to create a **CalYr-Mo Sort** column is:

```
CalYr-Mo Sort = YEAR('Dates'[Date]) * 100 + MONTH('Dates'[Date])
```

This calculation results in values such as *201601*. The YEAR function is applied against the **Date** column and multiplied by 100 to add two digits. This product is then added to the results of the MONTH function applied against the **Date** column, which returns a value between 1 and 12. Although this approach is simple to implement and inexpensive in terms of resource usage, the values of the new column are not sequential, thus limiting the use of the column in Time Intelligence measures (for example, trailing 3 months). Sequential surrogate key columns for each grain of the date dimension, including fiscal calendars, is an essential component of robust date intelligence logic.

The DAX RANKX function can also be leveraged for sorting. For example, use the following DAX code to create a ranking column in the **Departments** table based upon the funding each **Department Group** receives in the **General Ledger**.

```
DeptGroupRank = RANKX('Departments',[Finance])
```

The ranking is based on the **Finance** measure created in this recipe. The **DeptGroupRank** column is re-evaluated during each refresh and thus the sort order could change to reflect the source data.

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_149_0.png)

The **DepartmentGroupName** column can now have a **Sort by column** specified of **DeptGroupRank** to order departments by their funding, with **Sales and Marketing** being first (the most funding) and **Quality Assurance** being last (the least funding).

### See also

- Specify data categories in Power BI Desktop: http://bit.ly/2peNqPm
- Tag barcode fields to enable barcode-scan filtering in mobile apps: http://bit.ly/3re4dAV
- Add hyperlinks (URLs) to a table or matrix: http://bit.ly/39dxJ3r
- Display images in a table or matrix in a report: http://bit.ly/2Pkx1dE
- Sort by column in Power BI Desktop: http://bit.ly/2pFXhgh
- Chelsie Eiden's Duration: http://bit.ly/31KFUtf
- Custom format strings: http://bit.ly/3sj648B
- Use custom format strings in Power BI Desktop: http://bit.ly/3cXAW8o

## Hiding Columns and Tables

Data models must balance the competing demands of functionality, scope, and usability. As additional tables, columns, measures, and other structures are added to meet various analytical needs, a model can quickly become confusing to end users. Therefore, it is important to minimize the visibility of columns and tables to provide an intuitive interface.

In this recipe, we demonstrate how to hide columns and even entire tables from report consumers.

### Getting ready

To prepare for this recipe, follow these steps:

1. Open Power BI Desktop.
2. Create an Import mode data source query called AdWorksDW. This query should be similar to the following:

```
let
    Source = Sql.Database("localhost\\MSSQLSERVERDEV", "AdventureWorksDW2019")
in
    Source
```

3. Isolate this query in a query group called **Data Sources** and disable loading.
4. Right-click AdWorksDW and choose **Reference**.
5. Select the **FactInternetSales** table and name this query **Internet Sales**.
6. Repeat steps 4 and 5 to create a **Departments** query that imports **DimDepartmentGroup** and an **Accounts** query that imports **DimAccount**.
7. Create a **Products** query based upon the **DimProduct**, **DimProductCategory**, and **DimProductSubcategory** tables using the following code. Alternatively, use the SQL view for products from a prior recipe in this chapter, *Implementing a Data Model*.

```
let
    Source = AdWorksDW,
    Navigation = Source{[Schema="dbo", Item="DimProduct"]}[Data],
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
        ),
    RemoveColumns =
        Table.SelectColumns(
            Category,
            {"ProductKey", "EnglishProductName", "DaysToManufacture",
             "Product Subcategory", "Product Category"}
        )
in
    RemoveColumns
```

8. Move the **Internet Sales** query to a query group called **Facts** and the **Departments**, **Accounts**, and **Products** queries to a query group called **Dimensions**.

9. Click **Close & Apply** in the ribbon of the **Home** tab to load the tables to the data model.

10. Create a relationship based on the **ProductKey** columns of both the **Products** table and the **Internet Sales** table.

11. Create the following four measures in the **Internet Sales** table:

```
Internet Sales = SUM('Internet Sales'[SalesAmount])

Internet Sales Freight = SUM('Internet Sales'[Freight])

Internet Sales Order Quantity = SUM('Internet Sales'[OrderQuantity])

Internet Sales Distinct Products =
    COUNTROWS(DISTINCT('Internet Sales'[ProductKey]))
```

### How to Hide Columns and Tables

To implement this recipe, use the following steps:

1. In the **Data** view's **Fields** pane, right-click a column in the **Internet Sales** table and select **Hide in Report view**. Notice that the "visible" icon with a strike-through is displayed to the right of the column in the **Fields** pane. Also, the columns are grayed out in the data viewing area.

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_152_0.png)

Figure 3.20: Hide in report view

2. Repeat this procedure for all columns (but not measures) in the **Internet Sales** table.

3. Once all the columns are hidden, switch to the **Report** view and observe that the **Internet Sales** table is now displayed above the other two tables, out of alphabetical order. In addition, the icon for this table is a calculator instead of a table.

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_152_1.png)

Figure 3.21: Measure table in the Report view Fields list
