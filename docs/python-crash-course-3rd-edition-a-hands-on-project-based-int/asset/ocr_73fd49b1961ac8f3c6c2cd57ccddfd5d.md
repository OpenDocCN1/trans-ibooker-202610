earthquakes stand out as light-yellow points, in contrast to many darker points. You can also tell which regions of the world have more significant earthquake activity.

![](bbox=[0.125, 0.174, 0.877, 0.548])

Figure 16-9: In 30 days' worth of earthquakes, color and size are used to represent the magnitude of each earthquake.

# # Other Color Scales

You can choose from a number of other color scales. To see the available color scales, enter the following two lines in a Python terminal session:

```
```
>>> import plotly.express as px
>>> px.colors.named_colorscales()
['aggrnyl', 'agsunset', 'blackbody', ..., 'mygbm']
```
```

Feel free to try out these color scales in the earthquake map, or with any dataset where continuously varying colors can help show patterns in the data.