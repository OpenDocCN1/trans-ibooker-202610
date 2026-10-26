# Adding B-tree indexes and constraints

There is still one query, the point query, which needs additional optimization. In SQL Server 2016 and 2017, you can create regular, rowstore B-tree nonclustered indexes on a clustered columnstore index, on a table that is organized as columnar storage. The following code adds a nonclustered index with an included column, an index that is going to cover the point query:

```
CREATE NONCLUSTERED INDEX NCI_FactTest_CustomerKey
ON dbo.FactTest (CustomerKey)
INCLUDE (Profit);
GO
```

Before executing the queries, let's check the space used by the demo fact table:

```
EXEC sys.sp_spaceused N'dbo.FactTest', @updateusage = N'TRUE';
GO
```

The result is as follows:

| Name | rows | reserved | data | index_size | unused |
|------|------|----------|------|------------|--------|
| dbo.FactTest | 2279810 | 90256 KB | 19344 KB | 70192 KB | 720 KB |

You can see that row storage uses much more space than columnar storage. However, a regular NCI is very efficient for seeks. Let's test the queries, starting with the simple query:

```
SET STATISTICS IO ON;
SELECT f.StockItemKey,
SUM(f.TotalAmount) AS Sales
FROM dbo.FactTest AS f
WHERE f.StockItemKey < 30
GROUP BY f.StockItemKey
ORDER BY f.StockItemKey;
```

This query still needed 23 LOB logical reads. If you check the execution plan, you can see that SQL Server is still using the columnstore index scan. Of course, the NCI is not very useful for this query. How about the complex query?:

```
SELECT f.SaleKey,
f.CustomerKey, f.Customer, cu.[Buying Group],
f.CityKey, f.City, ci.Country,
f.DateKey, d.[Calendar Year],
f.StockItemKey, f.Product,
f.Quantity, f.TotalAmount, f.Profit
FROM dbo.FactTest AS f
INNER JOIN Dimension.Customer AS cu
```