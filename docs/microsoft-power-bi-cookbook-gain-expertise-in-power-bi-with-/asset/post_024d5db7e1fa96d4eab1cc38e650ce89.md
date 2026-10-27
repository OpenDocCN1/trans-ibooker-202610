
## Building a Power BI Data Model

2. Create many-to-one, single-direction relationships from each fact table to its corresponding dimension table. Bidirectional relationships should only be implemented with clear guidance and evaluation. Date data type columns should be used for the Date table relationships.

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_132_0.png)

Figure 3.4: Relationships view of a multi-fact data model

3. Write the core measures for each fact table identified in the planning phase and validate them for accuracy. If complex DAX expressions are needed for the core measures, then the source and retrieval should be reviewed. Give each measure an intuitive name and a standard format (for example, two decimal places, a thousands separator).

4. Add multi-level hierarchies in commonly used dimension tables such as Product and Date. Hierarchies are essentially just groups of columns that allow for report authors to more easily navigate or drill between different levels of detail.

5. Set the Default Summarization and Sorting of columns, such as Month Name, sorted by Month Number.

6. Assign Data Categories to columns such as Address or Postal Code to support geographical visualization.

7. Hide columns from the fact tables such that only measure groups are visible.

8. Validation and user testing of the new model should follow implementation.

9. Model documentation can be developed via Dynamic Management Views (DMVs) to provide users with definitions and relationships. See the Importing and visualizing dynamic management view (DMV) data recipe of Chapter 10, Administering and Monitoring Power BI, for a detailed example of this pattern.

### How it works

The steps in this recipe align with the logical flow of model development and can be implemented in discrete phases across teams or by an individual Power BI developer. Given the different lead times associated with the components of the model, it can be advantageous to move forward with a more mature or simple component such that business teams can engage and provide feedback as enhancements and other components are deployed.

Details on all primary metadata settings are included in this chapter in the following recipes:

- Configuring Model Metadata
- Hiding Columns and Tables
- Creating Hierarchies and Groups

All of these settings impact the usability and functionality of the data model and should not be neglected.

### There's more...

If a model's source view is shared with other applications and may change or include columns not needed by the model, the Table.SelectColumns() M function can be used:

```
let
    Source = AdWorksDW,
    InternetSales = Source{[Schema="dbo",Item="vInternetSales"]}[Data],
    SalesColumns =
        Table.SelectColumns(
            InternetSales,{"ProductKey","OrderDateKey"},MissingField.UseNull
        )
in
    SalesColumns
```

Each column required by the table in the model is explicitly selected, and the MissingField.UseNull parameter allows the query to refresh successfully despite a specified column being missing—such as when a column's name has changed.

### See also

- Create SQL Views: https://bit.ly/2T3FzHF
- Create and manage relationships in Power BI: http://bit.ly/3tLwcJM
- Model view in Power BI: http://bit.ly/3vUXwag
- Model data in Power BI: http://bit.ly/3f8p7yT
- Design a data model in Power BI: http://bit.ly/3d28LVW

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

8. Separate the fact query **Internet Sales** into a **Facts** query group by right-clicking the query in the Queries pane and choosing Move to Group and then New Group

9. Separate the dimension queries, **Customers**, **Dates**, and **Products** into a **Dimensions** query group.

10. Create a **Blank Query** called **Calculations** in the **Other Queries** group.

11. Choose **Close & Apply** in the ribbon of the **Home** tab to load the tables to the data model.

## How to Create Relationships

To implement this recipe, use the following steps:

1. Create a relationship between the **Products** table and the **Internet Sales** table. This can be done in the **Model** view by dragging and dropping the **ProductKey** column in the **Products** table to the **ProductKey** column in the **Internet Sales** table. Alternatively, select **Manage relationships** in the ribbon of the **Modeling** tab while in the **Report** view or from the ribbon of the **Table tools** tab while in the **Data** view. This relationship should be a one-to-many relationship between the **Products** table on the **One** side and the **Internet Sales** table on the **Many** side. The **Cross-filter direction** should be set to **Single**.

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_136_0.png)

