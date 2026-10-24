

point that have been uploaded into OpenStreetMap, which you can select and download in GeoJSON format. For example, when you search and click on Toronto Centre, the tool displays several neighborhood-level boundaries, the Old Toronto city boundary, the present-day Toronto city boundary, and regional and provincial boundaries, as shown in Figure 13-3. Read more details about each layer to evaluate their accuracy, then select any layer to download in GeoJSON format. The tool also includes an editor (the scissors symbol) to remove water areas from the boundary file (such as deleting Lake Ontario from Toronto). When using any type of data that you downloaded from OpenStreetMap, always credit the source in your final product like this: © OpenStreetMap contributors.¹

![](img/216890131d46798dfe1cd5f886cc608c_389_0.png)

## SPADINA—FORT YORK

tags: boundary administrative more ▼

## OLD TORONTO

tags: boundary administrative more ▼

## TORONTO

tags: boundary administrative more ▼

download

## GOLDEN HORSESHOE

tags: boundary administrative more ▼

## ONTARIO

You know how to find geodata now, so let’s look at free online tools to create, convert, edit, and join GeoJSON files with other types of data.

## Draw and Edit with GeoJson.io

GeoJson.io is a popular open source web tool to convert, edit, and create GeoJSON files. The tool was originally developed by Tom MacWright in 2013 and quickly became a go-to tool for geospatial practitioners.

In this tutorial, we’ll show you how to convert existing KML, GPX, TopoJSON, and even CSV files with latitude/longitude data into GeoJSON files. We’ll also explore how to edit attribute data, add new features to GeoJSON files, and create new geodata from scratch by tracing satellite imagery.

### Convert KML, GPX, and Other Formats into GeoJSON

Navigate to the GeoJson.io tool. You will see a map on the left, and a Table/JSON attribute view area on the right. At the start, it represents an empty feature collection. Remember that features are points, polylines, and polygons.

Drag and drop your geospatial data file into the map area on the left. Alternatively, you can also import a file from Open > File menu. If you don’t have a geospatial file, download the Toronto neighborhoods sample file in KML format to your computer, and upload it to the GeoJson.io tool. This simplified sample KML file was created from the Toronto Open Data portal.

If GeoJson.io can recognize and import your geodata file, you’ll see a green pop-up message in the upper-left corner indicating how many features were imported. For example, Figure 13-4 shows us that 140 features were imported from the sample Toronto neighborhoods KML file, and these polygons appear in the top of the map view.

> **NOTE**
> 
> If GeoJson.io cannot import your file, you’ll see a red pop-up saying it “Could not detect file type.” Instead, try to convert your file into GeoJSON format using the Mapshaper tool, as described in “Edit and Join with Mapshaper”.

![](img/216890131d46798dfe1cd5f886cc608c_391_0.png)

To download a converted GeoJSON file to your computer, go to Save > GeoJSON.

> **WARNING**
> 
> The GeoJson.io tool will automatically name your downloaded file as *map.geojson*, so rename it to avoid confusion.

### Create GeoJSON from a CSV File

GeoJson.io can transform a CSV spreadsheet with *latitude* (or *lat*) and *longitude* (or *lon*) columns into a GeoJSON file of point features. Each row in the spreadsheet becomes its own point, and all columns other than *lat* and *lon* become *attributes* (or *properties*) of point features. For this exercise, you can download the Toronto locations sample CSV file to your computer, which contains three rows of data as shown in Figure 13-5.

| name | lat | lon | link |
|---|---|---:|---:|
| CN Tower | 43.6425956 | -79.38712307 | http://www.cntower.ca/ |
| Toronto Pearson International Airport | 43.6777176 | -79.6270137 | http://www.torontopearson.com/ |
| Royal Ontario Museum | 43.667679 | -79.394809 | http://www.rom.on.ca/en |

*Figure 13-5. A CSV spreadsheet with lat/lon columns can be transformed into a GeoJSON with point features.*

- 1. Select New to clear data from the prior exercise in the GeoJson.io tool, then drag-and-drop the Toronto locations CSV file you downloaded into the map area of the tool. A green pop-up will notify you that three features were successfully imported.

> **NOTE**
> 
> If you add new data to existing data in GeoJson.io, it will combine them into one file, which can be useful for certain tasks.

- 2. Click on a marker to see a pop-up with point properties. If you used the Toronto locations sample file, you’ll see *name* and *link* features, in addition to the tool’s default *marker-color*, *marker-size*, and *marker-symbol* fields. Note that you can edit and delete properties in the Map view.

