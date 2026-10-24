
## Percent Change in Home Value, August 2019 to 2020

-5% 0% 5% 10%

### Refine

**Select column**

- Code
- Aug2020 Home Value
- Name
- Aug2019 Home Value
- Pct Change 2019-20

**Select palette**

**Type**

- steps
- continuous

**Steps**

5 Rounded values

**Custom range**

-6.66 MIN

10.74 MAX

### Annotate

### Design

### Colors

### Legend

Show map legend

Caption: Your title here

![](img/216890131d46798dfe1cd5f886cc608c_222_0.png)

Which data columns, colors, and intervals make the best map? There’s no easy answer, since there’s more than one way to make a true and meaningful map. But keep two principles in mind. First, make sure that you honestly show the data, rather than hide or disguise it. Second, reflect on what kind of data story you believe is important to tell, since design choices emphasize different interpretations of the data. Review our guidance in “Design Choropleth Colors and Intervals”.

### Tooltips

Show tooltips

Customize tooltips

```
{{ name }}
```

```
$ {{ FORMAT(aug2019_home_value, "0,0") }}
home value
```

You can use HTML and insert data columns. Using our formula syntax you can also perform calculations (see full syntax reference).

- Code
- Aug2020 Home Value
- Name
- Aug2019 Home Value
- Pct Change 2019-20
- 1,000.00
- 0
- 0.0
- 0.00
- 0.000
- 0.0

![](img/216890131d46798dfe1cd5f886cc608c_224_0.png)

**CHART SIZE**

600 × 456

15. Finally, click Proceed or advance to the Publish & Embed screen to share your work with others. Follow the prompts, or the more detailed Datawrapper tutorial, to obtain an embed code to your interactive map, and learn more about your next steps in Chapter 9.

> **TIP**
> Learn more about choropleth map design in this excellent series of posts by the Datawrapper Academy.

Now that you’ve learned how to create a choropleth map using one tool, Datawrapper, let’s compare the process using a different tool, Tableau Public.

## Choropleth Map with Tableau Public

We first introduced you to the free Tableau Public desktop application (for Mac or Windows) when building scattercharts and filtered line charts in Chapter 6. Now let’s use the same tool to create an interactive choropleth map, and compare the process with the Datawrapper tool we described in the prior section. We’re showing you how to create the same type of map with both tools to show you the difference. On one hand, Datawrapper gives you more control over interpolating data and shaping the appearance of color intervals in your choropleth map. On the other hand, some people prefer Tableau Public because they’re already familiar with its interface.

Tableau Public can create many different types of maps for geographical place names or ISO codes it already recognizes, such as nations, states, counties, and airports. But Tableau Public cannot geocode street addresses by itself, so you’ll need to obtain their latitude and longitude with another tool, such as those described in the geocode section of “Geocode Addresses in Google Sheets”. Furthermore, if you want to upload customized map boundaries, learn how to Create Tableau Maps from Spatial Files on the support page.

In this section, we’ll create a choropleth map of healthcare spending per country as a percentage of their gross domestic product (GDP), as shown in Figure 7-18. Remember that choropleth maps work best when we normalize the data to show relative, rather than absolute, numbers (see “Normalize Choropleth Map Data”). Creating a map of total health spending per country wouldn’t be very meaningful, as larger nations tend to have larger economies, so we’ll base our map on the percentage of their economy that is spent on healthcare.

Before we start, you should obtain and install the free Tableau Public desktop application if you don’t have it yet. It is available for Mac or Windows. You’ll need to enter an email address to download the application.

Let’s look at the steps involved to create a choropleth from Figure 7-18 in detail:

1. Open the Healthcare Spending by Nation as Percent of GDP data in Google Sheets, which we downloaded from the World Bank. Examine the data and the notes.

   Good maps often require the cleaning up of messy data, as described in Chapter 4. In our spreadsheet, we removed rows for nations that did not report any data. Tableau Public recognizes many different types of geographic names (such as cities and nations), so we’ll rely on the tool to deal with any spelling issues and properly place all of them on the map.

2. In the Google Sheet, go to File > Download and select CSV format to save the data to your local computer.

3. Launch Tableau Public. When you first open it, you’ll see the Connect menu on the left-hand side that displays file formats you can upload. Choose the “Text file” format and upload the healthcare spending CSV data file you’ve just downloaded in the previous step.

> **NOTE**
> Tableau lets you access data directly from Google Sheets that live in your Drive using Connect > “To a Server” option. So instead of downloading a CSV file in step 2, you could have made a copy of the sheet, and connected to it directly.

4. In the Data Source screen, inspect the dataset, which contains three columns: Country Name, Country Code, and Health Spending As % of GDP. Notice that a small globe appears at the top of the Country Name and Country Code columns, which shows that Tableau Public successfully recognized these as geographic data, rather than string or text data. Sometimes Tableau doesn’t recognize location data automatically, so you need to manually change the data type. To do so, click the data type icon (e.g., globe or a green # for numeric values), and then choose Geographic Role > Country/Region.

![](img/216890131d46798dfe1cd5f886cc608c_228_0.png)

5. In the bottom-left corner, click the orange Sheet 1 button to create a worksheet with your first visualization.

6. In Sheet 1, create your choropleth map using a two-step process. First, drag and drop the Country Name field into the middle of the worksheet (alternatively, to the Detail box of the Marks card) to create the map. The default view is the symbol map, which we need to replace with a polygon map. To add colored polygons, drag and drop the Health Spending As % of GDP field into the Color box of the Marks card to transform it into a choropleth map.

**Data / Analytics**: health-pct-gdp-worldbank

**Pages**

**Columns**: Longitude (generated)

**Rows**: Latitude (generated)

Search

**Tables**

- Country Code
- Country Name
- Measure Names
- Health Spending As % of ...
- health-pct-gdp-worldbank...
- Latitude (generated)
- Longitude (generated)
- Measure Values

**Filters**

**Sheet 1**

**Marks**

- Automatic
- Colour
- Size
- Label
- Detail
- Tooltip
- Colour: SUM(Health Sp...)
- Detail: Country Name

7. Tableau Public may hide the map legend behind the Show Me menu in the upper-right corner, so click the menu to shrink it and display your legend.

8. You can change the color palette by clicking the Color box of the Marks card, and then “Edit colors.” Change the palette to Green, and change it from continuous to steps.

![](img/216890131d46798dfe1cd5f886cc608c_231_0.png)

9. When you hover over countries, you’ll notice a tool tip that tells you the name of the country and gives you the percent value. It’s generally well-formatted as our initial data table had proper column headers. But we can make the tool tip even better. Click the Tooltip box of the Marks card and change the first instance of Country Name to just Country (don’t change the grayed-out text inside < and > because these are variable names). Add a % sign at the end of the second row.

![](img/216890131d46798dfe1cd5f886cc608c_231_1.png)

10. Let’s make our map title more meaningful. Double-click the default Sheet 1 name just above the map to bring up the Edit Title window, and change the name of your chart to 2017 Healthcare Spending by Country as % of GDP.

11. At this point, the data is loaded and should be displayed correctly, so we’re going to create the final layout that includes the map’s title and credits, the legend, and is appropriate for sharing. At the bottom-left of the program, create a New Dashboard. Dashboards in Tableau are layouts that can contain visualizations from multiple sheets, as well as text boxes, images, and other elements, creating rich exploratory interfaces. In this tutorial, we’ll stick to just a single sheet that contains our choropleth map.
