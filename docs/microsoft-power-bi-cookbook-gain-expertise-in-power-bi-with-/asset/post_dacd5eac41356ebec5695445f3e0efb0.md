
5. Create a new blank query called **StartDate** that computes a date value based on the **CurrentDate** query and the new parameter, and then disable the load:

    ```powerquery
    let
        Source = Date.AddDays(CurrentDate, - #"Days Prior to Current Date")
    in
        Source
    ```

    In this example, a date 365 days prior to the current date is returned based on the default parameter value.

6. Create an **Internet Sales** query based on the following M code:

    ```powerquery
    let
        Source = AdWorksDW,
        dbo_FactInternetSales =
            Source{[Schema="dbo",Item="FactInternetSales"]}[Data],
        ChangeType = Table.TransformColumnTypes(
            dbo_FactInternetSales,{{"OrderDate", type date}}),
        RowFilter = Table.SelectRows(
            ChangeType,
            each [OrderDate] >= StartDate and [OrderDate] <= CurrentDate)
    in
        SalesTerritoryJoin
    ```

7. Click on **Close & Apply** from the **Home** tab of Query Editor.

8. Optionally, build a report or query against the refreshed fact table to validate the filter.

9. From the **Home** tab of the **Report** view, click on the **Transform data** dropdown in the ribbon and select **Edit Parameters**:

    ![](img/0b67f3eabca89f1e12f62eac78fb9ffb_335_0.png)

    Figure 7.3: Editing parameters from the Report view

10. Either select a suggested value from the drop-down menu or enter a number in the input box and click on **OK**.

11. Click on **Apply Changes** from the warning dialog. StartDate and the fact table queries impacted by the parameter change will both be refreshed.

## How it works...

In a production scenario, the CurrentDate query would not be hardcoded to a date and would be something similar to the following:

```powerquery
let
    Source = DateTime.Date(DateTime.LocalNow())
in
    Source
```

