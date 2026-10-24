Figure 18-19 shows the output.

![](bbox=[0.25, 0.14, 0.7, 0.48])

# Figure 18-19. Bar chart

You can convert the index to a column as follows:

```
df3.reset_index(level=0, inplace=True)
df3
```

The output is as follows:

|   | country | sumCases |
|---|---------|----------|
| 0 | Italy | 846756.0 |
| 1 | Iraq | 590049.0 |
| 2 | Bulgaria | 453353.0 |
| 3 | China | 370357.0 |
| 4 | Taiwan (Province of China) | 296268.0 |
| 5 | Egypt | 284449.0 |
| 6 | Iran (Islamic Republic of) | 225798.0 |
| 7 | Nigeria | 203688.0 |
| 8 | Germany | 133425.0 |
| 9 | Republic of Korea | 117018.0 |