years) rather than the default, which is every 50 months. Here
the inputs to **arange** are 0, 361, and 60 as beginning, ending,
and step figures.

Click here to view code image

```
plt.xticks(np.arange(0, 361, 60),
 ('2020', '2025', '2030', '2035', '2040',
 '2045', '2050') )
```

**Note**

The `np.arange` function generates values _up to but not including_ the endpoint.
That's why these examples use the endpoints 1.1 and 361 instead of 1.0 and 360.

All that you need to do now is add these two statements
(`plt.yticks` and `plt.xticks`) to the program code in the
previous section. Let’s also change the X-axis label to “Years.”

```
plt.xlabel('Years')
```

Now Figure 13.22 produces a nice-looking result.