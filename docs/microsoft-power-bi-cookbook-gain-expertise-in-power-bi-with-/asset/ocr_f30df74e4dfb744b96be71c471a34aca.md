In this recipe, a parameter and supporting query are added to a Power BI Desktop file to support the distribution of a Power BI template.

# Getting ready

To prepare for this recipe, follow these steps:

+   1. Download `CH7_R3_Start.pbix` from the following GitHub repository: `https://github.com/PacktPublishing/Microsoft-Power-BI-Cookbook-Second-Edition`.
+   2. Edit the AdWorksDW query to point to your copy of the AdventureWorksDW2019 database.

# How to work with templates

In this example, the goal is to provide a Power BI template that only retrieves data for a single customer country. To implement this recipe, perform the following steps:

+   1. Open `CH7_R3_Start.pbix` in Power BI Desktop.
+   2. Choose **Transform data** from the ribbon of the **Home** tab to open **Power Query Editor**.
+   3. In the **Queries** pane, right-click an empty area and choose **New Group** to create a new query group called **Parameters**.
+   4. Right-click **Other Queries** and create a List query called **CustomerCountries** to retrieve the **Country** names from the **Customers** query:

```
let
    Source = Customers,
    SelectColumn = Table.SelectColumns(Source,{"Country"}),
    Distinct = Table.Distinct(SelectColumn),
    List = Table.ToList(Distinct)
in
    List
```

+   5. Disable the load for the **CustomerCountries** query.
+   6. Right-click the **Parameters** query group and choose **New Parameter**.
+   7. Create a parameter called **Customer Country** with **Text** as the type, **Suggested Values** for the **CustomerCountries** query, and **United States** as the current value.
+   8. In the **Other Queries** group, create a query called **CustomerKeys** that references the **Customer Country** parameter, and disable the load on this query:

```
let
    CountryParamFilter = Table.SelectRows(Customers, each [Country] = #"Customer Country"),
    CustomerKeys = Table.SelectColumns(CountryParamFilter,
```