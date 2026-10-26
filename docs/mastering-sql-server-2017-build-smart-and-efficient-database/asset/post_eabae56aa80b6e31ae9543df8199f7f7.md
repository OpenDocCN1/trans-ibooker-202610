
Let me first introduce the notation I will use. I will work on two intervals, denoted as *i₁* and *i₂*. The beginning time point of the first interval is *b₁*, and the end is *e₁*; the beginning time point of the second interval is *b₂* and the end is *e₂*. Allen's **Boolean operators** are defined in the following table:

| Name | Notation | Definition |
|---|---|---|
| Equals | (*i₁ = i₂*) | (*b₁ = b₂*) AND (*e₁ = e₂*) |
| Before | (*i₁ before i₂*) | (*e₁ < b₂*) |
| After | (*i₁ after i₂*) | (*i₂ before i₁*) |
| Includes | (*i₁ ⊇ i₂*) | (*b₁ ≤ b₂*) AND (*e₁ ≥ e₂*) |
| Properly includes | (*i₁ ⊃ i₂*) | (*i₁ ⊃ i₂*) AND (*i₁ ≠ i₂*) |
| Meets | (*i₁ meets i₂*) | (*b₂ = e₁ + 1*) OR (*b₁ = e₂ + 1*) |
| Overlaps | (*i₁ overlaps i₂*) | (*b₁ ≤ e₂*) AND (*b₂ ≤ e₁*) |
| Merges | (*i₁ merges i₂*) | (*i₁ overlaps i₂*) OR (*i₁ meets i₂*) |
| Begins | (*i₁ begins i₂*) | (*b₁ = b₂*) AND (*e₁ ≤ e₂*) |
| Ends | (*i₁ ends i₂*) | (*e₁ = e₂*) AND (*b₁ ≥ b₂*) |

In addition to Boolean operators, three of Allen's operators accept intervals as input parameters and return an interval. These operators constitute simple **interval algebra**. Note that these operators have the same name as relational operators you are probably already familiar with: `Union`, `Intersect`, and `Minus`. However, they don't behave exactly like their relational counterparts.

In general, using any of the three interval operators, if the operation will result in an empty set of time points or in a set that cannot be described by one interval, then the operator should return `NULL`. A union of two intervals makes sense only if the intervals meet or overlap. An intersection makes sense only if the intervals overlap. The `Minus` interval operator makes sense only in some cases. For example, (3:10) Minus (5:7) returns `NULL` because the result cannot be described by one interval.

The following table summarizes the definitions of the operators in interval algebra:

| Name | Notation | Definition |
|---|---|---|
| Union | $(i_1 \text{ union } i_2)$ | $(Min(b_1, b_2) : Max(e_1, e_2))$, when $(i_1 \text{ merges } i_2)$; NULL otherwise |
| Intersect | $(i_1 \text{ intersect } i_2)$ | $(Max(b_1, b_2) : Min(e_1, e_2))$, when $(i_1 \text{ overlaps } i_2)$; NULL otherwise |
| Minus | $(i_1 \text{ minus } i_2)$ | $(b_1 : Min(b_2 - 1, e_1))$, when $(b_1 < b_2)$ AND $(e_1 \le e_2)$; $(Max(e_2 + 1, b_1) : e_1)$, when $(b_1 \ge b_2)$ AND $(e_1 > e_2)$; NULL otherwise |

The following figure shows the interval algebra operators graphically:

![](img/07d1225e179d5df9f2b2d26b026386c6_180_0.png)

## Temporal constraints

Depending on the business problem you are solving, you might need to implement many temporal constraints. Remember that for application time, SQL Server does not help you much. You need to implement the constraints in your code, using SQL Server declarative constraints where possible. However, most of the constraints you need to implement through custom code, either in triggers or in stored procedures, or even in the application code.

Imagine the Suppliers table example. One supplier can appear multiple times in the table because the same supplier could be under contract for separate periods of time. For example, you could have two tuples like this in the relation with the shortened header Suppliers (supplierid, companyname, from, to):

```
```text
{2, Supplier VHQZD, d05, d07}
{2, Supplier VHQZD, d12, d27}
```
```

Here are some possible constraints you might need to implement:

- *To* should never be less than *from*
- Two contracts for the same supplier should not have overlapping time intervals
- Two contracts for the same supplier should not have abutting time intervals
- No supplier can be under two distinct contracts at the same point in time
- There should be no supplies from a supplier at a point in time when the supplier is not under contract

You might find even more constraints. Anyway, SQL Server 2016 brings support for system versioned tables only. To maintain application validity times, you need to develop the code by yourself.

## Temporal data in SQL Server before 2016

