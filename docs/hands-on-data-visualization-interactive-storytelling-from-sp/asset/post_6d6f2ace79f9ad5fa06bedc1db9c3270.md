
Next we’ll examine data about climate change, one of the most pressing issues we face on our planet, yet deniers continue to resist reality, and some of them twist the facts. In this tutorial, we’ll examine global temperature data from 1880 to the present, downloaded from US National Aeronautics and Space Administration (NASA). It shows that the mean global temperature has risen about 1° Celsius (or about 2° Fahrenheit) during the past 50 years, and this warming has already begun to cause glacial melt and rising sea levels. Your task is to create *misleading* line charts that *diminish* the appearance of rising global temperature change in the reader’s eye.

- 1. Open the global temperature change 1880–2019 data in Google Sheets, and go to File > “Make a copy” to create a version you can edit in your own Google Drive.

- 2. Examine the data and read the notes. Temperature change refers to the mean global land–ocean surface temperature in degrees Celsius, estimated from many samples around the earth, relative to the temperature in 1951–1980, about 14°C (or 57°F). In other words, the 0.98 value for 2019 means that global temperatures were about 1°C above normal that year. Scientists define the 1951–1980 period as “normal” based on standards from NASA and the US National Weather Service; adults who grew up during that time can remember this “normal.” While there are other ways to measure temperature change, this data from NASA’s Goddard Institute for Space Studies (NASA/GISS) is generally consistent with data compiled by other scientists at the Climatic Research Unit and the National Oceanic and Atmospheric Administration.

- 3. Create a Google Sheets line chart by selecting the two columns in the data sheet, then Insert > Chart. The tool should recognize your time-series data and produce a *default* line chart, though we never blindly accept it as the best representation of the truth. Click on the three-dot kebab menu to open the “Chart editor” and select the Customize tab. Add a better title and vertical axis label using the notes to clarify the source and how temperature change is measured.

![](img/216890131d46798dfe1cd5f886cc608c_426_0.png)

Now let’s create three more charts using the same data but different methods, and discuss why they are *not wrong* from a technical perspective, but nevertheless *very misleading*.

## Lengthen the vertical axis to flatten the line

We’ll use the same method as shown in previous section, but change the axes in the opposite direction. In the Google Sheets Chart editor, customize the vertical axis by changing the minimum value to −5 and the maximum to 5, as shown in Figure 14-1. By increasing the length of the vertical scale, you flattened our perception of the rising line, and canceled our climate emergency—but not really.

What makes this flattened line chart *misleading* rather than *wrong*? In the first half of the tutorial, when you reduced the vertical axis of the US GDP chart, you violated the zero-baseline rule, because column and bar charts *must* begin at zero because they require readers to judge *height* and *length*, as described in “Chart Design Principles”. You may be surprised to learn that the zero-baseline rule does *not* apply to line charts. Visualization expert Alberto Cairo reminds us that line charts represent values in the *position* and *angle* of the line. Readers interpret the meaning of line charts by their shape, rather than their height, so the baseline is irrelevant. Therefore, flattening the line chart for temperature change may mislead readers, but it’s technically not wrong, as long as it’s labeled correctly.

![](img/216890131d46798dfe1cd5f886cc608c_427_0.png)

*Figure 14-1. Misleading chart with a lengthened vertical axis.*

## Widen the chart to warp its aspect ratio

In your Google Sheet, click the chart and drag the sides to make it very short and wide, as shown in Figure 14-2. Image measurements are listed in width by height, and we calculate the aspect ratio as width divided by height. Since the default chart is 600 × 370 pixels, its aspect ratio is about 1.6 to 1. But the stretched-out chart is 1090 × 191 pixels, and its ratio is about 5.7 to 1. By increasing the aspect ratio, you have flattened our perception of the rising line, and canceled our climate crisis once again—but not really.

What makes this warped line chart *misleading* rather than *wrong*? Once again, since changing the aspect ratio of a line chart doesn’t violate a clearly defined rule of data visualization, it’s not technically wrong, as long as it’s accurately labeled. But it’s definitely misleading. Cairo states that we should design charts with an aspect ratio that “neither exaggerates nor minimizes change.” What specifically does he suggest? Cairo recommends—yet clearly states this “isn’t a universal rule of chart design”—that the percent change expressed in a chart should roughly match its aspect ratio. For example, if a chart represents a 33% increase, which is the same as 33/100 or one-third, he recommends an aspect ratio of 3:1 (because the fraction is flipped by placing width before height), or in other words, a line chart that is three times wider than its height.⁵

