# Example 3-11. Getting a middle range probability using the CDF

```
from scipy.stats import norm
mean = 64.43
std_dev = 2.99
x = norm.cdf(66, mean, std_dev) - norm.cdf(62, mean, std_dev)
print(x) # prints 0.4920450147062894
```

You should find the probability of observing a golden retriever between 62 and 66 pounds to be 0.4920, or approximately 49.2%

# The Inverse Cumulative Density Function (CDF)

When we start doing hypothesis testing later in this chapter, you will encounter situations where you need to look up an area on the CDF and then return the corresponding x-value. Of course this is a backwards usage of the CDF, so we will need to use the inverse CDF which flips the axes as shown in Figure 3-11.