Figure 7-7. Sequential versus diverging color palettes to illustrate per capita income in US dollars in the contiguous states, from American Community Survey, 2018. Note that a diverging palette does not work well in black and white because it becomes difficult to distinguish shades moving away from the middle.

After you select data classes and a color palette, ColorBrewer displays alphanumeric codes that web browsers translate into colors. You can select hexadecimal codes (#ffffff is white), RGB codes (255,255,255 is white), or CMYK codes (0,0,0,0 is white), and export them in different formats, as shown in Figure 7-8, if your preferred map tool allows you to import color palettes.

![](bbox=[0.17, 0.115, 0.9, 0.77])

Figure 7-8. Click open the Export tab to display your color palette codes in various formats.

# Choose Color Intervals to Group Choropleth Map Data

Another important design choice is color intervals, which determine how you group and display data on your choropleth map. This powerful set of decisions will dramatically shape how your map appears in readers’ eyes, and the message conveyed by your data story. You will need to consider several options in this multistep decision-making process, and although there are few uniform design rules, we’ll offer guidance and recommendations. Since options for selecting intervals vary across different mapping tools, we’ll explain broad concepts in this section, with occasional screenshots from Datawrapper and Tableau Public, but will save the details for those specific tutorials later in the chapter.

Some mapping tools allow you to choose between two different types of color intervals to show movement up or down a data scale, as shown in Figure 7-9. Steps are clearly-marked color dividers, like a staircase, while continuous is a gradual change in color, like a ramp. Both go upward, but take you there in different ways.