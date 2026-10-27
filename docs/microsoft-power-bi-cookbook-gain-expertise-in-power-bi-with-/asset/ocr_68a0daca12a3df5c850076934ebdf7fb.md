The `ForecastDateBridge` and `ForecastTerritoriesBridge` tables each contain unique values for `YearMonth` and `SalesTerritoryGroup`, respectively. These tables are used in the model to bridge the different granularities between the Dates table, the Sales Territories table, and the Sales Forecast table.

The `Total Sales Amount` measure determines whether the measure is executing in a current or future YearMonth, and calculates the value by either summing the SalesAmount column from the Internet Sales table for current YearMonth values, or by summing the Forecast Sales column from the Sales Forecast table for future YearMonth values.

# There's more...

The forecasting can be modified to include a what-if parameter that can be chosen by the user to visualize beating the forecast by specified percentages. To demonstrate how this works, perform the following steps:

+   1. In the **Report** view, choose **New parameter** from the ribbon of the **Modeling** tab.
+   2. Name the parameter `Beat Forecast By`, choose `Decimal number` for the data type, set **Minimum** to `0.1`, **Maximum** to `0.4`, and **Increment** to `0.1`.

![](bbox=[0.199, 0.442, 0.867, 0.819])

Figure 7.10: What-if parameter