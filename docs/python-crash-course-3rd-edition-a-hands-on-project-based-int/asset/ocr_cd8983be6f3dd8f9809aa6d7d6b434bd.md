![](bbox=[0.16, 0.12, 0.79, 0.42])

Figure 16-1: A line graph showing daily high temperatures for July 2021 in Sitka, Alaska

# The datetime Module

Let’s add dates to our graph to make it more useful. The first date from the weather data file is in the second row of the file:

```
"USW00025333","SITKA AIRPORT, AK US","2021-07-01",,"61","53"
```

The data will be read in as a string, so we need a way to convert the string "2021-07-01" to an object representing this date. We can construct an object representing July 1, 2021, using the strptime() method from the datetime module. Let’s see how strptime() works in a terminal session:

```
>>> from datetime import datetime
>>> first_date = datetime.strptime('2021-07-01', '%Y-%m-%d')
>>> print(first_date)
2021-07-01 00:00:00
```