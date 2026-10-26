To ensure that the same index can improve performance for JSON queries searching the other JSON properties and not only `FullName` (as in the case of the index on the computed column), let's execute another query that searches the `PhoneNumber` property:

```
```sql
SELECT id, info
FROM dbo.T1
WHERE CONTAINS(info,'NEAR(PhoneNumber,"(209) 555-0103")');
```
```

The execution plan is the same as for the previous query, as you can see in the following screenshot:

![](bbox=[0.105, 0.298, 0.895, 0.484])

The same index covers both queries. Unfortunately, JSON path expressions are not supported in the `CONTAINS` predicate; you can only search for property values, but it is better than scanning the whole table.

You can store and process small and moderate amounts of JSON data within SQL Server with good support of JSON functions and acceptable performance. However, if your JSON documents are large and you need to search them extensively, you should use a NoSQL solution, such as DocumentDB.

Ensure that you have dropped the table used in this exercise:

```
```sql
USE WideWorldImporters;
DROP TABLE IF EXISTS dbo.T1;
```
```