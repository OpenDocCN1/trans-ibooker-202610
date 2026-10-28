
From the list of data sources, let’s try and integrate with InfluxDB. On the settings page, depending on the InfluxDB edition you are integrating with, you will have an option of choosing either InfluxQL or Flux as your query language of choice. We will go ahead with Flux, as we have InfluxDB 2.0 running. See Figure 6-14.

![](img/6646c41cea18bb4f7d3c2c6513a4d992_204_0.png)

Figure 6-14. Selecting a query language

Then we will pass on the connection information so that InfluxDB can be reached from Grafana. We will also need to mention the organization, default bucket, connection token, and minimum time interval for Grafana to query InfluxDB. The default slider will mark InfluxDB as the default data source. See Figure 6-15.

![](img/6646c41cea18bb4f7d3c2c6513a4d992_207_0.png)

Figure 6-17. Adding Prometheus as a data source to Grafana

On saving and testing this connection, we should get a message that the connection has been successfully established. You should now be able to see the list of added data sources in the Configuration section. We have also added a pre-populated data source called TestData DB, which comes with Grafana. See Figure 6-18.

![](img/6646c41cea18bb4f7d3c2c6513a4d992_210_0.png)

Figure 6-21. Adding a new panel

A panel is therefore a visual representation of results from one or more queries. In order to run queries, you must have a data source added. Once the query is executed, you can apply various formatting options to arrive at the perfect visualization as per your need. Panels can be arranged easily on the dashboard by dragging, dropping, and resizing them.

When you click Add new panel (shown in Figure 6-21), the new panel opens up the panel editor. Panel editor is also known as the Edit Panel screen or panel edit mode. You can also reach the panel editor by clicking an existing panel title drop-down and then selecting Edit, which opens the panel in edit mode. You can also click anywhere on an existing panel and press e to open the panel editor.

## Panel Editor

Once you add a panel, it will look like Figure 6-22. The various sections have been suitably marked for clearer understanding from a to f.

![](img/6646c41cea18bb4f7d3c2c6513a4d992_212_0.png)

Figure 6-23. Fill visualization preview

- **Fit:** This option will fit the visualization preview in the available space preserving the aspect ratio of the panel. On resizing, it will try to restore the visualization preview to maintain the aspect ratio at all times. See Figure 6-24.

![](img/6646c41cea18bb4f7d3c2c6513a4d992_212_1.png)

Figure 6-24. Fit visualization preview

- **Exact:** This option will resize the visualization preview to the exact size of the dashboard. In case, due to resizing of the panes, not enough space is available, it will scale down the visualization to preserve the aspect ratio. See Figure 6-25.

![](img/6646c41cea18bb4f7d3c2c6513a4d992_213_0.png)

Figure 6-25. Exact visualization preview

- **Time range controls:** This option helps you to select the suitable time range that you are interested in for the visualization preview.
- **Time range zoom out:** This small button in the shape of a lens, adjacent to the time range control, helps in zooming out the time window. With each successive click, you can have a wider time range under your inspection.
- **Refresh:** The Refresh button helps in manually refreshing the data for the selected time range.
- **Refresh dashboard interval:** If you want the data in the visualization preview to refresh automatically in an interval selected from the drop-down (which can range from 10 seconds to 1 day), you can use this option. It will fetch new data at an interval defined in the drop-down. You can also select the Off option in case you choose not to refresh the data automatically.

![](img/6646c41cea18bb4f7d3c2c6513a4d992_215_0.png)

Figure 6-27. Query editor

In the Data Section - Query tab, you can select a data source from the list of integrated data sources and then write a query in the specific query language supported by that data source. The query language and its capabilities are different for each data source. In a single dashboard, we can have multiple panels all connected to different data sources but each panel can only be tied to one specific data source. You might want to apply data transformation in the Transform tab and set alert rules in the Alert tab.

- d) Panel Settings, Field, and Overrides (see Figure 6-28)

```
from(bucket: "monitoringcloudnativeapps")
|> range(start: v.timeRangeStart, stop: v.timeRangeStop)
|> filter(fn: (r) => r["_measurement"] == "cpu")
|> filter(fn: (r) => r["_field"] == "usage_system")
|> drop(columns: ["host"])
|> aggregateWindow(every: v.windowPeriod, fn: mean)
|> yield(name: "mean")
```

- **Dashboard settings:** The gear icon is used to access the dashboard settings.
- **Discard:** This option will let you discard all the changes made to the panel, returning it to the last saved state. It will also close the panel editor and return you to the dashboard.
- **Save:** As the name implies, this option saves the dashboard with all the changes made in the panel editor till then. While saving, there is a tick box to save the current time range as the default for the complete dashboard. You can add a note describing the changes from the last save.
- **Apply:** This option will let you apply the changes made to the dashboard, but to persist those changes you have to save the dashboard. It then closes the panel editor, returning you to the dashboard.

Let’s look at a panel for CPU usage of the system over the last 5 minutes (Figure 6-30).

![](img/6646c41cea18bb4f7d3c2c6513a4d992_217_0.png)

Figure 6-30. CPU panel

The data source for the panel is InfluxDB (shown as default as we selected InfluxDB as the default data source during data source addition). In accordance with the data source, the query editor can use Flux as a query language. In the query editor, we have written a single query (A) to fetch data from the bucket monitoringcloudnativeapps. The time range is from a variable passed on for start and stop time. Then, we have included filters to filter data on the basis of _measurement = cpu and _field = usage_system. We are dropping the column host, as this information would be redundant since I am running this on a single host. Lastly, we are calculating a function (fn: mean) over the aggregating window, which is also a variable.

The result is visible in the form of line graphs depicting the CPU utilization of various CPUs like cpu0, cpu1, and so on. In order to inspect a particular line, you can hover over it and the tooltip displays field value and tag value at that instant.

## Visualization

Grafana is renowned for its visualization prowess and the ease with which it can cater to any use case. In this section, we will try to explore all the different visualizations available in Grafana.

### Stat

The Stat panel is one single panel which shows a particular stat value rather than the common line graph, although the option to add a graph is also there. This is important when one single value reveals the requisite information and the value from 5 minutes back might not be so relevant. For example, the total number of requests served will serve as a good stat value and not as a line graph. See Figure 6-31.

![](img/6646c41cea18bb4f7d3c2c6513a4d992_218_0.png)

Figure 6-31. Stat

![](img/6646c41cea18bb4f7d3c2c6513a4d992_220_0.png)

Figure 6-34. Display settings

The visualization section has the option of selecting the most appropriate visualization tool for the interpretation of the data. There are many options here, but sometimes the data which has to be displayed should be able to easily put across the idea of the graph and the dashboard. The most important suggestion doled out by monitoring experts is that it doesn’t matter if your charts are beautiful; what matters is what they convey. So, selecting the right visualization is very critical.