- 3. Click the Table tab to the right of the map to view all of the data at once, rather than individual marker pop-ups. You can edit and delete properties in the Table view, as well as the JSON code view.

- 4. If you edited your map data, go to Save > GeoJSON to download the file to your computer, which will automatically be named *map.geojson*, so rename it to avoid confusion. You can also log into GeoJson.io with your GitHub account and save it directly to your repository.

![](img/216890131d46798dfe1cd5f886cc608c_393_0.png)

### Create New GeoJSON Data with Drawing Tools

GeoJson.io lets you create geospatial files from scratch by using simple drawing tools to place points, polylines, or polygons on the map. These are useful when you have no original file to work with. Let’s create some new data:

- 1. Click New to clear data from the prior exercise in the GeoJson.io tool.
- 2. In the lower-left corner, switch from Mapbox (vector tiles) to Satellite (raster data).
- 3. In the upper-right corner of the map, use the Search tool to find an area of interest. For this exercise, we’ll trace the geography around an athletic field in Toronto.

4. In the toolbar, you have a choice of four drawing tools: a polyline (which is a series of points connected by lines, but not closed like a polygon), a polygon, a rectangle (which is just an instance of a polygon), and a point marker.

![](img/216890131d46798dfe1cd5f886cc608c_394_0.png)

5. Select the “Draw a marker” button, and click anywhere on the map to place it. You’ll see a gray marker that is now part of your map. You can modify its properties or delete it in the interactive pop-up.

6. Select the “Draw a polyline” button and click on multiple locations in the map to see connected lines appearing. Polylines are generally used for roads and paths. To finish and create a feature, click again on the final point.

7. Select the “Draw a polygon” button, which is similar to drawing a polyline, except that you need to complete the feature by making your final point at the same location as your initial point. Polygons are used to define boundaries, including small and large geographical areas.

8. Use the “Edit layers” tool (above Delete) to move a marker to a better position or adjust the shapes of your features.

9. After you have created features and their physical boundaries, add meaningful attribution data. Use the interactive pop-ups or the Table view to give objects names and other properties. When finished, save the GeoJSON file to your computer.

You can also use drawing tools to edit existing GeoJSON files. For example, if you created a GeoJSON from a CSV file, you might decide to move some markers with “Edit layers” instead of modifying their latitude and longitude values. Or you might decide to make polygons more precise by tracing around satellite imagery.

In the next section, we will introduce Mapshaper, another free online tool to convert and modify geospatial files.

## Edit and Join with Mapshaper

Like GeoJson.io, Mapshaper is a free, open source editor that can convert geospatial files, edit attribute data, filter and dissolve features, simplify boundaries to make files smaller, and much more. Mapshaper’s edit and join commands are much more powerful than the GeoJson.io tool. Unlike GeoJson.io, Mapshaper doesn’t have drawing tools, so you won’t be able to create geospatial files from scratch.

Mapshaper is developed and maintained by Matthew Bloch on GitHub. This easy-to-learn web tool has replaced many of our map preparation tasks that previously required expensive and hard-to-learn ArcGIS software, or its free but still-challenging-to-learn cousin QGIS. Even advanced GIS users may discover that Mapshaper can be a quick alternative for some common but time-consuming tasks.

### Import, Convert, and Export Map Boundary Files

You can use Mapshaper to convert between geospatial file formats. Unlike GeoJson.io, Mapshaper also allows you to upload Esri Shapefiles, so you can easily convert them into the web-friendly GeoJSON format. In the following steps, we’ll convert a geospatial file by importing it to Mapshaper, and then exporting it as a different file type:

- 1. Navigate to Mapshaper.org. The start page has two large drag-and-drop zones that you can use to import your file. The smaller area at the bottom, “Quick import,” uses default import settings and is a good way to begin.
- 2. Drag and drop your geospatial file to the “Quick import” area. For this exercise, you can download our US states Shapefiles in .zip format, which is a compressed archive that contains four Shapefiles.

> **NOTE**
> 
> If you want to import a folder of Shapefiles, you need to either select all files inside that folder and drop them all together to the import area, or upload all of them inside a compressed .zip archive.

- 3. Each imported file becomes a layer and is accessible from the drop-down menu in the top-middle of the browser window. There, you can see how many features each layer has, toggle their visibility, or delete them.
- 4. To export, go to Export in the upper-right corner, and select a desired file format. The choice of export formats is shown here. Currently, available formats are Shapefile, GeoJSON, TopoJSON (similar to GeoJSON, but with topological data), JSON records, CSV, or SVG (Scalable Vector Graphics, for web and print). If you export more than one layer at a time, Mapshaper will archive them first, and you will download an output.zip that contains all exported layers.