As mentioned, in SQL Server versions before 2016, you need to take care of temporal data by yourself. Even in SQL Server 2016, you still need to take care of the human, or application, times. The following code shows an example of how to create a table with validity intervals expressed with the b and e columns, where the beginning and the end of an interval are represented as integers. The table is populated with demo data from the WideWorldImporters.Sales.OrderLines table:

```
```sql
USE tempdb;
GO
SELECT OrderLineID AS id,
```

```
StockItemID * (OrderLineID % 5 + 1) AS b,
LastEditedBy + StockItemID * (OrderLineID % 5 + 1) AS e
INTO dbo.Intervals
FROM WideWorldImporters.Sales.OrderLines;
-- 231412 rows
GO
ALTER TABLE dbo.Intervals ADD CONSTRAINT PK_Intervals PRIMARY KEY(id);
CREATE INDEX idx_b ON dbo.Intervals(b) INCLUDE(e);
CREATE INDEX idx_e ON dbo.Intervals(e) INCLUDE(b);
GO
```

Please note also the indexes created. The two indexes are optimal for searches at the beginning of an interval or on the end of an interval. You can check the minimum beginning and maximum end of all intervals with the following code:

```
SELECT MIN(b), MAX(e)
FROM dbo.Intervals;
```

You can see in the results that the minimum beginning time point is 1 and the maximum end time point is 1155. Now you need to give the intervals some time context. In this case, a single time point represents a day. The following code creates a date lookup table and populates it. Note that the starting date is July 1, 2014:

```
CREATE TABLE dbo.DateNums
(n INT NOT NULL PRIMARY KEY,
 d DATE NOT NULL);
GO
DECLARE @i AS INT = 1,
 @d AS DATE = '20140701';
WHILE @i <= 1200
BEGIN
INSERT INTO dbo.DateNums
(n, d)
SELECT @i, @d;
SET @i += 1;
SET @d = DATEADD(day,1,@d);
END;
GO
```

Now you can join the `dbo.Intervals` table to the `dbo.DateNums` table twice, to give context to the integers that represent the beginning and the end of the intervals:

```
SELECT i.id,
    i.b, d1.d AS dateB,
    i.e, d2.d AS dateE
FROM dbo.Intervals AS i
    INNER JOIN dbo.DateNums AS d1
        ON i.b = d1.n
    INNER JOIN dbo.DateNums AS d2
        ON i.e = d2.n
ORDER BY i.id;
```

The abbreviated result from the previous query is:

| id | b   | dateB      | e   | dateE      |
|----|-----|------------|-----|------------|
| 1  | 328 | 2015-05-24 | 332 | 2015-05-28 |
| 2  | 201 | 2015-01-17 | 204 | 2015-01-20 |
| 3  | 200 | 2015-01-16 | 203 | 2015-01-19 |

Now you can see which day is represented by which integer.

## Optimizing temporal queries

The problem with temporal queries is that when reading from a table, SQL Server can use only one index, successfully eliminate rows that are not candidates for the result from one side only, and then scan the rest of the data. For example, you need to find all intervals in the table that overlap with a given interval. Remember, two intervals overlap when the beginning of the first one is lower than or equal to the end of the second one, and the beginning of the second one is lower than or equal to the end of the first one, or mathematically when $(b_1 \le e_2)\ AND\ (b_2 \le e_1)$.

The following query searches for all of the intervals that overlap with the interval $(10, 30)$. Note that the second condition $(b_2 \le e_1)$ is turned around to $(e_1 \ge b_2)$ for simpler reading (the beginning and the end of intervals from the table are always on the left side of the condition). The given or searched interval is at the beginning of the timeline for all intervals in the table:

```
SET STATISTICS IO ON;
DECLARE @b AS INT = 10,
    @e AS INT = 30;
SELECT id, b, e
FROM dbo.Intervals
```

There is a solution that uses that index to eliminate rows from both sides of the searched interval by using a single index. The following figure shows this logic:

![](img/07d1225e179d5df9f2b2d26b026386c6_185_0.png)

Optimizing a temporal query

The intervals in the figure are sorted by the lower boundary, representing SQL Server's usage of the idx_b index. Eliminating intervals from the right side of the given (searched) interval is simple: just eliminate all intervals where the beginning is at least one unit bigger (more to the right) of the end of the given interval. You can see this boundary in the figure denoted with the rightmost dotted line. However, eliminating from the left is more complex. In order to use the same index, the idx_b index for eliminating from the left, I need to use the beginning of the intervals in the table in the WHERE clause of the query. I have to go to the left side, away from the beginning of the given (searched) interval at least for the length of the longest interval in the table, which is marked with a callout in the figure. The intervals that begin before the left-hand yellow line cannot overlap with the given (blue/dark gray) interval.

Since I already know that the length of the longest interval is 20, I can write an enhanced query in a quite simple way:

```
DECLARE @b AS INT = 570,
    @e AS INT = 590;
DECLARE @max AS INT = 20;
SELECT id, b, e
```
