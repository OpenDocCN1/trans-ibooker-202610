+   3. Check whether reassigning some of the objects improves the criterion SSW. This would happen if an object in a group is closer to the centroid of another group. In this case, it is moved to the new group and the centroids of the two groups affected by the change are recomputed.

+   4. When it is no longer possible to improve the criterion, stop the algorithm.

Consequently, the result of the algorithm can depend on the initial allocation and order of the items. The algorithm should always be repeated from different starting values and permuting the items in the sample. The effect of the order is usually small, but it is advisable to make sure it has no influence in each case.

Both the SSW and trace criteria used by k-means have two important properties. First, they are not invariant to scaling changes. Second, minimizing the Euclidean distance produces approximately spherical groups. With respect to the scale, if the variables are in different units, it is better to standardize them so that the result of the k-means does not depend on irrelevant changes in the scale of the measurements. When they are in the same units, it is usually better not to standardize, as larger variance in one variable may be due precisely to the fact that there are several groups of items with different values of this variable, and such information could be hidden if the data are standardized.

The expected spherical groups produced by k-means can be problematic when the groups are expected to have very different shapes. In this case, we can apply variable transformations so that their joint distribution inside the groups is expected to be approximately normal with diagonal covariance matrix.

**5.3.1.1 Number of Groups** The Silhouette and Gap statistics of Section 5.2 can be applied to determine the number of groups in k-means. In these cases, the distances are the Euclidean distances between the coordinates of the features that represent the series. There are some additional criteria that are specific for variables. Hartigan (1975) proposed to carry out an approximate F-test of variability reduction, comparing SSW<sub>g</sub> to SSW<sub>g+1</sub> and calculating the relative reduction in variability with the increase of an additional group. The test is

\( F = \frac{SSW_g - SSW_{g+1}}{SSW_{g+1}/(n - g - 1)}, \) (5.27)

which measures the decrease in variability by increasing one group with the average variability in the clustering. The value of F is compared to an F distribution with p and p(n - g - 1) degrees of freedom. However, such a comparison may not be justified in applications. As a general rule, Hartigan (1975) suggested to introduce an additional group if this quotient is greater than 10.

Calinski and Harabasz (1974) proposed to maximize the ratio of the between-groups sum of squares and the within-groups sum of squares. Note that the covariance matrix of the data, V, can be written as

\(\mathbf{V} = \sum_{i=1}^k (\mathbf{x}_i - \bar{\mathbf{x}})(\mathbf{x}_i - \bar{\mathbf{x}})' = \sum_{g=1}^G \sum_{i=1}^{n_g} (\mathbf{x}_{ig} - \bar{\mathbf{x}}_g)(\mathbf{x}_{ig} - \bar{\mathbf{x}}_g)' + \sum_{g=1}^G n_g (\bar{\mathbf{x}}_g - \bar{\mathbf{x}})(\bar{\mathbf{x}}_g - \bar{\mathbf{x}})',\)

and applying the trace operator we have

\(\text{tr}(\mathbf{V}) = \text{SSW} + \text{SSB}.\)