

>>> train.Births.corr( train.Enrollment )

    0.9741680367357998

Now *that* is a strong correlation! Combining this quantitative measure with what we see in the scatterplot, we know that fitting a linear regression model to this data is the right thing to do. Here’s the result:

```
>>> train_model = smf.ols("Enrollment ~ Births", data=train)
>>> train_fit = train_model.fit()
>>> train_fit.params

    Intercept    -17.912152
    Births        8.802197
    dtype: float64
```

```
>>> train_fit.rsquared

    0.9490033637976821
```

```
>>> np.sqrt( train_fit.scale )

    0.388207377323538
```

The regression equation is therefore

```
Predicted enrollment = -17.912152 + 8.802197 * Births,
```

where the enrollment year is 20 years after the birth year. The slope tells us to expect enrollments to change by about 8.8 million students for every 1 million additional or fewer births from year to year— enrollments will likely increase in 20 years when there are more births and will likely decrease in 20 years when there are fewer births. The RSE, 0.388 and change, tells us how enrollments are likely to vary relative to this line— individual enrollments are unlikely to stray more than 2 * RSE ≈ .78 million students or so from the value predicted by the regression model. To get this model’s predictions for 2019 and beyond, we can feed the NCES birth data since 1999 to the model’s predict method:

```
>>> predictions = enroll.query("BirthYear > 1998").filter(["Birth
>>> predictions = predictions.assign( Enrollment = train_fit.pred
>>> sns.regplot(data=train, x="Births", y="Enrollment",
        ci=None, line_kws={"color":"black"})
```

```
>>> sns.scatterplot(data=predictions, x="Births", y="Enrollment")
>>> plt.xlim(3.4,4.4)
>>> plt.xlabel("Births (millions)")
>>> plt.ylabel("Enrollment (millions)")
>>> plt.title("Observed and predicted enrollments versus births")
```

## Figure 2.22. Predicted enrollments for 2019 and beyond

![](img/f25e2c651d2b8852d1c723f946646ff0_73_0.png)

As the code shows, the vertical coordinates of the orange points are the predicted enrollments. We have to be careful, though— see how the regression line (shown in black) terminates before reaching the 3 red points in the upper right corner? These 3 points are an example of extrapolation, the

crime of making predictions using values of the explanatory variable that are outside the range of values used to build the regression model. In this case, the values of Births used to construct train_fit range from 3.49 million to 4.16 million, but there were more than 4.16 million births in 2006, 2007, and 2008:

```
>>> predictions.query("Enrollment > 19")
```

|   | BirthYear | Births   | EnrollmentYear | Enrollment |
|---|-----------|----------|----------------|------------|
| 44 | 2006      | 4.265555 | 2026           | 19.634103  |
| 45 | 2007      | 4.316233 | 2027           | 20.080181  |
| 46 | 2008      | 4.247694 | 2028           | 19.476887  |

Consequently, the predicted enrollments for 2026, 2027, and 2028 are not as reliable as the others. With this caveat in mind, let’s see what we get when we combine the model’s predictions with the known enrollments with which we started this section:

```
>>> sns.lineplot( data=trend, x="EnrollmentYear", y="Enrollment")
>>> sns.scatterplot( data=trend, x="EnrollmentYear", y="Enrollmen
>>> sns.lineplot( data=predictions, x="EnrollmentYear", y="Enroll
>>> sns.scatterplot( data=predictions, x="EnrollmentYear", y="Enr
>>> plt.axhline( 19, linestyle=":", color="black")
>>> plt.title("Observed and predicted enrollments, 1985 to 2038")
>>> plt.xlabel("Year")
>>> plt.ylabel("Enrollment (millions)")
```

Figure 2.23. Observed and predicted enrollments

![](img/f25e2c651d2b8852d1c723f946646ff0_75_0.png)

The dotted line is a reminder that the 3 predictions above it are extrapolations from the model. Even so, it seems like colleges and universities are justifiably concerned about the future— enrollments might start climbing again for a few years, but our best guess, based on this data and this simple model, is that enrollments will nosedive in the late 2020s. Ouch!

The simplicity of this model is a great advantage, making its predictions easy to interpret, but enrollments clearly depend on variables other than just births. We’ll explore this later as a multiple regression problem.

[7] http://www.fishbase.org/

[8] https://ropensci.org

[9] https://www.iucnredlist.org

[10] Digest of Education Statistics:
https://nces.ed.gov/programs/digest/current_tables.asp

[11] https://www.cdc.gov/nchs/data-visualization/natality-trends/

## 2.4 Exercises

+   1. Apply smf.ols to the heights dataset to verify the simple regression model reported earlier. Compute the correlation, the coefficient of determination, and the residual standard error.
+   2. Explore some public data repositories and practice building simple regression models with their data. Some good sources are the UCI Machine Learning repository, the Bureau of Labor Statistics, NHANES, and Kaggle, but there are many other sites with interesting data! Once you find some data, do the following:
+      1. If there’s enough data, organize it into groups and compute averages to get the line of averages directly.
+      2. Compare the model diagnostics (correlation, coefficient of determination, residual standard error) with scatterplots of the data and residuals.
+      3. How many observed responses are within 1 RSE of the regression line? 2 RSEs? 3 RSEs?

## 2.5 Summary

+   - Use a scatterplot to inspect your data!
+   - If a linear relationship is evident in the scatterplot,
+     - quantify its strength with the correlation coefficient, and
+     - fit the linear regression model with smf.ols.

+   - The regression line $\hat{y} = a + b\,x$ minimizes the squared sum of residuals and fits the data better than any other line, in the least squares sense.
+     - The slope, $b$, quantifies how the response depends on the predictor.
+     - The coefficient of determination, $R^2$, measures the reduction in variability gained by including the predictor instead of excluding it.

## 3 The simple linear regression model

### This chapter covers

+   - Understanding the theory behind simple linear regression
+   - Leveraging the theory to assess and interpret fitted regression models
+   - Analyzing the residuals to check assumptions

Now that we’ve fitted regression lines to several different datasets, we have to deal with a few things. The first is glaringly obvious in most of the examples: few of the observed data points are on the regression lines, and some of them seem pretty far away! Of course, this makes sense— while we can make an informed guess about, say, the infant mortality rate corresponding to a given literacy rate, we know that our guess is probably going to be wrong, by an amount that we cannot predict precisely. The second concern is that the equation of the line would almost certainly change if we used different data. For the UNICEF data, for example, the infant mortality rates were collected in 2011 and the literacy rates were collected between 2006 and 2010; if we used older or newer data, the slope and the intercept of the regression line would probably differ from what we found before. Third, the quality of the data matters— since there are always some imperfections in recorded observations, the models built from them can’t be perfect, either.

In short, models built from data are, at best, only approximate descriptions of reality. It’s therefore essential to assess the uncertainties inherent in any modeling approach. To address and clarify these issues, it’s time to dig into the abstract model underlying what we’ve been doing. While this chapter is far more technical than the previous one, it is absolutely necessary to understand the theoretical linear regression model if you want to master this particular tool. Inferences about parameters, estimates, and predictions all depend on applying the theory correctly, and the topics we’ll discuss— standard errors, interval estimates, test statistics, and significance tests— are

