
Minutes, you should receive another email with a link to download your data.

The data you download should be structured just like the data we worked with in this section. It might have different headers than those you saw in this section, but if you follow the same steps we used here, you should be able to generate visualizations of the data you're interested in.

## TRY IT YOURSELF

- **16-1. Sitka Rainfall:** Sitka is located in a temperate rainforest, so it gets a fair amount of rainfall. In the data file *sitka_weather_2021_full.csv* is a header called PRCP, which represents daily rainfall amounts. Make a visualization focusing on the data in this column. You can repeat the exercise for Death Valley if you're curious how little rainfall occurs in a desert.

- **16-2. Sitka-Death Valley Comparison:** The temperature scales on the Sitka and Death Valley graphs reflect the different data ranges. To accurately compare the temperature range in Sitka to that of Death Valley, you need identical scales on the y-axis. Change the settings for the y-axis on one or both of the charts in Figures 16-5 and 16-6. Then make a direct comparison between temperature ranges in Sitka and Death Valley (or any two places you want to compare).

- **16-3. San Francisco:** Are temperatures in San Francisco more like temperatures in Sitka or temperatures in Death Valley? Download some data for San Francisco, and generate a high-low temperature plot for San Francisco to make a comparison.

- **16-4. Automatic Indexes:** In this section, we hardcoded the indexes corresponding to the TMIN and TMAX columns. Use the header row to determine the indexes for these values, so your program can work for Sitka or Death Valley. Use the station name to automatically generate an appropriate title for your graph as well.

- **16-5. Explore:** Generate a few more visualizations that examine any other weather aspect you're interested in for any locations you're curious about.

## Mapping Global Datasets: GeoJSON Format

In this section, you’ll download a dataset representing all the earthquakes that have occurred in the world during the previous month. Then you’ll make a map showing the location of these earthquakes and how significant each one was. Because the data is stored in the GeoJSON format, we’ll work with it using the `json` module. Using Plotly’s `scatter_geo()` plot, you’ll create visualizations that clearly show the global distribution of earthquakes.

## Downloading Earthquake Data

Make a folder called *eq_data* inside the folder where you’re saving this chapter’s programs. Copy the file *eq_1_day_m1.geojson* into this new folder. Earthquakes are categorized by their magnitude on the Richter scale. This file includes data for all earthquakes with a magnitude M1 or greater that took place in the last 24 hours (at the time of this writing). This data comes from one of the United States Geological Survey’s earthquake data feeds, at https://earthquake.usgs.gov/earthquakes/feed.

## Examining GeoJSON Data

When you open *eq_1_day_m1.geojson*, you’ll see that it’s very dense and hard to read:

```json
{"type":"FeatureCollection", "metadata":{"generated":1649052296000,...
{"type":"Feature","properties":{"mag":1.6,"place":"63 km SE of Ped...
{"type":"Feature","properties":{"mag":2.2,"place":"27 km SSE of Ca...
{"type":"Feature","properties":{"mag":3.7,"place":"102 km SSE of S...
{"type":"Feature","properties":{"mag":2.92000008,"place":"49 km SE...
{"type":"Feature","properties":{"mag":1.4,"place":"44 km NE of Sus...
--snip--
```

This file is formatted more for machines than humans. But we can see that the file contains some dictionaries, as well as information that we’re interested in, such as earthquake magnitudes and locations.

The `json` module provides a variety of tools for exploring and working with JSON data. Some of these tools will help us reformat the file so we can look at the raw data more easily before we work with it programmatically.

Let’s start by loading the data and displaying it in a format that’s easier to read. This is a long data file, so instead of printing it, we’ll rewrite the data to a new file. Then we can open that file and scroll back and forth through the data more easily:

### eq_explore_data.py

