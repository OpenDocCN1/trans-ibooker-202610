
+   3. Create a second parameter called **ExcelFile** as **Text** and set **Current Value** to `Employees.xlsx`.
+   4. Move the **ExcelFolder** and **ExcelFile** parameters to a **Parameters** query group.
+   5. Create a new **Blank Query** in the **Other Queries** group called **ExcelFilePath** and disable the load:

```
let
    ExcelPath = ExcelFolder & "\\" & ExcelFile
in
    ExcelPath
```

+   6. Edit the Source line of the `EmployeeKeysAdHoc` query to use the `ExcelFilePath` query:

```
Source = Excel.Workbook(File.Contents(ExcelFilePath), null, true),
```

The path and filename of the Excel file are now controlled by the ExcelFolder and ExcelFile parameters.

## See also

+   ➤ Writing Custom Functions in Power Query M: https://bit.ly/3xnPuYc

## Capturing user selections with parameter tables

An alternative method for providing parameter functionality to users of Power BI reports is via dedicated parameter tables. In this approach, the parameter values of a table are either computed during the dataset refresh process or are loaded as a one-time manual operation. DAX measures reference this parameter table and other tables and expressions of the model to enrich the self-service analysis experience and support Power BI report development. This approach is best suited for parameters that change infrequently since hard-coded parameter tables within Power BI means that the dataset owner must make such changes. If parameters need to change frequently, it may be better to have a database table or external file for the parameter table that is easier to edit and change.

The example in this recipe involves providing simple visibility to four alternative scenarios to the baseline annual sales plan—10 and 20 percent above and below the baseline plan. An inline set of scenario values are embedded in the data model, and DAX measures are used to capture filter context—such as business user selections—and compute the corresponding scenario logic.

## Getting ready

To prepare for this recipe, follow these steps:

+   1. Open a Power BI Desktop file locally and access **Power Query Editor** by clicking on **Transform Data** in the ribbon of the **Home** tab.
+   2. Create a query named AdWorksDW similar to the following:
   
   ```powerquery
   let
       Source = Sql.Database("localhost\\MSSQLSERVERDEV", "AdventureWorksDW2019")
   in
       Source
   ```
+   3. Disable the load on the AdWorksDW query and place it in a Data Sources query group.
+   4. Right-click the AdWorksDW query and choose **Reference**.
+   5. Create a Products query using the following code:
   
   ```powerquery
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
+   6. Create a CategoryPlan query with the following code:
   
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
   ```

```
Table.AddColumn(
    RemoveNulls, "Calendar Year", each Date.Year([OrderDate])
),
Month =
    Table.AddColumn(
        Year, "Calendar Month", each Date.MonthName([OrderDate])
    ),
MonthNum =
    Table.AddColumn(
        Month, "Month Number", each Date.Month([OrderDate])
    ),
Group =
    Table.Group(MonthNum, {"Calendar Year", "Calendar Month", "Product Category", "Month Number"}, {{"SalesAmount", each List.Sum([SalesAmount]), type nullable number}}),
Index = Table.AddIndexColumn(Group, "Index", 1, 1, Int64.Type),
Plan =
    Table.AddColumn(
        Index, "Plan Amt",
        each if Number.IsEven([Index])
            then 1/Number.RandomBetween(5,10) * [SalesAmount] + [SalesAmount]
            else (-1 * 1/Number.RandomBetween(5,10) * [SalesAmount]) + [SalesAmount]
    ),
YearMonthSort =
    Table.AddColumn(
        Plan, "YearMonthSort", each [Calendar Year] * 100 + [Month Number]
    ),
YearMonth =
    Table.CombineColumns(
        Table.TransformColumnTypes(
            YearMonthSort, {{"Calendar Year", type text}}, "en-US"
        ),
        {"Calendar Year", "Calendar Month"},
        Combiner.CombineTextByDelimiter("-", QuoteStyle.None), "Year-Month"
    ),
Currency = Table.TransformColumnTypes(YearMonth, {{"Plan Amt", Currency.Type}})
in
    Currency
```

+   7. Move the **CategoryPlan** query to a query group called **Facts**, and the **Products** query to a query group called **Dimensions**.

## Parameterizing Power BI Solutions

+   8. Disable the load for the **Products** query.
+   9. Select **Close & Apply** from the ribbon of the **Home** tab.
+   10. Set the **Sort By** column for the **Year-Month** column in the CategoryPlan table to the YearMonthSort column.
+   11. Save your work.

## How to capture user selections

To implement this recipe, perform the following steps:

+   1. In Power BI Desktop, while in the **Report** view, click on **New Table** in the ribbon of the **Modeling** tab.
+   2. Use the DAX DATATABLE function to create a calculated table with the scenario name, scenario value, and a sort key:

```
Plan Scenarios =
    DATATABLE(
        "Plan Scenario", STRING,
        "Var to Plan", DOUBLE,
        "Scenario Sort", INTEGER,
        {
            {"Plan",1,3},{"10% Above Plan",1.1,2},
            {"20% Above Plan",1.2,1},
            {"10% Below Plan",.9,4},
            {"20% Below Plan",.8,5}
        }
    )
```

+   3. Select the new table (**Plan Scenarios**) in the **Data** view, and set the **Plan Scenario** column to sort by the **Scenario Sort** column using the **Sort by column** feature in the **Column tools** tab of the ribbon.
+   4. Right-click on the **Scenario Sort** and **Var to Plan** columns and select **Hide in Report View**.
+   5. Return to the **Report** view and create a measure that retrieves the filter context of the **Plan Scenario** column:
   
   ```
   Sales Plan Scenario Filter Branch =
       SWITCH(TRUE(),
           NOT(ISFILTERED('Plan Scenarios'[Plan Scenario])),
           "No Selection",
           NOT(HASONEFILTER('Plan Scenarios'[Plan Scenario])),
           "Multiple Selections",
           "Single Selection"
       )
   ```
+   6. Create a **Plan Amount** measure:
   
   ```
   Plan Amount = SUM('SubCatPlan'[Plan Amt])
   ```
+   7. Create an **Internet Sales Plan Scenario** measure that dynamically calculates a budget/plan amount based on the filter context (slicers, visuals):
   
   ```
   Internet Sales Plan Scenario =
       VAR FilterContext = [Sales Plan Scenario Filter Branch]
   RETURN
       SWITCH(TRUE(),
           FilterContext = "Single Selection",
               MIN('Plan Scenarios'[Var to Plan]) * [Plan Amount],
           FilterContext = "No Selection",
               [Plan Amount],
           FilterContext = "Multiple Selections",BLANK()
       )
   ```
+   8. Apply a currency format to the **Plan Amount** and **Internet Sales Plan Scenario** measures.
+   9. Create visualizations to display the different plan scenarios; an example is shown in Figure 7.7:

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_352_0.png)

Figure 7.7: Visualizations of plan scenarios

## How it works…

When using the DAX DATATABLE function in the Plan Scenarios table, the column names and types are declared, and each row is enclosed in curly braces—List values in M queries. Ideally, the new scenario table can be persisted within a data warehouse and the Power BI solution can be resilient to changes in scenario names and values.
