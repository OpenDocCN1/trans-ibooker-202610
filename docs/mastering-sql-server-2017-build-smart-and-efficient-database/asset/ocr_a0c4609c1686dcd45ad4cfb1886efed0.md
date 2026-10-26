Rows that do not meet the retention policy have been removed from the result set. The execution plan for the query is shown in the following screenshot:

![](bbox=[0.105, 0.164, 0.895, 0.712])

When you look at the execution plan, you can see that the retention policy filter is automatically applied. Therefore, historical rows older than three days are removed from the result set and it contains one actual and one historical row. Rows that meet the following criteria are not shown:

```
Vt < DATEADD (Day, -3, SYSUTCDATETIME ());
```