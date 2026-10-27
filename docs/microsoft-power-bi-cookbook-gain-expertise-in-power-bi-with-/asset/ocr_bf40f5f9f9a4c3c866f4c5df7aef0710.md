```
{"City", "StateProvinceCode", "EnglishCountryRegionName", "PostalCode", "DimSalesTerritory"},
{"City", "StateProvinceCode", "EnglishCountryRegionName", "PostalCode", "DimSalesTerritory"}),
ExpandSalesTerritory = Table.ExpandRecordColumn(ExpandGeography, "DimSalesTerritory", {"SalesTerritoryRegion"}, {"SalesTerritoryRegion"}),
DupCity = Table.DuplicateColumn(ExpandSalesTerritory, "City", "City1"),
DupZip = Table.DuplicateColumn(DupCity, "PostalCode", "Zip"),
Address = Table.CombineColumns(DupZip, {"AddressLine1", "City1", "StateProvinceCode"}, Combiner.CombineTextByDelimiter(", ", QuoteStyle.None),"Address"),
FullAddress = Table.CombineColumns(Address,{"Address", "Zip"}, Combiner.CombineTextByDelimiter(" ", QuoteStyle.None),"Full Address")
in
FullAddress
```

+   8. Create a **Products** query using the following code:

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

+   9. Move the **Internet Sales** query to a **Facts** query group and the **Sales Territories**, **Dates**, **Customers**, and **Products** queries to a **Dimensions** query group

+   10. Choose **Close & Apply** from the ribbon of the **Home** tab