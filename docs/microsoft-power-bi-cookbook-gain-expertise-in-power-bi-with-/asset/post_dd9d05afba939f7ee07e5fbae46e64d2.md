
## Supporting Virtual Table Relationships

Virtual table relationships are DAX expressions implemented to filter one table from another when a relationship does not exist between the two. Report visualizations can then be constructed using both tables (and those related to it), and the DAX measures will update as though a normal relationship is defined. Virtual relationships are often used to address disparate grains of tables and to leverage performance segmentation tables.

Although physical relationships are the preferred long-term solution for both performance and manageability, virtual relationships provide an attractive alternative when physical relationships are not feasible. In this recipe, we provide virtual relationship examples that use both a custom performance segmentation table and an aggregated table.

### Getting ready

To prepare for this recipe, follow these steps:

- 1. Open Power BI Desktop.
- 2. Create an Import mode data source query called **AdWorksDW**. This query should be similar to the following:

```powerquery
let
    Source = Sql.Database("localhost\\MSSQLSERVERDEV", "AdventureWorksDW2019")
in
    Source
```

- 3. Isolate this query in a query group called **Data Sources** and disable loading.
- 4. Right-click AdWorksDW and choose **Reference**.
- 5. Select the **FactInternetSales** table and name this query **Internet Sales**.
- 6. Repeat steps 4 and 5 to create a **Dates** query that imports **DimDates**.
- 7. Create a **Customers** query using the following code:

```powerquery
let
    Source = AdWorksDW,
    dbo_DimCustomer = Source{[Schema="dbo",Item="DimCustomer"]}[Data],
    Country =
        Table.ExpandRecordColumn(
            dbo_DimCustomer, "DimGeography",
            {"EnglishCountryRegionName"}, {"Country"})
in
    Country
```

- 8. Create a **Products** query using the following code:

```powerquery
let
    Source = AdWorksDW,
    Navigation = Source{[Schema="dbo",Item="DimProduct"]}[Data],
    Subcategory =
        Table.ExpandRecordColumn(
            Navigation, "DimProductSubcategory",
            {"EnglishProductSubcategoryName", "DimProductCategory"},
            {"Product Subcategory", "DimProductCategory"}
        )
in
    Subcategory
```

- 9. Move the **Internet Sales** query to a query group called **Facts**, and the **Dates**, **Products**, and **Customers** queries to a query group called **Dimensions**.
- 10. Use an **Enter Data** query to create a **GrowthTiers** query in the **Other Queries** group.

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_170_0.png)

Figure 3.32: Sales Growth Segmentation Table, GrowthTiers

- 11. Create a **SubCatPlan** query in the **Other Queries** group with the following code:

```powerquery
let
    Source = Products,
    Sales =
        Table.ExpandTableColumn(
            Source, "FactInternetSales",
            {"SalesAmount", "OrderDate"}, {"SalesAmount", "OrderDate"}
        ),
    RemoveNulls = Table.SelectRows(Sales, each [SalesAmount] <> null),
    Year =
        Table.AddColumn(
            RemoveNulls, "Calendar Year", each Date.Year([OrderDate])
        ),
    Month =
        Table.AddColumn(
            Year, "Calendar Month", each Date.MonthName([OrderDate])
        ),
    Group =
        Table.Group(
            Month,
            {"Calendar Year", "Calendar Month", "Product Subcategory"},
            {{"SalesAmount", each List.Sum([SalesAmount]), type nullable number}}
        ),
    Index = Table.AddIndexColumn(Group, "Index", 1, 1, Int64.Type),
    Plan =
        Table.AddColumn(
            Index, "Plan Amt",
            each if Number.IsEven([Index])
                then 1/Number.RandomBetween(5,10) * [SalesAmount] + [SalesAmount]
                else (-1 * 1/Number.RandomBetween(5,10) * [SalesAmount]) + [SalesAmount]
        ),
    Remove = Table.RemoveColumns(Plan,{"SalesAmount", "Index"}),
    Currency = Table.TransformColumnTypes(Remove,{{"Plan Amt", Currency.Type}})
in
    Currency
```

- 12. Create a Blank Query called **Calculations** in the Other Queries group.
- 13. Click Close & Apply in the ribbon of the Home tab to load the tables to the data model.
- 14. Create the single cross-filtering relationships shown in Figure 3.33; note that there are no relationships for the **GrowthTiers** or **SubCatPlan** tables:

| Active | From: Table (Column) | To: Table (Column) |
| --- | --- | --- |
| ☑ | Internet Sales (CustomerKey) | Customers (CustomerKey) |
| ☐ | Internet Sales (DueDateKey) | Dates (DateKey) |
| ☑ | Internet Sales (OrderDateKey) | Dates (DateKey) |
| ☑ | Internet Sales (ProductKey) | Products (ProductKey) |
| ☐ | Internet Sales (ShipDateKey) | Dates (DateKey) |

Figure 3.33: Model relationships

