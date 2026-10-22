
*sum of the residuals is zero,*

```
∑_{x, y} (y − a − b x) = − ∑_{x, y} (y − ŷ) = − 0.
```

Remember this important fact! Now determine b:

```
∂/∂b (SSR) = −2 ∑_{x, y} x(y − a − b x) = −0  ⟹  b = − ∑_{x, y} (x − x̄)(y − ȳ) / ∑_{x, y} (x − x̄)² = − r (s_y / s_x).
```

Exercise 5.2.1 will guide you through the algebraic manipulations needed here. The preceding equation guarantees that the *weighted sum of the residuals is zero,*

```
∑_{x, y} x(y − a − b x) = − ∑_{x, y} x(y − ŷ) = −0,
```

another important fact to file away for later use. Finally, since both ∂²/∂a² (SSR) and ∂²/∂b² (SSR) are positive, we know that the SSR achieves its minimum at these critical values of a and b.

Applying these formulas for the optimal slope and intercept to the UNICEF IMR data, we find that

```
>>> m = africa_corr*(s_y/s_x)
>>> m

    -0.8204100566323119

>>> b = y_bar - m*x_bar
>>> b

    119.60222615990713
```

so the equation of the *line of best fit* is

Predicted IMR = 119.6022 - .8204 * Literacy.

This is the *regression line*, also known as the *least squares line*. To check the fit visually, here are the UNICEF data and the regression line together:

```
>>> sns.scatterplot(data=africa, x="Literacy", y="IMR")
>>> plt.axline( (x_bar,y_bar), slope = m, color="black")
>> plt.title("IMR vs Literacy: Line of best fit")
```

Figure 2.11. Literacy rates and infant mortality, with regression line

![](img/f25e2c651d2b8852d1c723f946646ff0_45_0.png)

For comparison, here are the data, the regression line, and the SD line in one scatterplot, with their intersection at the point of averages clearly marked:

```
sns.scatterplot(data=africa, x="Literacy", y="IMR")
plt.axline( (x_bar,y_bar), slope = m, color="black")
plt.axline( (x_bar, y_bar), slope = m_sd, linestyle='--', color="
plt.title("IMR vs Literacy: Regression line (solid) and SD line (
```

```
plt.plot(x_bar,y_bar,'ok')
```

## Figure 2.12. Literacy rates and infant mortality, with regression line and SD line

![](img/f25e2c651d2b8852d1c723f946646ff0_46_0.png)

It would be difficult to determine just by looking that the regression line fits the data better than the SD line, but now we know that the SD line is too steep and therefore has too much error. By minimizing the sum of squared residuals, the regression line has the smallest possible overall error (in the least squares sense) and therefore provides more accurate predictions, on average, than any other line. The actual value of the SSR is important, as it measures the uncertainty of these predictions and provides a key model

diagnostic, the residual standard error (see below). To explore this without doing a bunch of unnecessary calculations, it’s time to start using the statsmodels library to construct linear regression models— while the formulas above for the coefficients are interesting and instructive, no one uses them much in real life! To make things as simple as possible, we’ll use the statsmodels.formula.api interface, imported above as smf. Here’s how to prepare the model, fit it, and take a first look at the results:

```
>>> africa_model = smf.ols('IMR ~ Literacy', data = africa)
>>> africa_fit = africa_model.fit()
>>> africa_fit.params
```

```
Intercept      119.602226
Literacy       -0.820410
dtype: float64
```

The syntax here corresponds to what we want: smf.ols('IMR ~ Literacy', data = africa) means that we want to use ordinary least squares (ols) to fit a simple linear model to the africa data, with IMR as the response and Literacy as the predictor. We see the intercept, 119.6022, and the slope, -0.8204, that we just computed from the formulas above, but fitting the model this way is better for many reasons:

- this is faster, easier, and less error-prone,
- smf.ols can be used to construct regression models with multiple predictors, as we’ll see later, and
- the results, stored in africa_fit, contain much more information than just the coefficients!

Before expanding on this last point, let’s back up a bit and take another look at the null model. Its constant prediction is easy enough to compute by averaging the observed responses, but we can also use smf.ols to get it. After all, the null model is a line— with a slope of 0— and smf.ols fits lines to data. We just have to specify a slope of 0 and fit the best constant, i.e., the best horizontal line:

```
>>> africa_null = smf.ols('IMR ~ 1', data = africa)
>>> africa_null = africa_null.fit()
>>> africa_null.params
```

Intercept      58.106383
dtype: float64

There’s the null model! You can decide which method you prefer to get it.

Now let’s see what else is contained in africa_fit. The residuals are easy to access and analyze; for example, we can compute the SSR directly from the residuals:

```
>>> africa_ssr = (africa_fit.resid**2).sum()
>>> africa_ssr
```

21280.530941116467

As you might expect, you don’t have to compute the SSR this way, as it’s provided in the results:

```
>>> africa_fit.ssr
```

21280.530941116467

As mentioned earlier, squaring the residuals before adding them eliminates cancellations, which is good, but it also squares the response variable’s measurement units, making the SSR harder to interpret. To get a more useful error metric with the same units as the original observations, we average the squared residuals and take the square root, obtaining the residual standard error (RSE):

RSE = sqrt(1/(n - 2) SSR) = sqrt(1/(n - 2) sum_y (y - ŷ)^2).

We divide by ( n - 2) here because computing the 2 coefficients in the model sacrifices 2 degrees of freedom. In general, when we use n data points to compute p coefficients in a regression model, there are ( n - p) degrees of freedom— the same thing happens with the variance and standard deviation, since we use up 1 degree of freedom by calculating the average, leaving (n - 1) degrees of freedom for those calculations.

For the UNICEF IMR data, the RSE is

```
>>> np.sqrt( africa_ssr / (n_countries - 2) )

    21.746279856112537
```

The RSE is about 21.75, while the standard deviation of the response variable — the analogous quantity for the null model— is about 27.31, as found above. This is one way to compare these 2 models: infant mortality rate predictions based on the regression line will typically be off by about 21.75 deaths or so, but predictions with the null model will be off by about 27.31 deaths. That is definitely an improvement!

Since the RSE is such a useful measure of the model’s fit, it should come as no surprise that smf.ols bundles what we need to get the RSE with the other information in africa_fit. What might be surprising is that smf.ols calls the quantity SSR / (n − 2) the model’s scale parameter[6], so we get the RSE by taking its square root:

```
>>> np.sqrt( africa_fit.scale )

    21.746279856112537
```

Be aware that other people and other books might refer to the RSE by another name, such as the standard error of regression; more important than the choice of lingo, though, is understanding what the RSE does— it quantifies the variability of the responses relative to the regression line.

Another essential— and widely misinterpreted!— regression diagnostic is the coefficient of determination, denoted R² and accessed as you would then expect:

```
>>> africa_fit.rsquared

    0.379729491613339
```

What does this number do? Its whole job is to compare the simple linear regression model and the null model. To explain how it works, we have to slog through some calculations— this might seem tedious, but it’s the only way to understand the coefficient of determination properly. So grab an espresso, a pencil, and some paper and consider the following decomposition:

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

### Listing 2.7. Directly calculating the coefficient of determination

```
>>> 1 - (africa_ssr / africa_tss)
```
