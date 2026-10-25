You can specify any number of elements, as long as the element is a positive integer. You can specify any data type listed in Table 12.2, although some are more difficult to accommodate. (The **bool** type produces unsatisfying results.) Here’s an example:

Click here to view code image

```
>>> np.linspace(1, 5, num=5, dtype=np.int16)
array([1, 2, 3, 4, 5], dtype=int16)
```

In this case, integers worked out well. However, if you specify a range that would normally require floating-point values and use an integer type, the function has to convert many or all of the values to integer type by truncating them.

# ## 12.5.4 The “empty” Function

The `empty` function generates an uninitialized `numpy` array. If you want to produce an array in which the initial values are not initialized but rather are set later, and if you want to save time by not initializing twice, you may want to use the `empty` function. Be careful, however, because using uninitialized values is a risky practice. It’s reasonable when you’re trying to perform every last trick to increase execution speed—_and_ if you’re sure that the elements will be given meaningful values before being used.

Don’t assume that because the values are uninitialized that they are useful random numbers for the purposes of simulations or games. These numbers have statistical anomalies