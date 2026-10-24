Figure 12-7. Explore the interactive Leaflet point map with CSV data.

You can adapt this template to create your own point map by following these instructions:

+   1. Visit the GitHub repo that stores the code for this template. Make sure you’re logged in, and press “Use this template button” to create a copy of this repository in your own GitHub account.
+   2. Put your point data inside data.csv. The only relevant columns that will be read by the template are Latitude, Longitude, and Title. The first two determine the location of the marker, and the last one is displayed in a pop-up. The order of columns doesn’t matter. There can be other columns in the dataset, but they will be ignored.
+   3. Depending on the geography of your points, you’ll want to change the default position of the map when it’s first loaded. In index.html, find the <script> tag, and edit the following chunk of code:

Your data can look like the following:

```
Title,Latitude,Longitude
Trinity College,41.745167,-72.69263
Wesleyan University,41.55709,-72.65691
```

```
var map = L.map('map', {
  center: [41.57, -72.69], // Default latitude and longitude on start
  zoom: 9, // Between 1 & 18; decrease to zoom out, increase to zoom in
  scrollWheelZoom: false
});
```

We used default Leaflet markers for code simplicity, but you may want to use custom icons instead. The following code snippet can give you an idea of how to set it up in your GitHub repository, where you insert your unique pathname to your icon in place of the sample.

```
var marker = L.marker([row.Latitude, row.Longitude], {
  opacity: 1,
  // Customize your icon
  icon: L.icon({
    iconUrl: 'path/to/your/icon.png',
    iconSize: [40, 60]
  })
}).bindPopup(row.Title);
```

To learn more, see this helpful Leaflet documentation example about custom icons.

# Leaflet Heatmap Points with CSV Data

Heatmaps turn individual points into hotspots or clusters, allowing viewers to explore spatial distributions of events, such as areas of high and low population density or incidents of crime. Figure 12-8 shows an interactive heatmap of bike theft locations in London between January and July 2020. The underlying data are coordinate locations for each reported bike theft, which the Leaflet.heat plug-in transforms into areas of various densities. Red shows areas of highest density, or areas where bike theft happened most often. When you zoom in, areas are recalculated into more distinct clusters.