```
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
```

+   2. Click on **New Parameter** from the **Manage Parameters** dropdown in the ribbon of the **Home** tab.

+   3. Create a parameter called **Territory Group** with a **Text** data type that uses the **SalesTerritoryGroupsList** query for **Suggested Values**:

![](bbox=[0.199, 0.377, 0.868, 0.633])

Figure 7.4: Territory Group parameter

+   4. Create a **SalesTerritoryKeys** query that selects the unique key values associated with this dimension using the value set by the **Territory Group** parameter and disable the load:

```
```powerquery
let
    Source = AdWorksDW,
    Navigation = Source{[Schema="dbo",Item="DimSalesTerritory"]}[Data],
    ParamFilter = Table.SelectRows(Navigation, each [SalesTerritoryGroup] = #"Territory Group"),
    KeyColumn = Table.Distinct(Table.SelectColumns(ParamFilter,{"SalesTerritoryKey"}))
in
    KeyColumn
```
```