2. Create additional relationships between the other dimension tables and the **Internet Sales** table until your **Manage relationships** dialog box has the same relationships checked as shown in *Figure 3.6*.

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_136_1.png)

3. The **Model** view of the dataset should now present, as shown in *Figure 3.7*:

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_137_0.png)

Figure 3.7: Model view of relationships

4. Right-click the **Dates** table and choose **Mark as date table** using **FullDateAlternateKey** as the **Date** column.

5. Create the following four DAX measures by right-clicking the **Calculations** table in the **Data** view and selecting **New measure**:

```
Sales Table Rows = COUNTROWS('Internet Sales')
```

```
Customers Table Rows = COUNTROWS('Customers')
```

```
Dates Table Rows = COUNTROWS('Dates')
```

```
Products Table Rows = COUNTROWS('Products')
```

6. Right-click the **Calculations** column in the **Calculations** table and choose **Hide in report view.**

7. Use a **Multi-row card** visualization to display these four measures.

8. Add a **Slicer** visualization for **CalendarYear** from the **Dates** table, **Product Category** from the **Products** table, and **Gender** from the **Customers** table.

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_138_0.png)

Figure 3.8: Relationship and cross-filter testing visualizations

9. Select slicer values in the **Slicer** visualizations and note that with single-direction relationships, a selection on any of the slicers only impacts the row counts in the table used in the slicer and the **Internet Sales** table.

10. Open the **Manage relationships** dialog and edit the **Cross-filter direction** of the relationship between the **Internet Sales** and **Products** tables.

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_138_1.png)

Figure 3.9: Bidirectional Relationship Configuration (Both) from the Edit Relationships Dialog

11. Slicer selections for **CalendarYear** and **Gender** now also filter the Products table.

12. Create a DAX measure that applies an alternative cross-filter behavior to the relationship in the model:

```
Products Table Rows (CF) =
    CALCULATE([Products Table Rows],
        CROSSFILTER('Internet Sales'[ProductKey],'Products'[ProductKey], OneWay) )
```

13. Add this measure to the Multi-row card visualization and observe that this measure is unaffected by the Gender and CalendarYear slicers.

14. Write a DAX measure to propagate filter context. The following measure respects the filters applied to the Internet Sales table, such as Customer Gender = "M":

```
Products Rows (Sales) = CALCULATE(COUNTROWS('Products'),'Internet Sales')
```

15. Add this measure to the Multi-row card visualization and observe that this measure returns the same results as the bidirectional relationship when selecting values in the Gender and CalendarYear slicers, except when there are no slicer selections at all—then the measure returns 158.

16. Write a DAX measure to use an inactive relationship between the Dates and Internet Sales tables:

```
Sales Table (Orders) =
    CALCULATE([Sales Table Rows],
        USERELATIONSHIP('Dates'[DateKey],'Internet Sales'[OrderDateKey])
    )
```

17. Add this measure to the Multi-row card visualization and notice that different numbers are returned from the Sales Table Rows measure when selecting CalendarYear 2010 or higher.

### How it works

Single-direction cross-filter relationships only propagate filters in the direction of the relationship. Thus, when Products is set to filter Internet Sales in a single-direction, other filters affecting Internet Sales are not propagated to the Products table. With bidirectional cross-filter relationships, filters affecting one side of the relationship are always propagated to the other side of the relationship unless DAX measures use functions such as CROSSFILTER() to override the default behavior defined by the model relationships.

Bidirectional relationships between fact and dimension tables should generally be avoided when the given dimension table is also related to other fact tables. This can cause over-filtering or unintended filter contexts. However, bidirectional relationships are an integral part of efficient solutions to common (and otherwise complex) modeling scenarios, such as actual versus budget and classic many-to-many scenarios.

Regarding the Date table relationships, the DAX time intelligence functions, such as DATESYTD, DATEADD, and SAMEPERIODLASTYEAR, all require either a relationship based on a date data type or a Mark as date table setting. Since the relationships for the Date table were formed on surrogate keys (20150101), the Date table needs to be marked as a date table in order for DAX time intelligence functions to operate properly.
