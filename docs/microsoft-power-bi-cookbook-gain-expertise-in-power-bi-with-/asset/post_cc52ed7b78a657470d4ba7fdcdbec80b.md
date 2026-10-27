
Mkt-Sales R Squared = [Mkt-Sales Correl]^2

MktSalesSlope = DIVIDE([MktSalesCorrelNum],[MktSalesCorrelDenomX])

MktSales Intercept = AVERAGE(MktSalesRegression[Internet Sales]) - ( [MktSalesSlope]*AVERAGE(MktSalesRegression[Marketing Amount]) )

3. Click New table from the ribbon of the Modeling tab and use the following code:
Residuals =
    VAR Intercept = [MktSales Intercept]
    VAR Slope = [MktSalesSlope]
RETURN
    ADDCOLUMNS(MktSalesRegression,"Y Intercept",Intercept,"Slope",Slope,
    "Predicted Internet Sales", ([Marketing Amount]*Slope) + Intercept,
    "Residual",[Internet Sales] - (([Marketing Amount]*Slope) + Intercept))

4. Create the following measures:
Residuals Amount = SUM(Residuals[Residual])

Residuals Average = CALCULATE(AVERAGE(Residuals[Residual]),ALL(Residuals))

Residuals Sample Std Dev = CALCULATE(STDEV.S(Residuals[Residual]),ALL(Residuals))

Residuals Z Score = DIVIDE([Residuals Amount] - [Residuals Average],[Residuals Sample Std Dev])

Regression Line Message = "Regression Line: Y= " & FORMAT([MktSalesSlope],"#,###") & "X" & "+" & FORMAT([MktSales Intercept],"#,###")

Last Month Internet Sales =
    VAR __LastMonth = MAX('Residuals'[YearMonthSort])
RETURN
    LOOKUPVALUE('Residuals'[Internet Sales],[YearMonthSort],__LastMonth)

Last Month Predicted Internet Sales =
    VAR __LastMonth = MAX('Residuals'[YearMonthSort])
RETURN
    LOOKUPVALUE('Residuals'[Predicted Internet Sales],[YearMonthSort],__LastMonth)

+   5. From the **Report** view, create card visuals to display the **Last Month Internet Sales**, **Last Month Predicted Internet Sales**, **Mkt-Sales Correl**, **Mkt-Sales R Squared**, and **Regression Line Message** measures
+   6. Create a scatter chart visualization using the **Marketing Amount** column (sum) from the **MktSalesRegression** table as the **X Axis**, the **Internet Sales** column (sum) from the **MktSalesRegression** table as the **Y Axis**, and the **YearMonth** column from the **MktSalesRegression** table as **Details**
+   7. Add a **Trend** line from the **Analytics** pane
+   8. Create a second scatter chart visualization using the **Predicted Internet Sales** column (sum) from the **Residuals** table as the **X Axis**, the **Residuals Z Score** column (sum) from the **Residuals** table as the **Y Axis**, and the **YearMonth** column from the **Residuals** table as **Details**

![](img/0b67f3eabca89f1e12f62eac78fb9ffb_428_0.png)

Figure 9.23: Regression report page

With this report design, the user can instantly perceive the strength of the relationship via the **Marketing Amount and Internet Sales by YearMonth** scatter chart and the high values for the correlation and R squared cards shown in *Figure 9.23*. The **Predicted Internet Sales and Residuals Z Score by YearMonth** scatter chart helps to identify the months with relatively large variations.

## How it works

In this example, we have obviously engineered a monthly marketing spend metric to have a high correlation with internet sales. Had we used the actual **Sales and Marketing** expenditure numbers from the general ledger (**FactFinance**) fact table, we would have found no correlation at all.

Simple (single-variable) regression models are often insufficient to estimate Y values accurately, but many of the concepts and techniques used in this recipe are applicable to more complex, multiple linear regression models.

