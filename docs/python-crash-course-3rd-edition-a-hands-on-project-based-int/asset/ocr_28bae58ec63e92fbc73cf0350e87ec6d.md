reading for the day, this station includes TOBS, a reading for a specific observation time.

Change *sitka_highs_lows.py* to generate a graph for Death Valley using the indexes we just noted, and see what happens:

# *death_valley_highs_lows.py*

```
--snip--
path = Path('weather_data/death_valley_2021_simple.csv')
lines = path.read_text().splitlines()
    --snip--
# Extract dates, and high and low temperatures.
dates, highs, lows = [], [], []
for row in reader:
    current_date = datetime.strptime(row[2], '%Y-%m-%d')
    high = int(row[3])
    low = int(row[4])
    dates.append(current_date)
--snip--
```

We update the program to read from the Death Valley data file, and we change the indexes to correspond to this file’s TMAX and TMIN positions.

When we run the program, we get an error:

```
Traceback (most recent call last):
    File "death_valley_highs_lows.py", line 17, in <module>
        high = int(row[3])
① ValueError: invalid literal for int() with base 10: ''
```

The traceback tells us that Python can’t process the high temperature for one of the dates because it can’t turn an empty string ('') into an integer ①. Rather than looking through the data to find out which reading is missing, we’ll just handle cases of missing data directly.

We’ll run error-checking code when the values are being read from the CSV file to handle exceptions that might arise.