The StartDate query uses the parameter to calculate a date prior to the CurrentDate by a number of days selected from the list of values. Parameters can be referenced within queries, just like referencing other queries or steps in a query, in which the same rules also apply. When parameters include spaces in their names, referencing the parameter must be preceded by a hash (#) and double quotes, and suffixed by double quotes (#"Days Prior to Current Date"). Parameters that do not include spaces in their names can be referenced directly without the prefix or suffix.

The Internet Sales query uses the Table.SelectRows function in the RowFilter step to filter the Fact.InternetSales table using the OrderDate column to be greater than or equal to StartDate (a date value). It is important that the data type of the OrderDate column and the data type of the StartDate query match one another. For example, Date data types cannot be compared to DateTime data types.

Currently, parameters cannot be created or edited in the Power BI service. The parameter values configured when published will be used for each refresh.

When considering your own scenarios, identify candidates for query parameters, such as hardcoded date filters and dimension attributes with few distinct values (for example, department groups). Identify scenarios in which certain business users or teams require edit rights to a dataset (that is, source queries, model relationships, and measures), but only need a small, highly filtered model for self-service development.

## There's more...

Queries can be affected by multiple parameters. In addition, parameters can also use queries for their Suggested Values. To see how to use multiple parameters in a query, as well as how to use a query to provide Suggested Values for a parameter, do the following:

1. In **Power Query Editor**, create a query called **SalesTerritoryGroupsList** that returns the distinct **Sales Territory Groups** from the **DimSalesTerritory** table and disable loading of the query:

    ```powerquery
    let
        Source = AdWorksDW,
        Navigation = Source{[Schema="dbo",Item="DimSalesTerritory"]}[Data],
        RemoveColumns = Table.SelectColumns(Navigation,{"SalesTerritoryGroup"}),
        FilterNA = Table.SelectRows(RemoveColumns, each ([SalesTerritoryGroup] <> "NA")),
        RemoveDups = Table.Distinct(FilterNA),
        List = Table.ToList(RemoveDups)
    in
        List
    ```

2. Click on **New Parameter** from the **Manage Parameters** dropdown in the ribbon of the **Home** tab.

3. Create a parameter called **Territory Group** with a **Text** data type that uses the **SalesTerritoryGroupsList** query for **Suggested Values**:

    ![](img/0b67f3eabca89f1e12f62eac78fb9ffb_337_0.png)

    Figure 7.4: Territory Group parameter

4. Create a **SalesTerritoryKeys** query that selects the unique key values associated with this dimension using the value set by the **Territory Group** parameter and disable the load:

    ```powerquery
    let
        Source = AdWorksDW,
        Navigation = Source{[Schema="dbo",Item="DimSalesTerritory"]}[Data],
        ParamFilter = Table.SelectRows(Navigation, each [SalesTerritoryGroup] = #"Territory Group"),
        KeyColumn = Table.Distinct(Table.SelectColumns(ParamFilter,{"SalesTerritoryKey"}))
    in
        KeyColumn
    ```

5. Within the Internet Sales table query, create an inner join expression step against the new SalesTerritoryKeys query:

    ```powerquery
    let
        Source = AdWorksDW,
        dbo_FactInternetSalesReason = Source{[Schema="dbo",Item="FactInternetSales"]}[Data],
        ChangeType = Table.TransformColumnTypes(dbo_FactInternetSalesReason, {{"OrderDate", type date}}),
        RowFilter = Table.SelectRows(ChangeType, each [OrderDate] >= StartDate and [OrderDate] <= CurrentDate),
        SalesTerritoryJoin = Table.Join(RowFilter,"SalesTerritoryKey",SalesTerritoryKeys,"SalesTerritoryKey",JoinKind.Inner)
    in
        SalesTerritoryJoin
    ```

The Internet Sales table is now filtered by both the Days Prior to Current Date parameter and the Territory Group parameter. Instead of a static list of values, Territory Group derives Suggested Values from another query in the model – the SalesTerritoryGroupsList query. For queries to be used as Suggested Values in a parameter, the query must return a List data type, and not a table. The Table.ToList function is used within the SalesTerritoryGroupsList query to transform the single-column table into a list.

The SalesTerritoryKeys query returns a single column table with the distinct SalesTerritoryKey values that correspond to the value chosen for the Territory Group parameter. This table is used in a Table.Join expression within the Internet Sales query to filter the Internet Sales table to only include the sales territories chosen using the Territory Group parameter.

## See also

- Using parameters: https://bit.ly/37rXLP6
- Edit parameter settings in the Power BI service: https://bit.ly/3iuC1sn

## Working with templates

Power BI templates can be created from Power BI Desktop files as a means of providing users and other report authors with access to pre-defined metadata, such as M queries, DAX measures, model relationships, and report visualizations. As the template files do not contain actual data, they are very lightweight and, for import mode models, data is only retrieved when the template is opened. Additionally, if query parameters have been configured, a user interface is provided for entering parameter values, and these parameters can be integrated with the source queries and other components of the dataset.

In this recipe, a parameter and supporting query are added to a Power BI Desktop file to support the distribution of a Power BI template.

### Getting ready

To prepare for this recipe, follow these steps:

1. Download `CH7_R3_Start.pbix` from the following GitHub repository: `https://github.com/PacktPublishing/Microsoft-Power-BI-Cookbook-Second-Edition`.
2. Edit the AdWorksDW query to point to your copy of the AdventureWorksDW2019 database.

### How to work with templates

In this example, the goal is to provide a Power BI template that only retrieves data for a single customer country. To implement this recipe, perform the following steps:

1. Open `CH7_R3_Start.pbix` in Power BI Desktop.
2. Choose **Transform data** from the ribbon of the **Home** tab to open **Power Query Editor**.
3. In the **Queries** pane, right-click an empty area and choose **New Group** to create a new query group called **Parameters**.
4. Right-click **Other Queries** and create a List query called **CustomerCountries** to retrieve the **Country** names from the **Customers** query:

    ```powerquery
    let
        Source = Customers,
        SelectColumn = Table.SelectColumns(Source,{"Country"}),
        Distinct = Table.Distinct(SelectColumn),
        List = Table.ToList(Distinct)
    in
        List
    ```

5. Disable the load for the **CustomerCountries** query.
6. Right-click the **Parameters** query group and choose **New Parameter**.
7. Create a parameter called **Customer Country** with **Text** as the type, **Suggested Values** for the **CustomerCountries** query, and **United States** as the current value.
8. In the **Other Queries** group, create a query called **CustomerKeys** that references the **Customer Country** parameter, and disable the load on this query:

    ```powerquery
    let
        CountryParamFilter = Table.SelectRows(Customers, each [Country] = #"Customer Country"),
        CustomerKeys = Table.SelectColumns(CountryParamFilter,
    ```
