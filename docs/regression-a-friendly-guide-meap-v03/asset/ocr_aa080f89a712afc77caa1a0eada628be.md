```
Ratio        16.31580
dtype: float64

>>> concrete_fit.rsquared

0.5258245635023568
```

The explicit fitted model is thus

Predicted Strength~ = ~12.59679+16.31580* Ratio.

The positive slope quantifies how compressive strength increases as the cement-to-water ratio increases, the correlation of 0.725 reflects the strong linear relationship shown in the scatterplot, and the coefficient of determination reveals a 53% reduction in predictive variability relative to the null model. We’ll return to this example in the next chapter to interrogate the model further, but the practical takeaway is already clear: using less water leads to stronger concrete!

# 2.3.2 Imputing fish weights

So far, we’ve avoided a fairly common problem in data analysis: missing or corrupted values. For various reasons, datasets are often incomplete; someone might not answer a question on a survey, a value might be recorded incorrectly or not at all, or the information just might not be available. Replacing missing data is called data imputation and can be a tricky business. One simple approach to imputation is to fill every missing entry with the average of the available observations for that variable— this is precisely how the null model predicts quantitative responses. As we’ve seen, a simple linear regression model can sometimes make much better predictions than the null model, and the next example demonstrates how this can provide an effective means of data imputation. Specifically, we’re going to use the linear relationship between the logarithms of the lengths and weights of fish to predict fish weights from their known lengths. We’ll need the resulting weights for an example in Chapter 4, in which we’ll explore the extinction risks for cartilaginous fish of different sizes.

We’ll use data from FishBase [7], a popular global database of fish species information. Fortunately, the folks at rOpenSci[8] have made the data easy to