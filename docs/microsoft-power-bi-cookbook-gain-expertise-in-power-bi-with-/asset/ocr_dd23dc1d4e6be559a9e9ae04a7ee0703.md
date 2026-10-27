# Applying Advanced Analytics and Custom Visuals

+   7. In the **Report** view, create another **Line chart** visualization using **Week Ending Date** (not the hierarchy) from the **Dates** table as the **Axis**, and the **Internet Net Sales** measure as the **Values**
+   
+   8. With the visualization selected, use the **Filters** pane and add the **IsCurrentWeek** column to the filters for the visualization, and exclude values of **1**
+   
+   9. With the line chart selected, open the **Analytics** pane, expand **Forecast**, and click **Add**
+   
+   10. Enter **8** for **Forecast** length, **5** for **Ignore last**, and **52** for **Seasonality**

![](bbox=[0.199, 0.252, 0.868, 0.408])

Figure 9.34: Weekly sales trend and three-week forecast, which excludes the prior 5 completed weeks

# How it works

The Power BI Forecast tool uses the exponential smoothing time series predictive algorithm. This method is widely used in multiple domains and helps to suppress outlier values while efficiently capturing trends. The forecast tool is currently only available to the **Line chart** visual, and only one measure (line) on this visual is allowed. The x-axis value needs to have a date or date/time data type or be a uniformly increasing whole number and a minimum of six date points are required.

The Forecast tool in Power BI includes an **Ignore last** feature, which allows the exclusion of incomplete periods (months, weeks, and days) from the forecast. However, for common additive measures, such as **Sales Amount**, not filtering out the current period often significantly detracts from the usability of the visual, given the steep decline represented by the current (incomplete) period. Dynamically updated date columns, such as **IsCurrentWeek**, generally resolve this issue.

In the first line chart, by default, a forecast of the chosen measure is created for 10 points (months) in the future with a 95 percent **Confidence interval**. The forecast automatically determines the step (monthly grain) and also determines a seasonality factor to apply to the forecast. In this example, no filters have been applied to the report, and thus the current month, which is incomplete, is initially being used by the forecast. By setting **Ignore last** to **1** point we exclude the most current, incomplete month.