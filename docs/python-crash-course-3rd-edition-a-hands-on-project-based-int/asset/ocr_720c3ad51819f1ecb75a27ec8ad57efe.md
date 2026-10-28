We modify the filename to use the new data file *sitka_weather_2021_simple.csv*, and we update the title of our plot to reflect the change in its content. Figure 16-3 shows the resulting plot.

![](bbox=[0.12, 0.22, 0.8, 0.52])

Figure 16-3: *A year’s worth of data*

# Plotting a Second Data Series

We can make our graph even more useful by including the low temperatures. We need to extract the low temperatures from the data file and then add them to our graph, as shown here:

*sitka_highs_lows.py*

```
--snip--
reader = csv.reader(lines)
header_row = next(reader)

# Extract dates, and high and low temperatures.
① dates, highs, lows = [], [], []
for row in reader:
    current_date = datetime.strptime(row[2], '%Y-%m-%d')
```