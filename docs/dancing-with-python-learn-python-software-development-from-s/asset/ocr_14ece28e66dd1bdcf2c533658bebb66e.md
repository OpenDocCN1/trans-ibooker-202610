# Exercise 13.16

Push out any wedge corresponding to a data value less than or equal to 10.

Finally, you can rotate the chart to display the pie for the best visual effect.

![](bbox=[0.32, 0.24, 0.65, 0.42])

Figure 13.11: The rotated pie chart

In Figure 13.11, I rotated the pie 180° via the startangle=180 argument.

# Exercise 13.17

Explain what the counterclock keyword argument to pie does when you assign it True or False.

# 13.5 Scatter plots

We use a scatter plot to display many points. After doing that, we make it fancy with colors, markers, labels, titles, and fonts and adjust the axes so the plot conveys your data effectively and pleasantly.

Like pie, we control most of the look of a scatter plot via keyword arguments to scatter. The data for the plots is the high temperatures for three cities—A, B, and C—over the first ten days of March 2021. The temperatures are in Fahrenheit, and I use numpy arrays to hold the numeric days of the month and the data.