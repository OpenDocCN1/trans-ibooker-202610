# Solution

Sample average: $\bar{x}=\sum_{i=1}^{n}x_i=\sum_{i=1}^{12}x_i/12=673.1/12=56.09\ \text{mg/l}$

Sample standard deviation: $\sum_{i=1}^{12}x_i=673.10;\sum_{i=1}^{12}x_i^2=39\ 178$

$S=\sqrt{\frac{\sum_{i=1}^{n}x_i^2-\left(\sum_{i=1}^{n}x_i\right)^2/n}{n-1}}$

$S=\sqrt{\frac{39\ 168-(673.10)^2/12}{12-1}}=\sqrt{\frac{1\ 412.70}{11}}$ $=\sqrt{128.43}=11.33\ mg/l$

# Example 8.2 The following data are direct solar intensity measurements (W/m²) on different days at a location in the State of Rajasthan, India: 562, 869, 708, 775, 775, 704, 809, 856, 655, 806, 878, 909, 918, 558, 768, 870, 918, 940, 946, 661, 820, 898, 935, 952, 957, 693, 835, 905, 939, 955, 960, 498, 653, 730, and 753. Calculate the sample mean and sample standard deviation. Prepare a dot diagram of these data. Indicate where the sample mean falls on this diagram. Provide a practical interpretation of the sample mean.

# Solution

Sample average: $\bar{x}=\frac{\sum_{i=1}^{35}x_i}{35}=\frac{28\ 368}{35}=810.51\ \text{W/m}^2$

Sample variance: $\bar{x}=\frac{\sum_{i=1}^{35}x_i}{35}=28\ 368$ $\sum_{i=1}^{35}x_i^2=23\ 552\ 500$

Sample standard deviation: $S^2=\frac{\sum_{i=1}^{n}x_i^2-\left(\sum_{i=1}^{n}x_i\right)^2}{n-1}$; $S=\sqrt{\frac{\sum_{i=1}^{n}x_i^2-\left(\sum_{i=1}^{n}x_i\right)^2/n}{n-1}}$ $=\frac{23\ 552\ 500-(28\ 368)^2/35}{35-1}=\frac{559\ 830.74}{34}=16\ 465.62\ \text{W/m}^2$

Therefore, sample deviation, $S=\sqrt{16\ 465.62}=128.32\ \text{W/m}^2$

The sample standard deviation could also be found using $S=\sqrt{\frac{\sum_{i=1}^{n}(x_i-\bar{x})^2}{n-1}}$, where $\sum_{i=1}^{35}(x_i-\bar{x})^2=559\ 830.74$

Dot diagram (rounding of the data is used to create the dot diagram)

![](bbox=[0.53, 0.27, 0.92, 0.385])

The sample mean is the point at which the data would balance if it were on a scale.

# 8.2.2 Stem-and-Leaf Diagram

The dot diagram is a useful data display for small samples, up to (say) about 20 observations. However, when the number of observations is moderately large, other graphical displays may be more useful. A stem-and-leaf display or stem-and-leaf plot, also called a stemplot, is a device for presenting quantitative data in a graphical format, similar to a histogram, to assist in visualizing the shape of a distribution. They evolved from Arthur Bowley’s work in the early 1900s and are useful tools in exploratory data analysis. Stemplots became more commonly used in the 1980s after the publication of John Tukey’s book on exploratory data analysis in 1977 (Tukey 1977).

A stem-and-leaf display often refers to another chart type. A simple stemplot may refer to plotting a matrix of y values onto a common x-axis, and identifying the common x value with a vertical line, and the individual y values with symbols on the line (Wild and Seber 2000).

Unlike histograms, stem-and-leaf displays retain the original data to at least two significant digits, and put the data in order, thereby easing the move to order-based inference and nonparametric statistics.

A basic stem-and-leaf display contains two columns separated by a vertical line. The left column contains the stems and the right column contains the leaves.