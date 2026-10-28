# Here’s how to do this:

*death_valley_highs_lows.py*

```
--snip--
for row in reader:
    current_date = datetime.strptime(row[2], '%Y-%m-%d')
    try:
        high = int(row[3])
        low = int(row[4])
    except ValueError:
        print(f"Missing data for {current_date}")
    else:
        dates.append(current_date)
        highs.append(high)
        lows.append(low)

# Plot the high and low temperatures.
--snip--

# Format plot.
title = "Daily High and Low Temperatures, 2021\nDeath Valley, CA"
ax.set_title(title, fontsize=20)
ax.set_xlabel('', fontsize=16)
--snip--
```

Each time we examine a row, we try to extract the date and the high and low temperature ①. If any data is missing, Python will raise a ValueError and we handle it by printing an error message that includes the date of the missing data ②. After printing the error, the loop will continue processing the next row. If all data for a date is retrieved without error, the else block will run and the data will be appended to the appropriate lists ③. Because we’re plotting information for a new location, we update the title to include the location on the plot, and we use a smaller font size to accommodate the longer title ④.

When you run *death_valley_highs_lows.py* now, you’ll see that only one date had missing data: