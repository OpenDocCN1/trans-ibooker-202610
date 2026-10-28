# TRY IT YOURSELF

**16-6. Refactoring:** The loop that pulls data from `all_eq_dicts` uses variables for the magnitude, longitude, latitude, and title of each earthquake before appending these values to their appropriate lists. This approach was chosen for clarity in how to pull data from a GeoJSON file, but it’s not necessary in your code. Instead of using these temporary variables, pull each value from `eq_dict` and append it to the appropriate list in one line. Doing so should shorten the body of this loop to just four lines.

**16-7. Automated Title:** In this section, we used the generic title *Global Earthquakes.* Instead, you can use the title for the dataset in the metadata part of the GeoJSON file. Pull this value and assign it to the variable `title`.

**16-8. Recent Earthquakes:** You can find online data files containing information about the most recent earthquakes over 1-hour, 1-day, 7-day, and 30-day periods. Go to https://earthquake.usgs.gov/earthquakes/feed/v1.0/geojson.php and you’ll see a list of links to datasets for various time periods, focusing on earthquakes of different magnitudes. Download one of these datasets and create a visualization of the most recent earthquake activity.

**16-9. World Fires:** In the resources for this chapter, you’ll find a file called *world_fires_1_day.csv*. This file contains information about fires burning in different locations around the globe, including the latitude, longitude, and brightness of each fire. Using the data-processing work from the first part of this chapter and the mapping work from this section, make a map that shows which parts of the world are affected by fires.

You can download more recent versions of this data at https://earthdata.nasa.gov/earth-observation-data/near-real-time/firms/active-fire-data. You can find links to the data in CSV format in the *SHP, KML, and TXT Files* section.

# Summary

In this chapter, you learned how to work with real-world datasets. You processed CSV and GeoJSON files, and extracted the data you want to focus on. Using historical weather data, you learned more about working with Matplotlib, including how to use the `datetime` module and how to plot multiple data series on one chart. You plotted