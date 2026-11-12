
in the class, $p_{jg}$, where $\sum_{g=1}^G p_{j,g} = 1$. This average value is called the Gini index of the partition, and is defined by

$Gini_j = \sum_{g=1}^G p_{j,g}(1-p_{j,g}).$

A similar criterion is to minimize the cross-entropy or deviance, or impurity of the node, defined by

$Ent_j = - \sum_{g=1}^G p_{j,g} \log p_{j,g}.$ (8.3)

It is easy to see that the entropy assumes the maximum at a node when the split leads to equal proportion of observations in the groups and $p_{j,g} = 1/G$. It attains the minimum if the split has no classification error and, for each group $g$, either $p_{j,g} = 1$ or $p_{j,g} = 0$. Of course, this minimum value can only be achieved if the subsample to be split only contains observations from two groups. Note that, for small $x$, $\log(1-x) \approx -x$, and the entropy criterion would be similar to the Gini index. Both criteria are differentiable, and the most commonly used ones in practice. The pruning of the tree is as before, but using in Eq. (8.2) one of these criteria of optimal classification, instead of $SS(T_m)$.

### Example 8.1

Consider, again, the US monthly macroeconomic data of Example 7.1, but use the industrial production (IP) growth rate as the dependent variable. The predictors consist of lag-1 to lag-6 values of all 122 variables. We standardize the predictors for ease in graphical display. The tree-based method selects a tree with 20 leaves (terminal nodes) and the residual mean deviance (i.e., squared errors) is $2.52 \times 10^{-5}$. The 19 predictors selected for splitting are given in the R demonstration below. The first split is based on lag-1 value of the 33rd predictor with threshold -1.173. The output shows that, at the root node, there are 704 observations with deviance $3.88 \times 10^{-2}$ and sample mean 0.002163. Also, the first leaf has five observations with deviance $1.128 \times 10^{-4}$ and sample mean -0.01439. Figure 8.1 displays the selected regression tree.

### R demonstration: Regression tree

```
> require(tree)
> FREMDMApril19 <- read.csv("FREMDMApril19.csv", header=TRUE)
> x=as.matrix(FREMDMApril19)
> Y=x[7:710,6] # dependent variable
> X=cbind(x[6:709,],x[5:708,],x[4:707,],x[3:706,],x[2:705,],x[1:704,])
> X1 <- scale(X, center=TRUE, scale=TRUE)
> tree.fit <- tree(Y~.,data=data.frame(X1),split="deviance")
> tree.fit
node), split, n, deviance, yval
* denotes terminal node

1) root 704 3.880e-02 0.0021630
  2) V33L1 < -1.17333 72 1.144e-02 -0.0056860
    4) V15L1 < -1.30084 20 3.590e-03 -0.0157700
      8) V31L1 < 1.49551 11 8.355e-04 -0.0070080
16) V83L1 < 0.00411252 5 1.128e-04 -0.0143900 *
17) V83L1 > 0.00411252 6 2.230e-04 -0.0008548 *
19) V31L1 > 1.49551 9 8.754e-04 -0.0264900 *
5) V15L1 > -1.30084 52 5.033e-03 -0.0018060
10) V86L6 < 0.510091 44 3.044e-03 -0.0040430
20) V72L4 < 0.19261 34 8.944e-04 -0.0066460 *
21) V72L4 > 0.19261 10 1.273e-03 0.0041860
42) V118L2 < -0.265958 5 4.892e-04 0.0128700 *
43) V118L2 > -0.265958 5 2.915e-05 -0.0044990 *
11) V86L6 > 0.510091 8 5.579e-04 0.0105000 *
3) V33L1 > -1.17333 632 2.242e-02 0.0030570
6) V37L1 < 0.557606 481 1.526e-02 0.0021890
12) V87L1 < -0.648613 67 2.445e-03 -0.0014600
24) V104L3 < 0.854162 59 1.499e-03 -0.0003903 *
25) V104L3 > 0.854162 8 3.799e-04 -0.0093500 *
13) V87L1 > -0.648613 414 1.178e-02 0.0027790
26) V45L1 < -1.42523 14 6.244e-04 0.0106000
52) V88L5 < 0.722752 9 1.797e-04 0.0064910 *
53) V88L5 > 0.722752 5 1.840e-05 0.0180100 *
27) V45L1 > -1.42523 400 1.027e-02 0.0025050
54) V14L3 < -0.802891 34 1.297e-03 -0.00100940
108) V118L3 < -0.227833 15 3.127e-04 0.0032070 *
109) V118L3 > -0.227833 19 4.880e-04 -0.0044890 *
55) V14L3 > -0.802891 366 8.490e-03 0.0028390
110) V121L5 < 0.143741 239 5.161e-03 0.0020070 *
111) V121L5 > 0.143741 127 2.852e-03 0.0044060 *
7) V37L1 > 0.557606 151 5.643e-03 0.0058230
14) V47L3 < -0.830557 30 8.874e-04 0.0110000 *
15) V47L3 > -0.830557 121 3.753e-03 0.0045400
30) V20L2 < -0.0422253 32 8.761e-04 0.0008838 *
31) V20L2 > -0.0422253 89 2.296e-03 0.0058540
62) V76L5 < 0.928941 84 1.580e-03 0.0064280
124) V31L1 < 1.30745 75 1.012e-03 0.0056830 *
125) V31L1 > 1.30745 9 1.804e-04 0.0126400 *
63) V76L5 > 0.928941 5 2.232e-04 -0.0037850 *
> summary(tree.fit)
```

