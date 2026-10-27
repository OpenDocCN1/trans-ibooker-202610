+   5. Rename the query Internet Sales and choose the FactInternetSales table

+   6. Repeat steps 4 and 5, creating a Dates query that points to the DimDates table, and a Sales Territories query that points to the DimSalesTerritory table

+   7. Create a Customers query using the following code:

```
let
    Source = AdWorksDW,
    dbo_DimCustomer = Source[{Schema="dbo",Item="DimCustomer"}][Data],
    ExpandGeography = Table.ExpandRecordColumn(dbo_DimCustomer,
    "DimGeography",
        {"City", "StateProvinceCode", "EnglishCountryRegionName",
        "PostalCode",
        "DimSalesTerritory"},
        {"City", "StateProvinceCode", "EnglishCountryRegionName",
        "PostalCode",
            "DimSalesTerritory"}),
    ExpandSalesTerritory = Table.ExpandRecordColumn(ExpandGeography,
    "DimSalesTerritory", {"SalesTerritoryRegion"},
    {"SalesTerritoryRegion"}),
    DupCity = Table.DuplicateColumn(ExpandSalesTerritory, "City",
    "City1"),
    DupZip = Table.DuplicateColumn(DupCity, "PostalCode", "Zip"),
    Address = Table.CombineColumns(DupZip,
        {"AddressLine1", "City1", "StateProvinceCode"},
        Combiner.CombineTextByDelimiter(" ", QuoteStyle.None),"Address"),
    FullAddress = Table.CombineColumns(Address,{"Address", "Zip"},
        Combiner.CombineTextByDelimiter(" ", QuoteStyle.None),"Full Address")
    in
        FullAddress
```

+   8. Create a Products query using the following code:

```
let
    Source = AdWorksDW,
    Navigation = Source[{Schema="dbo",Item="DimProduct"}][Data],
    Subcategory =
        Table.ExpandRecordColumn(
            Navigation, "DimProductSubcategory",
            {"EnglishProductSubcategoryName", "DimProductCategory"},
            {"Product Subcategory", "DimProductCategory"}
        ),
    Category =
        Table.ExpandRecordColumn(
            Subcategory, "DimProductCategory",
```