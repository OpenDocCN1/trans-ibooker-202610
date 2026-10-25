The first argument, _A_, refers to a **numpy** array and is the only required argument.

The _bins_ argument determines the number of subranges, or bins. The default setting is 10, which means that the function determines the difference between the highest and lowest values and divides by 10 to get the size of each subrange. But you can specify settings other than 10.

Click here to view code image

```
plt.hist(A, bins=50) # Place results into 50 bins.
```

Other keyword arguments accepted by this function include **color**, which takes a string containing one of the characters shown in Table 13.2; **align**, which takes one of the values 'left', 'right', or 'mid'; and **cumulative**, which is a Boolean that indicates cumulative results are to be graphed. For more information, use help.

```
>>> help(plt.hist)
```

Another use of histograms is to graph a normal distribution curve. We showed how to do this in Chapter 11, but the approach here gives even better results.

We start by using the **numpy** randomization package to generate 200,000 data points in a normal distribution. This distribution has a mean of 0.0 and a standard deviation of 1.0. But by adding and multiplying, we can convert to an array of values with a mean of 100 and a standard deviation of 10.

Click here to view code image