```
Regression tree:
tree(formula = Y ~., data = data.frame(X1), split = "deviance")
Variables actually used in tree construction:
[1] "V33L1" "V15L1" "V33L1" "V83L1" "V86L6" "V72L4" "V118L2" "V37L1"
[9] "V87L1" "V104L3" "V45L1" "V88L5" "V14L3" "V118L3" "V121L5"
[16] "V47L3" "V20L2" "V76L5" "V33L1"
Number of terminal nodes:  20
Residual mean deviance:  2.522e-05 = 0.01725 / 684
Distribution of residuals:
   Min.     1st Qu.      Median       Mean     3rd Qu.        Max.
-2.085e-02 -3.167e-03 -1.751e-05 0.000e+00  3.237e-03  1.993e-02
> plot(tree.fit)
> text(tree.fit, cex=0.5)
```

### 8.1.4 Random Forests

The RF is another ensemble method for classification and regression. As seen in Chapter 7 with boosting, ensemble methods combine different procedures or algorithms to obtain better predictions than those obtained from any of the constituent algorithms, as for instance, in model averaging. RF is an extension of bagging (bootstrap aggregating) to avoid overfitting in tree-based methods. The method was introduced by Ho (1995) and developed into a proper statistical tool by Breiman (2001). Our discussion below focuses on forecasting. Basically, RFs employ many simple trees to produce predictions, then combine those predictions to provide a consensus forecast. The method is nonparametric as it does not assume any specific model for the underlying data.

Again, consider the data set \(\{(x_t, y_t)|t = 1, ..., n\}\), where \(x_t\) is a realization of the predictor \(X_t = (X_{1t}, ..., X_{kt})'\) and \(y_t\) is a realization of the dependent variable \(Y_t\). The goal is to predict \(y_{t+h}\) with \(h\) being the forecast horizon. Before discussing RFs, we state the _bagging_ methodology below:

- 1. Draw a random sample with replacement from the data set. Denoted the bootstrap sample as \(\{(x^*_t, y^*_t)|t = 1, ..., n\}\).
- 2. Build a regression tree using the bootstrap sample to obtain a prediction for \(y_{t+h}\) of interest.

By repeating the aforementioned bootstrap procedure many times, one obtains a collection of bootstrap forecasts, which, in turn, can be used to obtain a consensus forecast of \(y_{t+h}\).

RFs extend the bagging procedure by introducing a novel idea to explore fully the relationship between \(y_{t+h}\) and \(x_t\). The idea is that, for each tree split, one selects a random sample \(g\) of predictors as candidates for splitting. Let \(g\) be a positive integer satisfying \(1 \leq g \leq k\). Often \(g = \lfloor k/3 \rfloor\) is used. To build a regression tree from a bootstrap sample for RFs, one takes a random sample of \(g\) predictors from \(\{1, ..., k\}\) to serve as candidates for splitting at each split. This novel step serves multiple purposes. First, it reduces the computation intensity, especially when \(k\) is large. Second, it mitigates the multicollinearity between the predictors, because highly correlated predictors may not be jointly selected. Finally, it avoids the chance of using similar trees in the ensemble. A simple average of bootstrap forecasts is often used to produce a consensus prediction. Weighted average can also be used under a given loss function.

**Discussion**: Some discussions of RFs are in order. First, the R package **randomForest** can be used to perform RF prediction. See the demonstration below. Second, some modifications are needed to apply effectively RFs to dependent data. For instance, in time series analysis, the lag-1 value of the dependent variable is often more important than the lag-2 value. Similarly, seasonal lags of the dependent variable are often important in modeling seasonal time series. Consequently, one might use a weighted random sample to select the \(g\) predictors as candidates for splitting at each split instead of using a pure random sample. Third, care must be exercised in drawing bootstrap samples for dependent data and some form of block bootstrap should be used; see, for instance, Alonso et al. (2004) and Lahiri (2013).

### Example 8.2

To demonstrate the use of RFs, we also employ the monthly macroeconomic data used in Example 8.1. Again, the dependent variable is the monthly growth rate of US
