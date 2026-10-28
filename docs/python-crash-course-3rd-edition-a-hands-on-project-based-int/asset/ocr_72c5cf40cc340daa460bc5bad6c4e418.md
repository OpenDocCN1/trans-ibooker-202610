```
```python
    le,
        color=mags,
        color_continuous_scale='Viridis',
        labels={'color':'Magnitude'},
        projection='natural earth',
    )
fig.show()
```
```

All the significant changes here occur in the `px.scatter_geo()` function call. The `color` argument tells Plotly what values it should use to determine where each marker falls on the color scale ❶. We use the `mags` list to determine the color for each point, just as we did with the `size` argument.

The `color_continuous_scale` argument tells Plotly which color scale to use ❷. *Viridis* is a color scale that ranges from dark blue to bright yellow, and it works well for this dataset. By default, the color scale on the right of the map is labeled *color*; this is not representative of what the colors actually mean. The `labels` argument, shown in Chapter 15, takes a dictionary as a value ❸. We only need to set one custom label on this chart, making sure the color scale is labeled *Magnitude* instead of *color*.

We add one more argument, to modify the base map over which the earthquakes are plotted. The `projection` argument accepts a number of common map projections ❹. Here we use the `'natural earth'` projection, which rounds the ends of the map. Also, note the trailing comma after this last argument. When a function call has a long list of arguments spanning multiple lines like this, it’s common practice to add a trailing comma so you’re always ready to add another argument on the next line.

When you run the program now, you’ll see a much nicer-looking map. In [Figure 16-9](#), the color scale shows the severity of individual earthquakes; the most severe