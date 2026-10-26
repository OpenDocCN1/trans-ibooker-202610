You can see that SQL Server used row mode processing. As mentioned, batch mode is not strictly bound to columnar storage; however, it is much more likely that SQL Server would use it as you use the columnar storage. The following code creates a filtered nonclustered columnstore index. It is actually empty:

```
```sql
CREATE NONCLUSTERED COLUMNSTORE INDEX NCCI_FactTest
ON dbo.FactTest
    (SaleKey, CustomerKey,
     Customer, CityKey, City,
     DateKey, StockItemKey,
     Product, Quantity,
     TotalAmount, Profit)
WHERE SaleKey = 0;
GO
```
```

Now, execute the same query again. As you can see from the following screenshot, this time batch mode is used for the Hash Match (Partial Aggregate) operator:

![](bbox=[0.201, 0.414, 0.801, 0.829])