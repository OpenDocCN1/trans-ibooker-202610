# 3.3 Examples

# 3.3.1 Analyzing the compressive strength of concrete

Let’s apply what we’ve just learned to the regression model we built earlier to understand the compressive strength of concrete. First, we should look at the residuals to see if there are any obvious conflicts with the assumptions:

```
>>> concrete_rse = np.sqrt( concrete_fit.scale )
>>> concrete_rse

    10.142165416378145
```

```
>>> sns.residplot(data=concrete, x="Ratio", y="Strength", line_kw
>>> plt.axhline( 2*concrete_rse, color="black", linestyle=":")
>>> plt.axhline( -2*concrete_rse, color="black", linestyle=":")
>>> plt.title("Residuals: Strength vs cement:water ratio")
```

Figure 3.4. Plot the residuals to check assumptions about the error