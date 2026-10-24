# Leaflet Maps with Open Data APIs

Learn how to code your own Leaflet map with an API that continuously pulls the most current information directly from an open data repository, similar to the Socrata Open Data map you learned about in “Current Map with Socrata Open Data”. Leaflet maps can pull and display data from various open data repositories using APIs. Figure 12-10 shows an interactive map of North Dakota counties, colored by population density, with hospitals and emergency medical service (EMS) locations.

This map template pulls data from three different open repository sources:

+   - Hospital information is pulled directly from the Medicare.org Socrata database.
+   - County boundaries and population density are pulled from the North Dakota GIS ArcGIS server.
+   - EMS stations are fetched from the Homeland Infrastructure Foundation-Level Data ArcGIS server.