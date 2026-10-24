Figure 7-13. Use the full color range to show all of the differences in the data. Image by Datawrapper Academy, reprinted with permission.

Designing true and meaningful choropleth maps is challenging work. You’ll improve your skills the same way we did, by reading widely, looking at different maps, and testing various ways to visualize your data. Become more aware of how your decisions about color intervals can dramatically alter how the data appears to readers. Most importantly, create maps that focus on telling your story and truthfully representing the data.

# Normalize Choropleth Map Data

We introduced the concept of normalizing data in “Normalize Your Data”. Normalization means adjusting data that was collected using different scales into a common scale to make more appropriate comparisons. For example, it makes little sense to compare the total number of COVID cases between nations with very different populations, such as 9.61 million cases in the US (estimated population of 328.2 million) and 0.49 million cases in Belgium (estimated population of 11.5 million) as of November 6, 2020. A better strategy is to normalize the data by comparing cases per capita (such as 2,928 cases per 100,000 in the US versus 4,260 per 100,000 in Belgium) to adjust for the differences in population.

If you forget to normalize data for a choropleth map and display raw counts instead of relative values (such as percentages or rates per capita), you’ll often end up re-creating a meaningless map of population centers, rather than the phenomenon you’re trying to measure. For example, compare the two maps shown in Figure 7-14. They both are about COVID-19 cases in the continental US as of June 26, 2020. On the left, (a) shows the total number of recorded cases per state, then (b) shows COVID-19 cases adjusted by the state’s population. Darker colors represent higher values. Do you notice any differences in spatial patterns?

![](bbox=[0.169, 0.344, 0.895, 0.681])

(a) COVID-19 total cases

(b) COVID-19 cases per capita

Figure 7-14. Choropleth maps work best with normalized values.

Both maps show COVID-19 data collected by *The New York Times* and published on GitHub. In the map in (b), we normalized values by dividing the total number of cases by the population in each state, according to the 2018 US Census American Community Survey, the most recent data available on the day of writing. We didn’t add legends or other important cartographic elements so that you can better focus on interpreting spatial patterns. In both cases, we used Jenks natural breaks for classification.

What are the worst-hit states according to the map showing total COVID-19 counts, Figure 7-14 (a)? If you’re familiar with US geography, you can quickly tell that these are New York, New Jersey, Massachusetts, Florida, Illinois, Texas, and California. But five of these happen to be some of the most populous states in the US, so it makes sense that they will also have more COVID-19 cases.

Now, how about the map in Figure 7-14 (b)? You can see that New York and its neighbors, including New Jersey and Massachusetts, have by far the highest rates per capita (per person), which we saw in the first map. But you can also see that California, Texas, and Florida were impacted to a lesser extent than the map on the left suggests. So the map with per capita values is a much better illustration to the story about New York being the *first* epicenter of the COVID-19 crisis in the US.