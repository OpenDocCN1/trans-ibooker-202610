Of course, it returns the same result set and the execution plans are identical, as shown in the following screenshot:

![](bbox=[0.105, 0.164, 0.894, 0.431])

Under the hood, the query processor touches both tables and retrieves data, but the query with temporal extensions looks simpler.

The special case of a point-in-time query against a temporal table is a query where you specify the actual date as the point in time. The following query returns actual data from the same temporal table:

```
```sql
DECLARE @Now AS DATETIME = SYSUTCDATETIME();
SELECT PersonID, FullName, OtherLanguages, ValidFrom, ValidTo
FROM Application.People FOR SYSTEM_TIME AS OF @Now;
```
```

The previous query is logically equivalent to this one:

```
```sql
SELECT PersonID, FullName, OtherLanguages, ValidFrom, ValidTo FROM
Application.People;
```
```