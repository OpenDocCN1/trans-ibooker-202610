
Rows that do not meet the retention policy have been removed from the result set. The execution plan for the query is shown in the following screenshot:

![](img/07d1225e179d5df9f2b2d26b026386c6_220_0.png)

When you look at the execution plan, you can see that the retention policy filter is automatically applied. Therefore, historical rows older than three days are removed from the result set and it contains one actual and one historical row. Rows that meet the following criteria are not shown:

```
Vt < DATEADD (Day, -3, SYSUTCDATETIME ());
```

However, when you query the history table directly, you can still see all rows:

```
SELECT * FROM dbo.T1
UNION ALL
SELECT * FROM dbo.T1_Hist;
```

The query returns the following result:

| Id | C1 | Vf | Vt |
| --- | ----- | ----------------------------- | ----------------------------- |
| 1 | 3 | 2017-12-15 00:00:00.0000000 | 9999-12-31 23:59:59.9999999 |
| 1 | 1 | 2017-12-01 00:00:00.0000000 | 2017-12-10 00:00:00.0000000 |
| 1 | 2 | 2017-12-10 00:00:00.0000000 | 2017-12-15 00:00:00.0000000 |

This is not what you expected! How does SQL Server remove the aged rows? Identification of matching rows and their removal from the history table occur transparently, alongside the background tasks that are scheduled and run by the system. Since aged rows can be removed at any point in time and in arbitrary order, you should use Transact-SQL extensions for querying temporal data, as shown in the previous section.

In this example, you have used three days in the retention policy. This information is stored in the sys.tables catalog. Run the following query to check retention policy settings for the T1 table:

```
SELECT temporal_type_desc, history_retention_period,
history_retention_period_unit
FROM sys.tables WHERE name = 'T1';
```

The previous query returns this result set:

| temporal_type_desc | history_retention_period | history_retention_period_unit |
| ------------------- | ------------------------- | ----------------------------- |
| SYSTEM_VERSIONED_TEMPORAL_TABLE | 3 | 3 |

The second 3 in the results represents days. If you include the other tables, you can see that all temporal tables in the WideWorldImporters database have a value of -1 in this column, which means that the retention period is not defined for a temporal table, thus the historical rows will be kept forever. Here is the list of possible retention period units you can define in an SQL Server 2017 retention policy:

+   - -1: INFINITE
+   - 3: DAY
+   - 4: WEEK
+   - 5: MONTH
+   - 6: YEAR

A history retention policy is good and lets you manage and control data size. However, currently you can use only one criteria to define aged rows: the end of period. If you have a lot of historical entries for some rows and a few for most of the others, this will not help you. It would be nice if you could choose additional criteria such as the number of rows or table size. You also cannot define where aged data will be moved; currently it is simply cleaned up.

## Custom history data retention

Unfortunately, you cannot configure a history table to automatically move data to some other table or repository according to a user-defined retention policy. That means that you have to implement this kind of data retention manually. This is not very complicated, but it is not a trivial action. I expected it as a part of the implementation and would be disappointed if this was not delivered in the next SQL Server version. As mentioned, you can use some other SQL Server features to implement data retention for history tables:

+   - **Stretch databases** allow you to move entire parts of historical data transparently to Azure.
+   - **Partitioning** history tables allows you to easily truncate the oldest historical data by implementing the sliding window approach.
+   - **Custom Cleanup** doesn't require any other features. You use Transact-SQL scripts to convert a temporal table to non-temporal, delete old data, and convert the table back to a temporal table.

![](img/07d1225e179d5df9f2b2d26b026386c6_222_0.png)

You can find more details about data retention at Books Online at the following address: https://docs.microsoft.com/en-us/sql/relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.

## History table implementation

As mentioned in the Creating temporal tables section, you can create the history table in advance, or let SQL Server create it for you. In the former case, the created table is a row stored table with a clustered index on period columns. If the current table does not contain data types that prevent the usage of data compression, the table is created with PAGE compression. This physical implementation is acceptable if you query the history table by using period columns as filter criteria. However, if your temporal queries usually look for historical records for individual rows, this is not a good implementation. To check potential performance issues of an automatic created history table, use the following code to create and populate a sample temporal table:

```
CREATE TABLE dbo.Mila
(
    Id INT NOT NULL IDENTITY (1,1) PRIMARY KEY CLUSTERED,
    C1 INT NOT NULL,
    C2 NVARCHAR(4000) NULL
)
GO
INSERT INTO dbo.Mila(C1, C2)  SELECT message_id, text FROM sys.messages
WHERE language_id = 1033;
GO 50

ALTER TABLE dbo.Mila
ADD ValidFrom DATETIME2 GENERATED ALWAYS AS ROW START NOT NULL CONSTRAINT DF_Mila_ValidFrom DEFAULT '20170101',
    ValidTo DATETIME2 GENERATED ALWAYS AS ROW END NOT NULL CONSTRAINT DF_Mila_ValidTo DEFAULT '99991231 23:59:59.9999999',
    PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo);
GO
ALTER TABLE dbo.Mila SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.Mila_History));
GO
```

As you can see, the INSERT statement is repeated 50 times, so that the sample table has a lot of rows (661,150 rows). At this point, the history table is empty; all rows are in the current table only. To create a history row for each row in the current table, and one additional history row for the row with the ID of 1, use the following command:

```
UPDATE dbo.Mila SET C1 = C1 + 1;
UPDATE dbo.Mila SET C1 = 44 WHERE Id = 1;
```

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

![](img/07d1225e179d5df9f2b2d26b026386c6_224_0.png)

You can see the **Index Seek** operator in the plan, but the plan is not efficient since it uses the `ValidTo` column as filter criteria. Observe the execution parameters:

```
SQL Server Execution Times:
   CPU time = 0 ms,  elapsed time = 0 ms.
SQL Server parse and compile time:
   CPU time = 0 ms,  elapsed time = 0 ms.

Table 'Mila_History'. Scan count 1, logical reads 10583, physical reads 0,
```

```
read-ahead reads 0, lob logical reads 0, lob physical reads 0, lob read-ahead reads 0.
Table 'Mila'. Scan count 0, logical reads 3, physical reads 0, read-ahead reads 0, lob logical reads 0, lob physical reads 0, lob read-ahead reads 0.
```

```
SQL Server Execution Times:
    CPU time = 188 ms,   elapsed time = 193 ms.
SQL Server parse and compile time:
    CPU time = 0 ms, elapsed time = 0 ms.
```

```
SQL Server Execution Times:
    CPU time = 0 ms,   elapsed time = 0 ms.
```

The query that returns a single row takes almost 200 milliseconds and SQL Server had to read more than 10,000 pages to generate this row! Since you have the Id column in the filter, it is clear that you need an index with this column as the leading column:

```
CREATE CLUSTERED INDEX ix_Mila_History ON dbo.Mila_History(Id, ValidTo, ValidFrom) WITH DROP_EXISTING;
```

When you execute the same query again, you can see the execution plan shown as follows:

![](img/07d1225e179d5df9f2b2d26b026386c6_225_0.png)

Execution plan with a custom index on the history table
