![](bbox=[0.18, 0.14, 0.78, 0.6])

Hmm. The relationship between Strength and Ratio definitely looks *nonlinear*, so fitting a regression line to this data does *not* seem like the right thing to do. We’ll remedy this in a moment, but, for illustrative purposes, let’s see what happens if we plow ahead with linear regression. Fit the model with smf.ols as usual, then look at some basic diagnostics:

```
>>> concrete_model = smf.ols("Strength ~ Ratio", data=concrete)
>>> concrete_fit = concrete_model.fit()
>>> concrete.Strength.corr( concrete.Ratio )

-0.6551873840990604

>>> concrete_fit.rsquared
```