new column name by typing in the name you want before the aggregation function, as in this example:

```
Weather
| summarize SumOfDamageCrops = sum(DamageCrops) by State
```

You can see the new column name in Figure 4-34.