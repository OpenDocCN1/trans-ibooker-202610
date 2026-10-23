# 8.2.3 Constructing a Stem-and-Leaf Display

Example 8.3 To construct a stem-and-leaf display, the observations must first be sorted in ascending order: this can be done most easily if working by hand by constructing a draft of the stem-and-leaf display with the leaves unsorted, then sorting the leaves to produce the final stem-and-leaf display. Here is the sorted set of data values that will be used in the following example:

44, 46, 47, 49, 63, 64, 66, 68, 68, 72, 72, 75, 76, 81, 84, 88, 106

Next, it must be determined what the stems will represent and what the leaves will represent. Typically, the leaf contains the last digit of the number and the stem contains all of the other digits. In the case of very large numbers, the data values may be rounded to a particular place value (such as the hundreds place) that will be used for the leaves. The remaining digits to the left of the rounded place value are used as the stem. In this example, the leaf represents the ones place and the stem will represent the rest of the number (tens place and higher).

The stem-and-leaf display is drawn with two columns separated by a vertical line. The stems are listed to the left of the vertical line. It is important that each stem is listed only once and that no numbers are skipped, even if it means that some stems have no leaves. The leaves are listed in increasing order in a row to the right of each stem.

It is important to note that when there is a repeated number in the data (such as two 72s) then the plot must reflect such (so the plot would look like 7 | 2 2 5 6 7 when it has the numbers 72 72 75 76 77).

| Stem | Leaf | Frequency |
|---|---|---|
| 4 | 4 6 7 9 | 4 |
| 5 |  | 0 |
| 6 | 3 4 6 8 8 | 5 |
| 7 | 2 2 5 6 | 4 |
| 8 | 1 4 8 | 3 |
| 9 |  | 0 |
| 10 | 6 | 1 |

Key : 4 | 4 = 44

Rounding may be needed to create a stem-and-leaf display. Based on the following set of data, the stemplot below would be created:

−23.6 78 758, −12.45, −3.4, 4.43, 5.5, 5.678, 16.87, 24.7, 56.8

For negative numbers, a negative is placed in front of the stem unit, which is still the value X/10. Non-integers are rounded. This allowed the stem-and-leaf plot to retain its shape, even for more complicated data sets as in this example below:

| Stem | Leaf | Frequency |
|---|---|---|
| −2 | 3 | 1 |
| −1 | 2 | 1 |
| −0 | 3 | 1 |
| 0 | 4 6 6 | 3 |
| 1 | 7 | 1 |
| 2 | 5 | 1 |
| 3 |  | 0 |
| 4 |  | 0 |
| 5 | 7 | 1 |

Key : −1 1 2 = −12

The stem-and-leaf diagram displays make it relatively easy to find data features such as percentiles, quartiles, and median. The median is a measure of central tendency that divides the data into two equal parts, half below the median and half above. If the number of observations is even, the median is halfway between the two central values.

# 8.2.4 Application

Stem-and-leaf displays are useful for displaying the relative density and shape of the data, giving the reader a quick overview of distribution. They retain (most of) the raw numerical data, often with perfect integrity. They are also useful for highlighting outliers and finding the mode. However, stem-and-leaf displays are only useful for moderately sized data sets (around 15–150 data points). With very small data sets a stem-and-leaf displays can be of little use, as a reasonable number of data points are required to establish definitive distribution properties. A dot plot may be better suited for such data (Example 8.1). With very large data sets, a stem-and-leaf display will become very cluttered, since each data point must be represented numerically. A histogram or boxplot may become more appropriate as the data size increases.

# 8.2.5 Histogram

A histogram is a more compact summary of data than a stemplot. A histogram is an accurate representation of the distribution of numerical data. It is an estimate of the probability distribution of a continuous variable (quantitative variable) and was first introduced by Karl Pearson