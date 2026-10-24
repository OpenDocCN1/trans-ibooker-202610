Figure 12-8. Explore the interactive Leaflet Heatmap.

You can adapt the code we used for this London heatmap to create your own:

+   1. Visit the GitHub repository with our code, make sure you’re logged in, and click the “Use this template” to make a personal copy of this repo.
+   2. Modify the map’s title and description inside index.html.
+   3. Place your point coordinates data inside data.csv. Do not insert any column headers. Instead of the traditional order, you must write them in latitude,longitude (or y,x) order, one pair per line, with no spaces, like this:
+   4. Depending on your data density, you might want to tweak radius and blur parameters inside the <script> tag of index.html:
+   5. Edit the following chunk of code to set your map’s default position and zoom level:

```
51.506585,-0.139387
51.505467,-0.14655
51.507758,-0.141284
```

```
var heat = L.heatLayer(data, {
  radius: 25,
  blur: 15,
})
```

```
var map = L.map('map', {
  center: [51.5, -0.1], // Initial map center
  zoom: 10, // Initial zoom level
})
```

If for some reason you cannot see clusters, make sure your point data is represented in latitude,longitude order, not the other way around. If you have few points, try increasing the value of radius property of L.heatLayer.

# Leaflet Searchable Point Map

A searchable point map works best for showing multiple locations, where users can search by name or proximity to a location, or filter by category, with an optional list view. Figure 12-9 shows a powerful Leaflet template of a searchable and filterable point map, which draws from a CSV data file developed by Derek Eder from DataMade in Chicago. This map allows you to show points of interest, filter them by using “Search by name” functionality, and show them as a list instead of points on a map. In addition, the About page gives you plenty of space to describe the purpose and content of your map.