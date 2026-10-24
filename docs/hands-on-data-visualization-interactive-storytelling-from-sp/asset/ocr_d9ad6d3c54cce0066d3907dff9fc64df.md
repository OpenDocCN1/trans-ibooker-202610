be viewed by anyone with the link (see “Share Your Google Sheets”).

8. To embed an interactive version of your chart in another web page, click the kebab menu in the upper-right corner of your chart, and select Publish chart. In the next screen, select Embed and press the Publish button. See Chapter 9 to learn what to do with the iframe code.

Unfortunately, Google Sheets functionality is very limited when it comes to displaying error bars or uncertainty. You can only assign either constant numbers or percent values as error bar values to an individual series, not to specific data points. If you wish to display error bars in Google Sheets, in “Chart editor,” select Customize tab, scroll down to Series, and select a series from the drop-down menu. Check “Error bars,” and customize its value as either percent or a constant value. This setting will be applied to all data points in that series.

![](bbox=[0.38, 0.17, 0.72, 0.31])

9. Finally, remember that providing your data source adds credibility to your work. You can briefly describe the source in a chart subtitle in Google Sheets. But there is no easy way to insert a clickable link in your chart, so you would need to add more details or links in the separate web page that contains your embedded interactive chart.

# Split Bar and Column Charts

A split column (or bar) chart is best to compare categories in separate clusters. For example, imagine you wish to emphasize calorie counts for selected foods offered at two different restaurants, Starbucks and McDonalds. Format the restaurant data in vertical columns in your Google Sheet, as shown in Figure 6-14.

|   | A | B | C |
|---|---|---|---|
| 1 | Fast Food items | Starbucks | McDonalds |
| 2 | Mocha Frappuccino (24-ounce, 2% milk, whip cream) | 500 |  |
| 3 | White Hot Chocolate (20-ounce, 2% milk, whip cream) | 710 |  |
| 4 | Big Mac |  | 540 |
| 5 | Double Quarter Pounder with cheese |  | 770 |

Figure 6-14. To create a split bar (or column) chart, format each data series vertically, and leave cells blank where appropriate.

Because food items are unique to each restaurant, enter only calorie data in the appropriate column, and leave other cells blank. Now you can easily create a split bar (or column) chart that displays the restaurant data in different clusters, as shown in Figure 6-15. Unlike the grouped column chart previously shown in Figure 6-13, here the bars are separated from each other because we do not wish to draw comparisons between food items that are unique to each restaurant. Also, our chart displays horizontal bars (not columns) because some data labels are long.