with one or two nesting levels at a time. We’ll start by pulling out a dictionary for each earthquake that was recorded in the 24-hour time period.

> **NOTE**
> 
> When we talk about locations, we often say the location’s latitude first, followed by its longitude. This convention probably arose because humans discovered latitude long before we developed the concept of longitude. However, many geospatial frameworks list the longitude first and then the latitude, because this corresponds to the (x, y) convention we use in mathematical representations. The GeoJSON format follows the (longitude, latitude) convention. If you use a different framework, it’s important to learn what convention that framework follows.

# Making a List of All Earthquakes

First, we’ll make a list that contains all the information about every earthquake that occurred.

# eq_explore_data.py

```
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