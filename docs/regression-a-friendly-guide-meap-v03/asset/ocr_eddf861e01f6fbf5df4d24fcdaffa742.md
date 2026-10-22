```
mean      67.687097
std        2.744868
Name: Father, dtype: float64

>>> heights.Son.agg(['mean','std'])

mean      68.684070
std        2.814702
Name: Son, dtype: float64
```

We can now describe these two columns succinctly with some concise notation: the fathers' heights follow N(67.687, 2.745), the normal distribution with average 67.687 inches and standard deviation 2.745 inches, and the sons' heights follow N(68.684, 2.815), the normal distribution with average 68.684 inches and standard deviation 2.815 inches. Look at either of the histograms above and observe that most of the heights are within 2 standard deviations of the average, and almost all of them are within 3 standard deviations of the average. As long as we’re working with a roughly normal distribution, the standard deviation tells us where most of the observations are relative to the average— it measures the average deviation from the average.

This is a phenomenon that delighted the Victorians as they pored over the data they collected— many biological measurements, like these heights, turn out to be roughly normally distributed. We can put this to good use and create a simple statistical model: to predict a man’s height, we can just guess 68.684 inches, the average height of the sons, and we’ll likely be correct to within 2 standard deviations, 5.6 inches or so. This is an example of a null model, also known as a baseline model, and the question is: can we come up with something better? Of course we can, that’s why we’re here!

This null model ignores the data on fathers' heights, so we should be able to create a better predictive model by incorporating that extra information. After all, tall men tend to have tall sons and short men tend to have short sons, right? We can check this easily enough by plotting the sons' heights against the fathers' heights.

# Listing 2.5. Create a scatterplot of the heights

```
>>> sns.scatterplot(data=heights, x="Father", y="Son")
```