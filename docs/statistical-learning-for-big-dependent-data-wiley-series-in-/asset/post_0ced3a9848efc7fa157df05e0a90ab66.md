

Since the criterion only takes the minimum distance into account, it may happen that a series in C and a series in (AB) are far apart, even though d(C, AB) is small. In practice, this criterion tends to produce long drawn out clusters, which may contain quite dissimilar elements at the opposite ends.

- 2. Complete linkage or furthest neighbor: The distance between C and the new element (AB) is the largest distance between a series in C and a series in A or B. That is,
  \( d(C, AB) = \max\{d_{CA}, d_{CB}\} \).

As this criterion finds the maximum distance, it is possible that an element C is closer to elements in A than to elements in B, although most time series in C are closer to the time series in B than those to A. In practice, this criterion tends to produce spherical clusters.

- 3. Average linkage: The distance between C and the new group (AB) is average of the distances between C and elements in (AB). That is
  \( d(C, AB) = \frac{\sum_{c \in C} \sum_{g \in AB} d_{cg}}{n_c n_{ab}} \),

where \( n_{ab} \) is the number of time series in (AB). This criterion is a compromise between the previous two criteria. The main drawback is that the resulting distance depends on the number of elements in C and in AB whereas the other two criteria do not depend on the number of elements in a group.

## 5.2.2 The Dendrogram

The dendrogram, or hierarchical tree diagram, is a tree-type graphical display of clustering results with a hierarchical algorithm. This display is useful if the series follow a hierarchical structure, but can be misleading otherwise. Also, it is hard to comprehend with a large data set. The dendrogram is constructed in the following way:

- 1. The k initial series are displayed in the bottom part of the graph.
- 2. The fusion between the series are represented by three straight lines: two vertical ones connecting the series and a horizontal line that indicates the (distance) level at which the fusion occurs.
- 3. The process is repeated until all series are connected by straight lines.

If we cut the dendrogram at any given distance (height or depth), we obtain a classification of time series into groups at that distance level. Members of each group are also available. In the next section, we discuss some procedures to determine the number of groups.

## 5.2.3 Selecting the Number of Groups

### 5.2.3.1 The Height and Step Plots

The output of a hierarchical clustering includes the sequence of dissimilarities or distances \( d^{(1)} \le d^{(2)} \le \cdots \le d^{(k)} \) at which two elements are merged. These distances correspond to the heights in the dendrogram. It is not hard to imagine that if \( d^{(i)} \) and \( d^{(i+1)} \) are close then the difference between elements formed with height \( d^{(i)} \) and those formed with height \( d^{(i+1)} \) is relatively small. Therefore, the heights in a dendrogram and their increments
\(\Delta_i = d^{(i+1)} - d^{(i)}\) provide certain information about the number of clusters. To make use of this information in determining the number of clusters, we propose a *Height plot* and a *Step plot* for a set of time series \(\{\boldsymbol{z}_1, \dots, \boldsymbol{z}_T\}\), where \(\boldsymbol{z}_t\) is a \(k\)-dimensional time series:

- 1. Select one of the distance measures discussed before, such as the ACF distance, to build a dendrogram for \(\boldsymbol{z}_t\).
- 2. Let \(y_t\) be a randomly selected series from \(\boldsymbol{z}_t\).
- 3. Build a time series model for \(y_t\). For simplicity, we focus on the AR model with order selected by an information criterion, such as Akaike's information criterion (AIC) or Bayesian information criterion (BIC).
- 4. Perform the following simulation \(M\) iterations:
  - a. For the \(j\)th iteration, use the AR model for the randomly selected series \(y_t\) to generate \(k\) time series of sample size \(T\). Denote the simulated data for the \(j\)th iteration as \(\{z_t^{(j)}\}\).
  - b. Apply the same distance measure to \(\{z_t^{(j)}\}\) to obtain a dendrogram and call its set of minimum distances \(\{d_j^{(1)} \le d_j^{(2)} \le \cdots \le d_j^{(k)}\}\).
- 5. Let \(\lambda_{0.95}^{(i)}\) be the \(95\%\)th quantile of the set \(\{d_j^{(i)} | j=1, \dots, M\}\).
- 6. Consider a scatterplot of \(\{d^{(i)} | i=1, \dots, k\}\) of the observed data \(\{\boldsymbol{z}_t\}\) versus \(i\). Add to the scatterplot the line \(\{\lambda_{0.95}^{(i)}\}\) versus \(i\). The number of clusters is then \(g+1\), where \(g\) is the number of points at the right end of the scatterplot that are above the \(\lambda_{0.95}^{(i)}\) line.

