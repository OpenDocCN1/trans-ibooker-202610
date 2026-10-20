You can add more sorting by other columns by using a comma (,) and typing in the names of the columns you need. You can also type **asc** to put them in ascending order or leave them in descending order (desc), which is the default.

# Row-Based Calculations

When implementing the requirements for row-based calculations, the KQL script language is very similar to the T-SQL language.

For example, in the Weather table, you can add the DamageCrop and DamageProperty columns by using the extend operator. This operator adds a new column to the existing results set. Here’s an example script:

```
Weather
| extend TotalDamage = DamageCrop + DamageProperty
```

This script gives you the results set shown in Figure 4-31. Look at the column on the far right.

![](bbox=[0.14, 0.61, 0.86, 0.79])

Figure 4-31. Adding a new column by using the extend operator in KQL

In the previous example script, we made a very simple addition of two columns. Of course, we can do more advanced column manipulation. For example, we can use