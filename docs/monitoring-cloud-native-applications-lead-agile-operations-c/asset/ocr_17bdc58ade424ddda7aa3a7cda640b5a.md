![](bbox=[0.07, 0.105, 0.894, 0.48])

# Figure 6-27. Query editor

In the Data Section - Query tab, you can select a data source from the list of integrated data sources and then write a query in the specific query language supported by that data source. The query language and its capabilities are different for each data source. In a single dashboard, we can have multiple panels all connected to different data sources but each panel can only be tied to one specific data source. You might want to apply data transformation in the Transform tab and set alert rules in the Alert tab.

+   d) Panel Settings, Field, and Overrides (see Figure 6-28)

```
from(bucket: "monitoringcloudnativeapps")
|> range(start: v.timeRangeStart, stop: v.timeRangeStop)
|> filter(fn: (r) => r["_measurement"] == "cpu")
|> filter(fn: (r) => r["_field"] == "usage_system")
|> drop(columns: ["host"])
|> aggregateWindow(every: v.windowPeriod, fn: mean)
|> yield(name: "mean")
```