![](img/216890131d46798dfe1cd5f886cc608c_428_0.png)

*Figure 14-2. Misleading chart with a stretched aspect ratio.*

### NOTE

Some experts advise that aspect ratios for line charts should follow the banking to 45 degrees principle, which states that the average orientation of line segments should be equal to an angle of 45°, upward or downward, to distinguish individual segments. But this requires statistical software to calculate slopes for all of the lines, and still isn’t a rule that fits all cases. Read “Aspect Ratio and Banking to 45 Degrees,” a good overview by Robert Kosara.⁶

Cairo does not propose his aspect ratio recommendation as a universal rule because he recognizes how it fails with very small or very large values. For example, if we apply Cairo’s recommendation to our global temperature change chart, the difference between the lowest and highest values (−0.5° to 1°C) represents a 300% increase. In this case, we calculate the percent change using the lowest value of −0.5°C, rather than the initial value of 0°C, because dividing by zero is not defined, so (1°C − (−0.5°C)) / |−0.5°C| = 3 = 300%. Following Cairo’s general recommendation, a 300% increase suggests a 1:3 aspect ratio, or a line chart three times taller than its width, as shown in Figure 14-3. While this very tall chart is technically correct, it’s misleading because it exaggerates change, which is contrary to Cairo’s main message. The aspect ratio recommendation becomes ridiculous when we divide by numbers that are very close to zero.

Global Temperature Change, 1880–2019  
Misleading chart  
Change relative to 1951–80 average (Celsius)  
Year

![](img/216890131d46798dfe1cd5f886cc608c_429_0.png)

Cairo acknowledges that his aspect ratio recommendation can result in *misleading* charts in the opposite way: they *diminish change*. For example, instead of *global temperature change*, which increased from 0°C to 1°C, imagine a chart that displays *global temperature*, which increased from about 13°C to 14°C (or about 55°F to 57°F) over time. Even though a 1°C difference in average global temperature may not *feel* very significant to our bodies, it has dramatic consequences for the Earth. We can calculate the percent change as: (14°C − 13°C) / 13°C = 0.08 = 8% increase, or about one-twelfth. This translates into a 12:1 aspect ratio, or a line chart that is 12 times wider than it is tall, as shown in Figure 14-4. Cairo warns that this significant global temperature increase looks “deceptively small,” so he cautions against using his aspect ratio recommendation in all cases.

![](img/216890131d46798dfe1cd5f886cc608c_430_0.png)

*Figure 14-4.* Once again, rules of thumb do not always work. Cairo’s recommendation for an 8% increase results in a 12:1 aspect ratio that produces a misleading chart in this particular example.

Where does all of this leave us? If you feel confused, that’s because data visualization has *no universal rule* about aspect ratios. What should you do? First, never blindly accept the default chart. Second, explore how different aspect ratios affect the chart’s appearance. Finally, even Cairo argues that you should use your own judgment rather than follow his recommendation in every situation, because there’s no single rule about aspect ratio that fits all circumstances. Make a choice that honestly interprets the data and clearly tells a story to your reader.

## Add more data and a dual vertical axis

Another common way to mislead is to add more data, such as a second data series that corresponds to a second vertical axis on the right side of a line chart. While it’s technically possible to construct a dual-axis chart, we strongly advise against them because they can be easily manipulated to mislead readers. Let’s illustrate how with an example that combines two prior datasets—global temperature change and US GDP—in one dual-axis chart. In the Google Sheet, go to the *temp+GDP* sheet, where you will see temperature change plus a new column: US GDP in billions of dollars from 1929 to 2019, downloaded from the *US Federal Reserve*. To simplify this example, we deleted pre-1929 temperature data to match it up more neatly with available GDP data.

- 1. Select all three columns and Insert > Chart to produce a default line chart with two data series: temperature (in blue) and US GDP (in red).

- 2. In the “Chart editor,” select Customize and scroll down to Series. Change the drop-down menu from “Apply to all series” to US GDP. Just below that, in the Format area, change the Axis menu from “Left axis” to “Right axis,” which creates another vertical axis on the right side of the chart, connected only to the US GDP data.
