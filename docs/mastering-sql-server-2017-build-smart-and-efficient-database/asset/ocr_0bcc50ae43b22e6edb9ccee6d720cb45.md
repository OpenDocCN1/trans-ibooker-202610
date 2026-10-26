Now, assume that you want to check the state of the row with the ID of 1 at some time in the past. Ensure also that query statistics parameters are set to on:

```
SET STATISTICS IO ON;
SET STATISTICS TIME ON;
SELECT * FROM dbo.Mila FOR SYSTEM_TIME AS OF '20170505 08:00:00' WHERE Id = 1;
```

This query returns one row:

| Id | C1 | C2 | ValidFrom | ValidTo |
|---:|---:|---|---|---|
| 1 | 21 | Warning:Fatal error %d occurred | 2017-01-01 00:00 | 2017-12-13 16:57 |

However, it is most interesting to check the execution parameters. The execution plan for the query is shown as follows:

![](bbox=[0.105, 0.405, 0.895, 0.686])

You can see the **Index Seek** operator in the plan, but the plan is not efficient since it uses the `ValidTo` column as filter criteria. Observe the execution parameters:

```
SQL Server Execution Times:
   CPU time = 0 ms,  elapsed time = 0 ms.
SQL Server parse and compile time:
   CPU time = 0 ms,  elapsed time = 0 ms.

Table 'Mila_History'. Scan count 1, logical reads 10583, physical reads 0,
```