To display polygons:

+   1. In the Polygons tab, make sure *Polygon Legend Position* (cell B4) is visible by selecting “topleft” or a similar position.
+   2. In *Polygon GeoJSON URL* (cell B6), enter the pathname to the file you uploaded to your GitHub repo, such as *geodata/polygons.geojson*.
+   3. You can change the *Polygon Legend Title* (cell B3) and add an optional *Polygon Legend Icon* (cell B5).
+   4. Edit the Polygon Data and Color Settings sections to modify the labels and ranges to align with the properties of your GeoJSON file. In the *Property Range Color Palette*, you can automatically select a color scheme from the ColorBrewer tool we described in “Map Design Principles”, or manually insert colors of your choice in the cell below.
+   5. Read the *Hints* column in the *Polygons* sheet for tips on how to enter data.
+   6. If you wish to display multiple polygon layers, use the *Polygons* tab drop-down menu to Duplicate the sheet, and name additional sheets in this format: *Polygons1*, *Polygons2*, etc.

Now you’re ready to finalize your map. If you wish to share your map link with the public, read the options here and choose either step G or step H.

> **WARNING**
> 
> We reserve the right to change *our* Google Sheets API key at any time, especially if other people overuse or abuse it. This means that you *must* finalize your map using either step G or H before sharing it publicly, because it will *stop working* if we change our key.

# G. Save each Google Sheets tab as a CSV file and upload to GitHub

If you’ve finished entering most of your data into your Google Sheets, downloading them into separate CSV files and uploading those into your GitHub repo is the *best* long-term preservation strategy. This approach keeps your map and data together in the same GitHub repo and removes the risk that your map will break due to an interruption to Google services. Plus, you can still edit your map data. If this approach makes sense, follow these steps:

+   1. In your Google Sheets, go to each tab and select File > Download into CSV format, to create a separate file for each tab.