- 15. Switch to the Data view and hide the single column in the Calculations table.
- 16. Create the following measures in the Calculations table:

```DAX
Sales Current Year =
    CALCULATE(SUM('Internet Sales'[SalesAmount]),
        FILTER(ALLEXCEPT('Internet Sales','Customers'),
            YEAR('Internet Sales'[OrderDate]) =
        MAX('Dates'[CalendarYear]) ) )
```

```DAX
Sales Previous Year =
    CALCULATE(
        SUM('Internet Sales'[SalesAmount]),
        FILTER(ALLEXCEPT('Internet Sales','Customers'),
            YEAR('Internet Sales'[OrderDate]) =
        MAX('Dates'[CalendarYear])-1 ) )
```

```DAX
Sales Growth =
    DIVIDE([Sales Current Year] - [Sales Previous Year],
        [Sales Previous Year], BLANK() )
```

- 17. Right-click the Dates table and choose Mark as date table using FullDateAlternateKey as the Date column.

### How to Support Virtual Table Relationships

To implement this recipe, use the following steps:

- 1. Create the Sales Growth Tier measure in the Calculations table:

```DAX
Sales Growth Tier =
    VAR Growth = [Sales Growth]
    VAR Level1 =
        CALCULATE(MIN('GrowthTiers'[Max]), 'GrowthTiers'[GrowthTierKey] = 1)
    VAR Level2 =
        CALCULATE(MIN('GrowthTiers'[Max]), 'GrowthTiers'[GrowthTierKey] = 2)
    VAR Level3 =
        CALCULATE(MIN('GrowthTiers'[Max]), 'GrowthTiers'[GrowthTierKey] = 3)
    VAR Level4 =
        CALCULATE(MIN('GrowthTiers'[Max]), 'GrowthTiers'[GrowthTierKey] = 4)
    RETURN
        SWITCH(TRUE(),
            ISBLANK(Growth), BLANK(),
            Growth <= Level1, CALCULATE(DISTINCT('GrowthTiers'[Growth Tier]),
                'GrowthTiers'[GrowthTierKey] = 1),
            Growth <= Level2, CALCULATE(DISTINCT('GrowthTiers'[Growth Tier]),
                'GrowthTiers'[GrowthTierKey] = 2),
            Growth <= Level3, CALCULATE(DISTINCT('GrowthTiers'[Growth Tier]),
                'GrowthTiers'[GrowthTierKey] = 3),
            Growth <= Level4, CALCULATE(DISTINCT('GrowthTiers'[Growth Tier]),
                'GrowthTiers'[GrowthTierKey] = 4
            ), "Unknown")
```

- 2. Create the virtual table relationship measure **Customer Countries** in the **Calculations** table:

```DAX
Customer Countries =
    CALCULATE(DISTINCTCOUNT('Customers'[Country]),
        FILTER(ALL('Customers'[Country]),
            [Sales Growth] > MIN('GrowthTiers'[Min]) &&
            [Sales Growth] < MAX('GrowthTiers'[Max]) ) )
```

- 3. Create a **Slicer** visualization that uses the **CalendarYear** column from the **Dates** table.
- 4. Create a **Matrix** visualization with the **Country** column from the **Customers** table as **Rows** and the measures **Sales Growth**, **Sales Current Year**, **Sales Previous Year**, and **Sales Growth Tier** as **Values**.
- 5. Create a **Clustered column chart** visualization with the **Growth Tier** column from the **GrowthTiers** table as the **Axis**, and the **Customer Countries** measure as **Values**.

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_173_0.png)

Figure 3.34: Virtual relationship segmentation example

- 6. Save your work.

### How it works

The goal of this recipe is to apply the Growth Tier segmentations from the disconnected GrowthTiers table to the measures of the data model. Two measures are used to form this virtual relationship, Sales Growth Tier and Customer Countries. The measures apply filters based on the segmentation table thresholds and the Sales Growth measure. In effect, these measures form the relationship between the tables in the data model through the calculations performed.

The Sales Growth Tier measure returns the text value of the Growth Tier column for the specified Sales Growth measure value in context. The existing Sales Growth measure and the four segment threshold values are stored in DAX variables (the VAR statements). The SWITCH function compares sales growth with the segment thresholds to assign the Growth Tier value. Providing TRUE() as the first parameter of the SWITCH function allows independent logical conditions to be evaluated in order (from top to bottom). This is similar to the CASE expression in SQL. The Customer Countries measure uses a simple CALCULATE statement to return a DISTINCTCOUNT of countries filtered by the MIN and MAX limits of each Growth Tier.

The Sales Growth Tier measure could be used for analyzing other dimensions of the model, and at different grains; for example, if StateProvinceName was added to the Customers table and a hierarchy created, you could drill down with the matrix visual and see growth tiers by state/province.

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_174_0.png)

There are options to modify data models to support physical relationships and thus avoid the limitations of virtual relationships. For example, a concatenated column such as Year-Month could be created for each table via SQL, M, or a DAX-calculated column.
