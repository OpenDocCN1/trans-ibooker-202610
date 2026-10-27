The forecast now includes a forecasted value for the current (incomplete) month. Applying the **Seasonality** variable at its known grain (12 per year) overrides the default seasonality factor.

When the seasonality (points per cycle) is known, it is recommended to apply this value manually to improve accuracy. Hovering over a forecast data point exposes both the forecast and the upper and lower boundary values, given the 95 percent confidence interval.

In the second example, the last completed **Week Ending Date** is 1/25/14. Therefore, given **5** points to ignore, this point and the four previous weeks are excluded from the forecasting algorithm, such that the forecast can only use the weeks ending on 12/21/13 and earlier to generate its projections. Three additional forecast points (8 (Forecast Length) − 5 (Ignore Last)) are computed for the weeks ending on 2/1/14, 2/8/14, and 2/15/14.

# ### There's more

In addition to the forecasting feature, the line chart visualization also now includes an anomaly detection feature. To see how this works, do the following:

+   1. Ensure that the **Anomaly detection** feature is activated in the **Preview** features by selecting **File | Options and settings | Options**, and then **Preview features**
+   2. Copy the **Week Ending Date** line chart to a new page
+   3. Remove **Forecast** in the **Analytics** pane
+   4. Expand **Find anomalies** and choose **Add**

![](bbox=[0.197, 0.537, 0.869, 0.695])

Figure 9.35: Find anomalies

The default **Sensitivity** is 70 percent. Increasing the **Sensitivity** identifies more anomalies.

# ### See also

+   - Data Forecasting and Analytics with Power BI Desktop: https://bit.ly/3xFWakx
+   - Anomaly detection tutorial: https://bit.ly/3t6NAiC