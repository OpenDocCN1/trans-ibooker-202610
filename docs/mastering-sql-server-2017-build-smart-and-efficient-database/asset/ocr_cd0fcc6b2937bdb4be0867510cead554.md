On first glance, SQL Server system-versioned tables might be the solution to the SCD problem when using the type 2 implementation. However, this is typically not true. In a data warehouse, most of the time you need to implement a mixed solution: type 1 for some attributes and type 2 for others. The granularity of the time points in system-versioned tables is 100 nanoseconds; in a data warehouse, typical granularity is 1 day. In the source system, the same entity, like customer, can be updated multiple times per day. You can have multiple historical rows for the same entity in a single day. Therefore, when transferring the data from a transactional database to a data warehouse, you need to take care to transfer the last state for each day. The following query illustrates the problem:

```
USE WideWorldImporters;
SELECT PersonID, FullName,
    ValidFrom, ValidTo
FROM Application.People
    FOR SYSTEM_TIME ALL
WHERE IsEmployee = 1
    AND PersonID = 14;
```

In the WideWorldImporters database, Application.People is a system-versioned table. The previous query returns all rows for an employee called Lily Code. Here is the abbreviated result:

| PersonID | FullName | ValidFrom | ValidTo |
| 14 | Lily Code | 2016-05-31 23:14:00.0000000 | 9999-12-31 23:59:59.9999999 |
| 14 | Lily Code | 2013-01-01 00:00:00.0000000 | 2013-01-01 08:00:00.0000000 |
| 14 | Lily Code | 2013-01-01 08:00:00.0000000 | 2013-01-19 08:00:00.0000000 |
| 14 | Lily Code | 2013-01-19 08:00:00.0000000 | 2013-02-14 08:00:00.0000000 |

You can see that this person has multiple rows for a single date. For example, there are two rows for Lily where the ValidTo date (just the date part) equals 2013-01-01. You need to select only the last row per employee per day. This is done with the following query. You can run it and check the results:

```
WITH PersonCTE AS
(
SELECT PersonID, FullName,
    CAST(ValidFrom AS DATE) AS ValidFrom,
    CAST(ValidTo AS DATE) AS ValidTo,
    ROW_NUMBER() OVER(PARTITION BY PersonID, CAST(ValidFrom AS Date)
        ORDER BY ValidFrom DESC) AS rn
FROM Application.People
    FOR SYSTEM_TIME ALL
```