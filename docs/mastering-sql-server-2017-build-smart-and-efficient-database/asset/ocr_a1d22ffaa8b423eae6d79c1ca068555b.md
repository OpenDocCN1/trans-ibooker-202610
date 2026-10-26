Note again that values in the `ValidFrom` and `ValidTo` columns are displayed in short format for clarity. The value in the `ValidTo` column in the history table is identical to the `ValidFrom` value in the current table; there are no gaps.

![](bbox=[0.135, 0.189, 0.208, 0.247])

The start and end time period columns store time in the UTC time zone!

Now, assume that you reduced the price the next day to 180. Here is the code for this action:

```
```sql
UPDATE dbo.Product SET Price = 180.00 WHERE ProductId = 1;-- on 29th
November
```
```

The state of the current table is as follows after the preceding statement's execution:

| ProductId | ProductName | Price | ValidFrom | ValidTo |
|---|---|---|---|---|
| 1 | Fog | 180 | 29.11.2017 | 31.12.9999 |

The state of the history table is as follows after the preceding statement's execution:

| ProductId | ProductName | Price | ValidFrom | ValidTo |
|---|---|---|---|---|
| 1 | Fog | 150 | 12.11.2017 | 28.11.2017 |
| 1 | Fog | 200 | 28.11.2017 | 29.11.2017 |

You can see another entry in the history table indicating that the price 200 was valid for one day. What would happen if you execute the same statement again, say on 30th November? There is no real change; no business logic attributes are changed, but what does it mean for temporal tables? Here is the code for this action:

```
```sql
UPDATE dbo.Product SET Price = 180.00 WHERE ProductId = 1;-- on 30th
November
```
```

The state of the current table is as follows after the preceding statement's execution:

| ProductId | ProductName | Price | ValidFrom | ValidTo |
|---|---|---|---|---|
| 1 | Fog | 180 | 30.11.2017 | 31.12.9999 |