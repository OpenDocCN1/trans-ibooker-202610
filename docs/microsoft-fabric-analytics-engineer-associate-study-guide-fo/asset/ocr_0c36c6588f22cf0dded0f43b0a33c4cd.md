|  | 123 OrderID ↑ | ABC CustomerID |
|---|---|---|
| 1 | 1 | C001 |
| 2 | 2 | C002 |
| 3 | 3 | C001 |
| 4 | 4 | C003 |
| 5 | 5 | C004 |
| 6 | 6 | NULL |

*Figure 3-22. Table containing only unique rows due to the DISTINCT statement in the SQL query*

This table is quite straightforward, but often, we’ll deal with much larger tables in which it’s much harder to spot duplicates in the data. To find out if there are any duplicates, we can write another query that counts, for example, every row where a specific column appears more than once:

```
SELECT OrderID, COUNT(*) AS Count
FROM orders_with_issues
GROUP BY OrderID
HAVING COUNT(*) > 1;
```

This SQL query helps identify duplicates in the orders_with_issues table. It groups all rows by the OrderID column and counts the number of times each OrderID appears. If an OrderID appears more than once, it’s considered a duplicate. The HAVING COUNT(*) > 1 code filters the results so that only the duplicates are shown. The result is a list of OrderIDs that occur multiple times, along with the frequency of their appearance. Figure 3-23 shows the results of this query for our example.