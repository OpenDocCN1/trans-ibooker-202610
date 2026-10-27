The **Currency Measures** provide an example of computing the geometric mean at the day, month, and year grains. In the **Last Rate** measure, the LASTNONBLANK function is needed for days where there might not be a rate logged in the data. This measure is used to compute the previous rate at the day, month, and year granularity (**Previous Daily Rate**, **Previous Monthly Rate**, and **Previous Year Rate**). The percentage change measures, **Daily Change %**, **Monthly Change %**, and **Yearly Change %** are the source values for the geometric mean calculations and thus are expressed as positive numbers. The GEOMEANX function iterates over tables at the different grains and computes the Change % measure for each row. The geometric mean (the product of values taken to the Nth root) is computed last against this list of values. The geometric change measures can be used in a multi-row card visualization, with 10 decimal points specified:

| -0.0108542283 | -0.0011481367 | -0.0000349297 |
| Yearly Geometric Change | Monthly Geometric Cha... | Daily Geometric Change |

Figure 3.29: Geometric Mean Measures

# There's more...

A common scenario with many data models is comparing tables with different grains, for example, when comparing actual versus budget, where actuals are reported daily and budgets are assigned monthly or quarterly. In such scenarios, you can create bridge tables where each bridge table contains the unique values of the dimension at the grain of the budget table. These bridge tables can potentially leverage the existing M queries in the model used to load the dimension tables. Take the following example, where the M expression references the **Dates** table query as its source and selects only the distinct values of the **Calendar Yr-Mo** column. If this is the grain of the budget table, this single-column table could be used as the bridge table.

```
let Source = Dates,
    YearMonthColumn = Table.SelectColumns(Source,{"Calendar Yr-Mo"}),
    RemoveDuplicates = Table.Distinct(YearMonthColumn)
in RemoveDuplicates
```

Alternatively, a simple SQL view could be created ('Select Distinct [Calendar Yr-Mo] From dbo.DimDate') that selects the distinct values of the column; this view could be accessed from a new bridge table M query.

One-to-many relationships with single-direction cross-filtering from the bridge tables to the budget table can be created. Additional many-to-one relationships with bidirectional cross-filtering are then created between the bridge tables and their corresponding dimension tables. An example final model might look the same as that shown in Figure 3.30.