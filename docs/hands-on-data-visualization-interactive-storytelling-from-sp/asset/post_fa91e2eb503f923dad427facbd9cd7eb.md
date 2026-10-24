
![](img/216890131d46798dfe1cd5f886cc608c_112_0.png)

✓ Good. Y-axis starts at 0 and has natural increments

✗ Bad. Y-axis starts at 300 exaggerating the difference among columns. Intervals are awkward.

Figure 6-2. Start your bar chart at zero.

The zero-baseline rule does not apply to line charts. According to visualization expert Alberto Cairo, line charts represent values through the position and angle of the line, rather than its height or length. Starting a line chart at a number other than zero does not necessarily distort its encoded information because our eyes rely on its shape to determine its meaning, rather than its proximity to the baseline.² For example, compare both the right and left sides of Figure 6-3, where both are correct.

![](img/216890131d46798dfe1cd5f886cc608c_113_0.png)

![](img/216890131d46798dfe1cd5f886cc608c_113_1.png)

Figure 6-3. Since line charts do not require a zero baseline, both sides are correct.

Furthermore, while forcing a line chart to begin at the zero baseline is acceptable, it may not produce the best visualization for your data story. In Figure 6-4, the left side shows a line chart that starts the vertical axis at zero, but as a result, the line appears very flat at the top of the chart and hides changes in values. The right side shows a line chart where the vertical axis was reduced to match the range of values, which results in a clearer depiction of change. Both sides are technically correct, and in this case, the right side is a better fit for the data story. Still, you need to be cautious, because, as you’ll learn in “How to Lie with Charts”, people can mislead us by modifying the vertical axis, and there is no uniform rule about where it belongs on a line chart.

Acceptable, but starting the vertical axis at zero obscures changes in values

Better: By reducing the vertical axis to match the values, we see changes more clearly

Figure 6-4. While the line chart with the zero baseline is acceptable, the line chart with a modified baseline more clearly tells a data story about change.

## Pie charts represent 100%

Pie charts are one of the most contentious issues in data visualization. Most dataviz practitioners will recommend avoiding them entirely, saying that people are bad at accurately estimating sizes of different slices. We take a less dramatic stance as long as you adhere to the recommendations we give in the next section.

The one and only thing in data visualization that every single professional will agree on is that pie charts represent 100% of the quantity. If slices add up to anything other than 100%, it’s a crime. If you design a survey titled, “Are you a cat or a dog person?” and allow both “cat” and “dog” checkboxes to be selected, forget about putting the results into a pie chart.

## Chart Aesthetics

Remember that you create a chart to help the reader understand the story, not to confuse them. Decide if you want to show raw counts, percentages, or percent changes, and do the math for your readers.

## Avoid chart junk

Start with a white background and add elements as you see appropriate. You should be able to justify each element you add. To do so, ask yourself: Does this element improve the chart, or can I drop it without decreasing readability? This way you won’t end up with so-called “chart junk” as shown in Figure 6-5, which includes 3D perspectives, shadows, and unnecessary elements. They might have looked cool in early versions of Microsoft Office, but let’s stay away from them today. Chart junk distracts the viewer and reduces chart readability and comprehension. It also looks unprofessional and doesn’t add credibility to you as a storyteller.

![](img/216890131d46798dfe1cd5f886cc608c_115_0.png)

X Bad. Avoid elements that do not carry information.

Figure 6-5. Chart junk distracts the viewer, so stay away from shadows, 3D perspectives, unnecessary colors and other fancy elements.

Don’t use shadows or thick outlines with bar charts, because the reader might think that decorative elements are part of the chart, and thus misread the values that bars represent.

The only justification for using three dimensions is to plot three-dimensional data, which has x, y, and z values. For example, you can build a three-dimensional map of population density, where x and y values represent latitude and longitude. In most cases, however, three dimensions are best represented in a bubble chart, or a scatterplot with varying shapes and/or colors.

## Beware of pie charts

Remember that pie charts show only part-to-whole relationship, so all slices need to add up to 100%. Generally, the fewer slices, the better. Arrange slices from largest to smallest, clockwise, and put the largest slice at 12 o’clock, as shown in Figure 6-6.

![](img/216890131d46798dfe1cd5f886cc608c_116_0.png)

✓ ok

X Bad. Largest share should start at 12 o'clock and go clockwise.

X Everything is wrong

Figure 6-6. Sort slices in pie charts from largest to smallest, and start at 12 o'clock.

If your pie chart has more than five slices, consider showing your data in a bar chart, either stacked or split, like Figure 6-7 shows.

Consider using
bar charts
instead of pies

Better

Better

Figure 6-7. Consider using bar charts instead of pies.

## Don’t make people turn their heads to read labels

When your column chart has long x-axis labels that have to be rotated (often 90 degrees) to fit, consider turning the chart 90 degrees so that it becomes a horizontal bar chart. Take a look at Figure 6-8 to see how much easier it is to read horizontally-oriented labels.

![](img/216890131d46798dfe1cd5f886cc608c_118_0.png)

Figure 6-8. For long labels, use horizontal bar charts.

## Arrange elements logically

If your bar chart shows different categories, consider ordering them, as shown in Figure 6-9. You might want to sort them alphabetically, which can be useful if you want the reader to be able to quickly look up an item, such as their town. Ordering categories by value is another common technique that makes comparisons possible. If your columns represent a value of something at a particular time, they have to be ordered sequentially, of course.

![](img/216890131d46798dfe1cd5f886cc608c_119_0.png)

*Figure 6-9. For long labels, use horizontal bar charts.*

## Do not overload your chart

When labeling axes, choose natural increments that space equally, such as [0, 20, 40, 60, 80, 100], or [1, 10, 100, 1000] for a logarithmic scale. Don’t overload your scales. Keep your typography simple, and use (but do not overuse) **bold type** to highlight major insights. Consider using commas as thousands separators for readability (1,000,000 is much easier to read than 1000000).

## Be careful with colors

In this section, we would like to briefly introduce three important rules about colors. First, remember that in most cases monochromatic (single-hue) charts suffice, and there may be no need to introduce the extra dimension of color at all.

Second, refer to the color wheel and standard **harmony rules** when choosing your palette. Consider the rule of complementary colors—opposites in the color wheel—to find color pairs, such as blue and orange or yellow and purple. Analogous colors, or neighbors in the color wheel, make good palettes, such as orange, red, and pink.

Third, stay away from pure saturated colors and instead choose their “earthier” versions, such as olive green instead of bright green, or navy instead of neon blue.

Once you have chosen the color palette for your visualization, ask yourself:

- Is there a conflict of meaning between colors and the phenomenon they represent? Am I using red to represent profit or green to represent death rate? This question is complex as colors carry different associations for different social groups and cultures, but try to exercise your best sensitivity.
- Can people with color blindness interpret your chart? Palettes that contain reds and greens or yellows and blues can be challenging. Consider using Color Oracle or another simulator to make sure your visualization is accessible.
- Will the colors be distinguishable in black and white? Even if you don’t expect viewers to print your chart, they may. You can use Color Oracle or another simulator to check that your colors have different brightness levels and look distinguishable in grayscale. Figure 6-10 shows some good and bad examples of color use.
