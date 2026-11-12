# # CORRELATION IS NOT CAUSATION!

It is important to note that while we put a lot of emphasis on measuring correlation and building metrics around it, please note _correlation is not causation!_ You probably have heard that mantra before but I want to expand on why statisticians say it.

Just because we see a correlation between x and y does not mean x causes y. It could actually be y causes x! Or maybe there is a third uncaptured variable z that is causing x and y. It could be that x and y do not cause each other at all and the correlation is just coincidental, hence why it is important we measure the statistical significance.

Now I have a more pressing question for you. Can computers discern between correlation and causation? The answer is a resounding “NO!” Computers only have concept of correlation but not causation. Let’s say I load a dataset to Scikit-learn showing gallons of water consumed and my water bill. My computer, or any program including Scikit-learn, does not have any notion of whether more water usage causes a higher bill, or a higher bill causes more water usage. An “AI” system could easily conclude the latter, as nonsensical as that is. This is why many machine learning projects require a human in the loop to inject common sense.

In computer vision, this happens too. Computer vision often uses a regression on the numeric pixels to predict a category. If I train a computer vision system to recognize cows using pictures of cows, it can easily make correlations with the field rather than cows. Therefore if I show a picture of an empty field, it will label the grass as cows! This again is because computers have no concept of causality (the cow shape should cause the label “cow”) and instead gets lost in correlations we are not interested in.

# ## Standard Error of the Estimate

One way to measure the overall error of a linear regression is the **SSE**, or **sum of squared error**. We learned about this earlier where we square each residual and sum them. If ŷ (pronounced “y-hat”) is each predicted value from the line and y represents each actual y value from the data, here is the calculation:

```
SSE = \sum (y - \hat{y})^2
```

However, all of these squared values are hard to interpret so we can use some square root logic to scale things back into their original units. We will also average all of them, and this is what the **standard error of the**