![](bbox=[0.132, 0.081, 0.868, 0.412])

Figure 4.3: United Kingdom report page

Based on user feedback, a KPI visualization with a monthly trend and a goal measure is used to provide greater context compared to Card or Gauge visualizations. Overall, a standard visualization type with advantages in visual perception and additional analysis context was chosen. For example, KPIs were used instead of Card visuals; gauges, treemaps, and pie charts were avoided. A clustered column chart was chosen over a line chart for **Internet Sales to Plan**, given the importance of the individual values for each month. At a glance, each country report page also offers values with the KPIs visuals updating automatically.

Additional design decisions include the following:

+   - The slicer visuals as well as the **Play** axis on the scatter chart are organized on the left to support user filter selections.
+   - By hovering over the bubbles in the map or the bars in the chart, additional measures are exposed as tooltips.
+   - A textbox is used for the page titles, and the last refreshed date footer is a Card visual. An M query with the `DateTime.LocalNow` function is retrieved and passed to a DAX measure, returning text.
+   - The chart titles are customized, and the y-axis is removed when possible.