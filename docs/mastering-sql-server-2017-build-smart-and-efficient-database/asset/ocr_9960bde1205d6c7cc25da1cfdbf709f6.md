This is different compared to the execution plan SQL Server used for the same query when the NCCI was empty, when SQL Server used batch mode operators. The ignore option really means that SQL Server completely ignores the NCCI. You can check that this is also true for the other two queries, the complex and the point one. You can also check the execution plans when ignoring the NCCI with the complex and point queries:

```
-- Complex query
SELECT f.SaleKey,
    f.CustomerKey, f.Customer, cu.[Buying Group],
    f.CityKey, f.City, ci.Country,
    f.DateKey, d.[Calendar Year],
    f.StockItemKey, f.Product,
    f.Quantity, f.TotalAmount, f.Profit
FROM dbo.FactTest AS f
    INNER JOIN Dimension.Customer AS cu
        ON f.CustomerKey = cu.[Customer Key]
    INNER JOIN Dimension.City AS ci
        ON f.CityKey = ci.[City Key]
    INNER JOIN Dimension.[Stock Item] AS s
        ON f.StockItemKey = s.[Stock Item Key]
    INNER JOIN Dimension.Date AS d
        ON f.DateKey = d.Date
OPTION (ignore_nonclustered_columnstore_index);
-- Point query
SELECT CustomerKey, Profit
FROM dbo.FactTest
WHERE CustomerKey = 378
OPTION (ignore_nonclustered_columnstore_index);
SET STATISTICS IO OFF;
```

For both queries, SQL Server did a full table scan on the fact table, with around 63,000 logical reads in this table. Now let's finally see how the queries can benefit from the NCCI. The first query is the simple query again:

```
SET STATISTICS IO ON;
SELECT f.StockItemKey,
    SUM(f.TotalAmount) AS Sales
FROM dbo.FactTest AS f
WHERE f.StockItemKey < 30
GROUP BY f.StockItemKey
ORDER BY f.StockItemKey;
```