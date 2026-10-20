because your data may be physically structured in a different way on the disk in the database.

![](bbox=[0.143, 0.173, 0.857, 0.302])

Figure 4-24. Results set from the take operator in KQL

TIP

In KQL, the take operator has a synonym called limit that works in the exact same way.

If you want a specific n number of rows, based on column sorting, you can add by <columnname> <optional sort type (desc or asc)> to your script as shown here:

```
Weather
| take 5 by EpisodeId
```

This gives you the results set shown in Figure 4-25.

![](bbox=[0.143, 0.695, 0.857, 0.824])

Figure 4-25. Results set from the take operator with added column sorting in KQL