The main correlation calculations deal with data in the **MktSalesRegression** table. This table creates rows with corresponding data between the two metrics we wish to correlate. The SUMMARIZECOLUMNS function groups the table at the monthly grain, and the FILTER function removes any rows (months) that do not have both internet sales and marketing values. It is crucial to the calculations that the data does not contain gaps in any one time series. The CALCULATETABLE function passes a filtered date table to SUMMARIZECOLUMNS in order to exclude certain months. The **Marketing Amount** and **Internet Sales** columns are the two measures we wish to find or not find a correlation between. Note that to attempt a correlation between different measures, only the **Marketing Amount** and **Internet Sales** expressions need to be changed—all other measures and DAX calculations can remain the same.

A new SQL view could be developed in the source system to meet the regression table requirements and, as another alternative, M queries within the dataset could leverage the existing general ledger, internet sales, and date queries. However, small DAX tables such as this example (with 36 rows) are a good option for supporting custom or advanced analysis and functionality.

The correlation coefficient is split into three separate intermediate measures (**MktSalesCorrelNum**, **MktSalesCorrelDenomX**, and **MktSalesCorrelDenomY**) and these measures are referenced in the **Mkt-Sales Correl** measure. With the correlation and its components defined in the model, the slope (**MktSalesSlope**) measure can leverage the same numerator measure, **MktSalesCorrelNum**, and the **MktSalesCorrelDenomX** measure as well. Finally, we can compute the y-intercept for the regression, **MktSales Intercept**.

We now have all of the components for the equation of our line, *y = mx + b*, where *m* is the slope and *b* is the y-intercept. This formula is what allows us to compute the **Predicted Internet Sales** in our **Residuals** table. The **Predicted Internet Sales** column in the **Residuals** table implements the equation of a line (*y = mx + b*) by referencing the **Marketing Amount** column (*x*), the slope (**MktSalesSlope**), and the y-intercept (**MktSales Intercept**). Finally, the **Residual** column is computed to subtract the predicted sales value from the observed (actual) value in the **Internet Sales** column.

A Z-score is computed for each residual data point (a month) to determine if the variation (or "miss") between the predicted and observed values is large relative to other data points. To support the visualization, a measure returns a text string containing the equation of the regression model's line. Additionally, two measures are created to display actual and predicted internet sales for the last month in the analysis.

The two calculated tables in this recipe, **MktSalesRegression** and **Residuals**, do not have any relationships to other tables in the model. Refreshing the source tables (queries) of the two DAX tables also refreshes the calculated tables.

## There's more

The created DAX measures in this recipe correspond to the CORREL, SLOPE, and INTERCEPT functions in Microsoft Excel. The documentation for these Excel functions provides the following formulas:

The formula for the CORREL function, for a sample (Pearson's Correlation Coefficient):

Correl (X, Y) = Σ(x − x̄)(y − ȳ) / √Σ(x − x̄)²(y − ȳ)²

The standard formula for finding the slope of a line:

b = Σ(x − x̄)(y − ȳ) / Σ(x − x̄)

The standard formula for finding a y-intercept:

a = ȳ − b x̄

Correlation coefficients can range from -1 to 1. A positive correlation between two metrics means that as one measure goes up or down, the other metric also goes up or down. There are also inverse correlations represented by negative numbers, where the two metrics are inversely correlated, meaning that when one metric goes up, the other metric goes down, and vice versa. Values close to 1 or -1 indicate that the two metrics have a strong correlation and directly affect one another. Values closer to 0 indicate that the two metrics are not related to one another.

The R squared value, or the coefficient of determination, is simply the square of the correlation coefficient. R squared values fall between 0 and 1 since the square of a negative number is positive. An R squared value of 1 indicates that the movements of one metric are 100% explained by the movements in the comparison metric, while a value of 0 indicates that there is no relation between the two metrics. R squared values between .85 and 1 are considered "high," meaning that the two metrics are closely linked, while anything below .7 is considered weak or of relatively little influence.

It is important to consider that time series data often exhibits seasonality, meaning that the numbers are artificially higher or lower than they would otherwise be due to, for example, quarterly influences. Think retail sales and holidays in the fourth calendar quarter like Thanksgiving and Christmas. When data exhibits seasonality, it is critically important that the metric's values are de-seasonalized before performing the correlation.

Another consideration is leading and lagging indicators. Leading indicators mean that the values in one time series are related to another, but there is an offset of some number of weeks or months. In other words, one metric may go up and correspond to the other metric going up, but there is a period of time or delay before the second metric reflects the movement of the first metric.
