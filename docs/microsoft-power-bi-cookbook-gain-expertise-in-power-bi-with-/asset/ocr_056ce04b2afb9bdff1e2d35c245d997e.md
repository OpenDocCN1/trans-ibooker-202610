# There's more

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