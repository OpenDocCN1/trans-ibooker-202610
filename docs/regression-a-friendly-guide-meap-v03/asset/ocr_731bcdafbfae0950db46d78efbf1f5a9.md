the previous section:

```
cohorts_fit.deviance
```

```
375.7650234849703
```

```
cohorts_fit.df_resid
```

```
99
```

```
1 - st.chi2.cdf( cohorts_fit.deviance, cohorts_fit.df_resid )
```

```
0.0
```

This $p$-value is practically zero, advising us to reject the null hypothesis that the model fits the data well. Does this really mean that our model is terrible? Not at all! This means that the model could be improved dramatically by adding parameters, which makes perfect sense: we’re comparing a simple binomial logistic model with 2 parameters to a saturated model with 101 parameters, so it would be shocking if our tiny model were competitive with the larger model. Also, we know from our previous work that this simple binomial model does a good job of summarizing the relationship between ages and chronic conditions, so we shouldn’t interpret the deviance and its $p$-value in isolation. You should explore this further by grouping the responses in different ways, building various binomial logistic models, and seeing what happens with the deviances and $p$-values (see Exercise 7.1.2). As you play around with this and other examples, think carefully about the conventional wisdom that you should just compare the model’s deviance to $(n - p)$, the average value of the related $\chi^2$ distribution; there is a rule of thumb that a model with deviance less than $(n - p)$ fits well while a model with a larger deviance fits poorly, but context and common sense matter, too!