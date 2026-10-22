y - \bar{y} = (y - \hat{y}) + (\hat{y} - \bar{y}).

This shows that the deviation between an observation Y and the average response \bar{y} is made up of two parts, the residual that we’ve been analyzing and the difference between the predictions of the two competing models. The coefficient of determination quantifies the relative sizes of these two terms. Specifically, we do the usual thing and manipulate the sums of squares:

\sum (y - \bar{y})^2 = \sum \{ (y - \hat{y}) + (\hat{y} - \bar{y}) \}^2 = \sum (y - \hat{y})^2 - 2 \sum (y - \hat{y})(\hat{y} - \bar{y}) + \sum (\hat{y} - \bar{y})^2.

It might look like we’re making things messier and more complicated, but the mathematical forces of good are on our side— the middle term on the right vanishes:

\sum (y - \hat{y})(\hat{y} - \bar{y}) = \sum \hat{y}(y - \hat{y}) - \bar{y} \sum (y - \hat{y}) = 0.

When we went through the derivation of the regression coefficients earlier, we pointed out that these sums involving the residuals are both 0, two marvelous consequences of the least squares method. The payoff now is that

\sum (y - \bar{y})^2 = \sum (y - \hat{y})^2 + \sum (\hat{y} - \bar{y})^2, \quad so that \sum (\hat{y} - \bar{y})^2 = \sum (y - \bar{y})^2 - \sum (y - \hat{y})^2 = TSS - SSR.

This last identity guarantees that incorporating the predictor into our model decreases variability, with an absolute reduction equal to TSS - SSR; this quantity cannot be negative since it equals a sum of squares. To make this statistic easier to interpret, the coefficient of determination expresses the reduction in variability as a relative proportion, namely:

R^2 = \frac{\sum (y - \bar{y})^2 - \sum (y - \hat{y})^2}{\sum (y - \bar{y})^2} = 1 - \frac{\sum (y - \hat{y})^2}{\sum (y - \bar{y})^2} = 1 - \frac{SSR}{TSS}.

Having computed the relevant ingredients for the UNICEF data earlier, we can check that this formula agrees with the africa_fit model’s rsquared value:

# Listing 2.7. Directly calculating the coefficient of determination

```
>>> 1 - (africa_ssr / africa_tss)
```