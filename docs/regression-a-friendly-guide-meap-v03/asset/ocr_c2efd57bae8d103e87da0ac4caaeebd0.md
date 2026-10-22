| Upper left | Below average | Negative | Above average | Positive | Negative |
| Lower left | Below average | Negative | Below average | Negative | Positive |
| Lower right | Above average | Positive | Below average | Negative | Negative |

We see a negative association in the UNICEF data precisely because most of the points— about 3/4 of them— are in the upper left and lower right quadrants; most of the products of the deviations of their coordinates from their respective averages are therefore *negative*. The signs of these products encode how the coordinates vary together— positive products when they increase together, negative products when they change in opposite ways. The correlation coefficient is an average of these products, normalized as shown below, so the correlation between Literacy and IMR is negative. To determine its precise value, we can use the pandas corr() function:

```
>>> africa_corr = africa.Literacy.corr( africa.IMR )
>>> africa_corr

    -0.616221949960677
```

The correlation between Literacy and IMR is about -0.62, an indicator of a fairly strong linear relationship; for comparison, the correlation between the heights of fathers and sons is about 0.5. Before getting into how to interpret these values, let’s summarize the gross properties of the correlation between two quantitative variables, using the quadrants determined by their averages for reference:

+   - If most of the points in a scatterplot are in the upper right and lower left quadrants, the correlation coefficient will be positive.
+   - If most of the points in a scatterplot are in the upper left and lower right