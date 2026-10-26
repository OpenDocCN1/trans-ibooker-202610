
```
FROM dbo.Intervals
WHERE b <= @e AND b >= @b - @max
  AND e >= @b AND e <= @e + @max
OPTION (RECOMPILE);
```

This query retrieves the same rows as the previous one with 20 logical reads only. If you check the execution plan, you can see that the idx_b was used, with the seek predicate

Seek Keys[1]: Start: [tempdb].[dbo].[Intervals].b >= Scalar Operator((550)), End: [tempdb].[dbo].[Intervals].b <= Scalar Operator((590)), which successfully eliminated rows from both sides of the timeline, and then the residual predicate [tempdb].[dbo].[Intervals].[e]>= (570) AND [tempdb].[dbo].[Intervals].[e]<=(610) was used to select rows from a very limited partial scan.

Of course, the figure could be turned around to cover cases where the idx_e index would be more useful. With this index, elimination from the left is simple—eliminate all of the intervals that end at least one unit before the beginning of the given interval. This time, elimination from the right is more complex—the end of the intervals in the table cannot be more to the right than the end of the given interval plus the maximum length of all intervals in the table.

![](img/07d1225e179d5df9f2b2d26b026386c6_186_0.png)

> Please note that this performance is the consequence of the specific data in the table.

The maximum length of an interval is 20. This way, SQL Server can very efficiently eliminate intervals from both sides. However, if there were to be only one long interval in the table, the code would become much less efficient, because SQL Server would not be able to eliminate a lot of rows from one side, either left or right, depending on which index it would use. Anyway, in real life, interval length often does not vary a lot, so this optimization technique might be very useful, especially because it is simple.

After you finish with the temporal queries in this section, you can clean up your tempdb database with the following code:

```
DROP TABLE dbo.DateNums;
DROP TABLE dbo.Intervals;
```

## Temporal features in SQL:2011

Temporal data support was introduced in the SQL:2011—ANSI standard. There were also attempts to define support in the previous standard versions, but without success (TSQL2 extensions in 1995). They were not widely accepted and vendors did not implement them.

The SQL:2011 standard proposed that temporal data should be supported in relational database management systems. A very important thing is that SQL:2011 did not introduce a new data type to support temporal data; rather, it introduced the period.

A period is a table attribute and it's defined by two table columns of date type, representing start time and end time, respectively. It is defined as follows:

- A period must have a name.
- The end time must be greater than the start time.
- It is a closed-open period model. The start time is included in the period and the end time is excluded.

The SQL:2011 standard recognizes two dimensions of temporal data support:

- Application time or valid time tables
- System time or transaction time tables

**Application time period tables** are intended to meet the requirements of applications that capture time periods during which the data is believed to be valid in the real world. A typical example of such an application is an insurance application, where it is necessary to keep track of the specific policy details of a given customer that are in effect at any given point in time.

**System-versioned tables** are intended to meet the requirements of applications that must maintain an accurate history of data changes either for business reasons, legal reasons, or both. A typical example of such an application is a banking application, where it is necessary to keep previous states of customer account information so that customers can be provided with a detailed history of their accounts. There are also plenty of examples where certain institutions are required by law to preserve historical data for a specified length of time to meet regulatory and compliance requirements.

**Bitemporal tables** are tables that implement both application time and system-versioned time support.

After the standard was published, many vendors came up with the temporal table implementation:

- **IDM DB2 10** added full support for temporal tables (for both application time and system-versioned).
- **Oracle** implemented a feature called the **Flashback Data Archive (FDA)**. It automatically tracks all changes made to data in a database and maintains an archive of historical data. Oracle 12c introduced valid time temporal support.
- **PostgreSQL** does not support temporal tables natively, but temporal tables approximate them.
- **Teradata** implements both valid time and transaction time table types based on the TSQL2 specification.

All these implementations most probably affected Microsoft's decision to implement temporal tables in SQL Server 2016.

## System-versioned temporal tables in SQL Server 2017

SQL Server 2016 introduces support for system-versioned temporal tables. Unfortunately, application-time tables are not implemented neither in this version, nor in SQL Server 2017. System-versioned temporal tables bring built-in support for providing information about data stored in the table at any point in time rather than only the data that is correct at the current moment in time. They are implemented according to the specifications in the ANSI SQL:2011 standard with a few extensions.

## How temporal tables work in SQL Server 2017

A system-versioned temporal table is implemented in SQL Server 2017 as a pair of tables: the current table containing the actual data, and the history table where only historical entries are stored. There are many limitations for both current and history tables. Here are limitations and considerations that you must take into account for the current table of a system-versioned temporal table:

- It must have a primary key defined
- It must have one `PERIOD FOR SYSTEM_TIME` defined with two `DATETIME2` columns
- Cannot be `FILETABLE` and cannot contain `FILESTREAM` data type
- `INSERT`, `UPDATE`, and `MERGE` statements cannot reference and modify period columns; the start column is always set to system time, the end column to max date value
- `INSTEAD OF` triggers are not allowed
- `TRUNCATE TABLE` is not supported

The list of limitations for a history table is significantly longer and brings many additional restrictions. The following applies to the history table of a system-versioned temporal table:

- Cannot have constraints defined (primary or foreign keys, check, table, or column constraints). Only default column constraints are allowed.
- You cannot modify data in the history table.
- You can neither `ALTER` nor `DROP` a history table.
- It must have the same schema as the current table (column names, data types, ordinal position).
- Cannot be defined as the current table.
- Cannot be `FILETABLE` and cannot contain `FILESTREAM` data type.
- No triggers are allowed (neither `INSTEAD OF` nor `AFTER`).
- `Change Data Capture` and `Change Data Tracking` are not supported.

You can read more about considerations and limitations when working with temporal tables at https://msdn.microsoft.com/en-us/library/mt604468.aspx.

The list might look long and discouraging, but all these limitations are there to protect data consistency and accuracy in history tables. However, although you cannot change logical attributes, you can still perform actions related to the physical implementations of the history table: you can switch between rowstore and columnstore table storage, you can choose columns for clustered indexes, and you can create additional non-clustered indexes.

## Creating temporal tables

To support the creation of temporal tables, the `CREATE TABLE` and `ALTER TABLE` Transact-SQL statements have been extended.