We referred to this plot the *Height plot* of a dendrogram. Since the simulated time series are all from a single model, every set of the series \(\{z_t^{(j)}\}\) should have a single cluster. Therefore, the heights \(\{d_j^{(i)}\}\) have no particular meaning except for the scale of the heights. It is then reasonable to use these heights to provide a reference point for the behavior of the heights \(\{d^{(i)}\}\) of the observed data. If the observed data came from a single cluster, the \(d^{(i)}\) should behave in a similar manner as the simulated reference. Therefore, a deviation from the reference line, given by the \(95\%\)th quantiles \(\{\lambda_{0.95}^{(i)} | i=1, \dots, k\}\), signifies the existence of clusters.

Instead of using the heights \(d^{(i)}\) directly, one can obtain a scatterplot of the increments in heights \(\{D_i | i=1, \dots, k\}\), or steps, versus the index \(i\). This is called the *Step plot*. The reference line is then the increments in the \(95\%\)th quantiles of the heights.

To illustrate, we consider the Taiwan AirBox data set that consists of hourly series of PM\(_{2.5}\) data. After removing the outlying series (29 and 70), we have \(k=514\) and \(T=744\). Figure 5.10 shows the resulting height plot. From the plot, there are two points that appear to stand out with respect to the reference line obtained from the suggested simulation procedure with \(M=100\) iterations. Therefore, the height plot suggests that there are three clusters in the data set. Figure 5.11 shows the resulting step plot. In this particular case, the step plot suggests two clusters for the AirBox PM\(_{2.5}\) data set, but also lends some support for three clusters. The plots are obtained by the command `stepp` of the package for the book.

![](img/d4c84ae3c2178f4c506af3dd2ed13122_252_0.png)

Figure 5.10 The scatterplot of heights of dendrogram for the Taiwan AirBox PM2.5 data set. The solid line is a reference line obtained by simulation using a single model for a randomly selected series of the data. ACF is used in distance measure.

![](img/d4c84ae3c2178f4c506af3dd2ed13122_252_1.png)

Figure 5.11 The scatterplot of increments in heights of dendrogram for the Taiwan AirBox PM2.5 data set. The solid line is a reference line obtained by simulation using a single model for a randomly selected series of the data. ACF is used in distance measure.

### 5.2.3.2 Silhouette Statistic

The Silhouette statistic, proposed by Rousseeuw (1987), computes a measure of the quality of the final clustering result and selects the number of clusters to maximize the quality measure. The quality of the classification of one element can be defined as the difference between its average distance to members of its group and its average distance to members of the group closest to the one it belongs. Thus, given a distance measure between elements and a clustering result, the average distance of an element to all members of its group is compared with the average distance to members in its closest group. The closest group of an element is defined as a group, different from the one the element is included, that have members with the smallest average distance from the element. If the element is well classified, the average distance to elements in its group should be smaller than that to elements in its closest group, and the larger the difference the better the quality of the classification of this element. This idea can be extended to all the elements in the set and define the quality of the given classification as the average quality of the classification of all the elements. It is computed as follows.

Suppose we have a classification of the observations in \(g\) groups, say \(G_1, \dots, G_g\) with \(g > 1\). Let \(d(i, i')\) be the distance or dissimilarity used to build the groups. Let \(a(i)\) be the average distance of the \(i\)th element to other elements in its group. For instance, if the \(i\)th observation belongs to group \(G_j\) with \(n_j\) members and \(i'\) is any of the other members of this group, \(a(i)\) is given by

\[a(i) = \frac{\sum_{i' \in G_j} d(i, i')}{n_j - 1} .\]

To judge the value \(a(i)\), whether the element \(i\) is well classified, we compare \(a(i)\) to the average distance of the \(i\)th element to members in other groups \(G_h, h = (1, \dots, g, | h \neq j)\). The average distance to group \(G_h\), with \(n_h\) elements is given by

\[b_{G_h}(i) = \frac{\sum_{j \in G_h} d(i, j)}{n_h} .\]

Define the closest group to the \(i\)th observation as the group with minimum average distance. Note that the concept of closest group is defined for a particular element, and that different members of the same group may have, in general, different closest groups. Let \(c(i)\) be the average distance of the \(i\)th element in the \(G_j\) group to its closest group computed as

\[c(i) = \min_{h \neq j} [b_{G_h}(i)] .\]

The quality of the classification of the \(i\)th observation is then defined

\[s(i) = \frac{c(i) - a(i)}{\max(a(i), c(i))} .\]

This measure is called the *silhouette measure* of element \(i\). If \(c(i) > a(i)\), the element is well classified and \(s(i) > 0\). The maximum value of the silhouette measure is \(s(i) = 1\), which occurs when \(a(i) = 0\), because then the observation is identical to all other members of its group, showing a perfect fit. In general, for observations well classified, \(a(i) < c(i)\) and \(0 < s(i) < 1\). On the other hand, when the observation is not well classified \(s(i) < 0\), which means that the average distance to its closest group is smaller