```python
from pathlib import Path
import json

# Read data as a string and convert to a Python object.
path = Path('eq_data/eq_data_1_day_m1.geojson')
contents = path.read_text()
all_eq_data = json.loads(contents)

# Create a more readable version of the data file.
path = Path('eq_data/readable_eq_data.geojson')
readable_contents = json.dumps(all_eq_data, indent=4)
path.write_text(readable_contents)
```

We read the data file as a string, and use `json.loads()` to convert the string representation of the file to a Python object **❶**. This is the same approach we used in Chapter 10. In this case, the entire dataset is converted to a single dictionary, which we assign to `all_eq_data`. We then define a new `path` where we can write this same data in a more readable format **❷**. The `json.dumps()` function that you saw in Chapter 10 can take an optional `indent` argument **❸**, which

Let’s look at a dictionary representing a single earthquake:

***readable_eq_data.json***

```json
--snip--
{
    "type": "Feature",
    "properties": {
        "mag": 1.6,
        --snip--
        "title": "M 1.6 - 27 km NNW of Susitna, Alaska"
    },
    "geometry": {
        "type": "Point",
        "coordinates": [
            -150.7585,
            61.7591,
            56.3
        ]
    },
    "id": "ak0224bju1jx"
},
```

The key `"properties"` contains a lot of information about each earthquake ❶. We’re mainly interested in the magnitude of each earthquake, associated with the key `"mag"`. We’re also interested in the `"title"` of each event, which provides a nice summary of its magnitude and location ❷.

The key `"geometry"` helps us understand where the earthquake occurred ❸. We’ll need this information to map each event. We can find the longitude ❹ and the latitude ❺ for each earthquake in a list associated with the key `"coordinates"`.

This file contains way more nesting than we’d use in the code we write, so if it looks confusing, don’t worry: Python will handle most of the complexity. We’ll only be working with one or two nesting levels at a time. We’ll start by pulling out a dictionary for each earthquake that was recorded in the 24-hour time period.

> **NOTE**
>
> When we talk about locations, we often say the location’s latitude first, followed by its longitude. This convention probably arose because humans discovered latitude long before we developed the concept of longitude. However, many geospatial frameworks list the longitude first and then the latitude, because this corresponds to the (x, y) convention we use in mathematical representations. The GeoJSON format follows the (longitude, latitude) convention. If you use a different framework, it’s important to learn what convention that framework follows.

## Making a List of All Earthquakes

First, we’ll make a list that contains all the information about every earthquake that occurred.

### eq_explore_data.py

```python
from pathlib import Path
import json

# Read data as a string and convert to a Python object.
path = Path('eq_data/eq_data_1_day_m1.geojson')
contents = path.read_text()
all_eq_data = json.loads(contents)

# Examine all earthquakes in the dataset.
all_eq_dicts = all_eq_data['features']
print(len(all_eq_dicts))
```

We take the data associated with the key `'features'` in the `all_eq_data` dictionary, and assign it to `all_eq_dicts`. We know this file contains records of 160 earthquakes, and the output verifies that we’ve captured all the earthquakes in the file:

```
160
```

Notice how short this code is. The neatly formatted file `readable_eq_data.json` has over 6,000 lines. But in just a few lines, we can read through all that data and store it in a Python list. Next, we’ll pull the magnitudes from each earthquake.

## Extracting Magnitudes

We can loop through the list containing data about each earthquake, and extract any information we want. Let’s pull out the magnitude of each earthquake:

```python
eq_explore_data.py
--snip--
all_eq_dicts = all_eq_data['features']

mags = []
for eq_dict in all_eq_dicts:
    mag = eq_dict['properties']['mag']
    mags.append(mag)

print(mags[:10])
```

We make an empty list to store the magnitudes, and then loop through the list `all_eq_dicts` ①. Inside this loop, each earthquake is represented by the dictionary `eq_dict`. Each earthquake’s magnitude is stored in the `'properties'` section of this dictionary, under the key `'mag'` ②. We store each magnitude in the variable `mag` and then append it to the list `mags`.
