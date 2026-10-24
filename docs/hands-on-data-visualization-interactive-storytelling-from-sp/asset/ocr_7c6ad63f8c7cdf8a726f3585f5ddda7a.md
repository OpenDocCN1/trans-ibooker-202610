Edit Colours...

Format Legends...

Show Title

Edit Title...

Layout

Floating

Fix Height

Edit Height...

Select Container: Vertical

Deselect

Remove from Dashboard

Rename Dashboard Item...

14. Finally, let’s add a text block with data source underneath the map. From the Objects menu in the left-hand side, drag and drop Text to the lower half of the map. In the Edit Text window that appears, type Data by the World Bank, 2017, and click OK. Initially, the text area will occupy half the height of the screen, so resize it like you would resize any window on your computer.

And we’re done! Make sure you position your map’s center and zoom level because you want it to be visible by others. In this case, the best would be to have a world view because we’re showing data for most countries, although you may want to zoom in to a specific continent. Once you are ready to publish and share the map, go to File > “Save to Tableau Public.” In the pop-up window, log in to your account if requested. Give the map a title, such as Healthcare Spending, and click Save. See how to embed the map as an iframe in “Get the Embed Code or iframe Tag”.

# WARNING

Tableau may not be the best tool to create choropleth maps where you want to have full control of color breaks. By default, Tableau uses a linear color scheme that, as you learned earlier in the chapter, is prone to highlighting outliers, and there’s no straightforward way to change the intervals to nonlinear methods, such as quantiles. If you’re not happy with the way the linear scale represents your data, you can filter your data to remove outliers from the map, see Andy Kriebel’s VizWiz tutorial to use table calculations to group items into quantiles, or revisit “Choropleth Map with Datawrapper”, which gives you more control over color intervals and interpolation.

In all of the prior tutorials, you created interactive maps using static data, meaning it came from a spreadsheet. In the next tutorial, you’ll learn how to build a map using continuously updated data from a Socrata open data repository, which will always display the most current information.

# Current Map with Socrata Open Data

A map built on the Socrata platform shows current data because it continuously pulls the most up-to-date information from an open data repository, which you learned about in “Open Data Repositories”. The advantage of creating visualizations directly on an open data platform is that your chart or map is directly linked to the source. Some government agencies frequently