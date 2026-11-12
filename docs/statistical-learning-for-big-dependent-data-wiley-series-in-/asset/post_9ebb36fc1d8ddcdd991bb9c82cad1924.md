
```
>names (A1)
>A1$s
#[1] 7.210554
>dim(A1$G)
#[1] 67 60
>dim(A1$M)
#[1] 60 60
>new <- 100
>x_lon <- seq(min(loc1[,1]) , max(loc1[,1]) , length=new)
>x_lat <- seq(min(loc1[,2]) , max(loc1[,2]) , length=new)
>newloc <- as.matrix(expand.grid(x=x_lon,y=x_lat))
>Gpred <- predict(A1$G,newx=newloc)
>G <- A1$G
>Mhat <- A1$M
>dd <- eigen(G %*% Mhat %*% t(G) )
>fhat <- Gpred %*% Mhat %*% t(G) %*% dd$vector[,1:2]
>quilt.plot(newloc , -fhat[,1] , nx=new , ny=new , xlab="lon" , ylab="lat")
>map("state" , xlim=range(loc1[,1]) , ylim=range(loc1[,2]) , add=T)
>map.text("state" , xlim=range(x_lon) , ylim=range(x_lat) , add=T)
#
>quilt.plot(newloc , -fhat[,2] , nx=new , ny=new , xlab="lon" , ylab="lat")
>map("state" , xlim=range(loc1[,1]) , ylim=range(loc1[,2]) , add=T)
>map.text("state" , xlim=range(x_lon) , ylim=range(x_lat) , add=T)
```

## 9.7.4 Spatial Principal Component Analysis

Instead of using prespecified spatial basis functions, one can use empirical principal component analysis (PCA) to select suitable basis functions for random-effects modeling of spatial data. Wang and Huang (2017) propose a regularized PCA approach, which is relatively easy to use and data adaptive.

For simplicity, assume that there are no covariates, and \(Y(\mathbf{s}, t)\) is zero mean with a common spatial covariance function \(C_y(\mathbf{s}_1,\mathbf{s}_2) = \text{Cov}[Y(\mathbf{s}_1, t), Y(\mathbf{s}_2, t)]\) for all \(t = 1, \dots, T\). As before, we assume that the data are observed at locations \(\mathbf{s}_1, \dots, \mathbf{s}_m\) on the spatial domain \(D \subset R^d\) with independent measurement errors. Furthermore, assume that \(\{Y(\mathbf{s}, t)\}\) are mutually temporally uncorrelated. Consider a rank \(K\) spatial random-effects model for \(Y(\mathbf{s}, t)\):

```
Y(\mathbf{s}, t) = [\psi_1(\mathbf{s}), \dots, \psi_K(\mathbf{s})]\boldsymbol{\xi}_t = \sum_{i=1}^K \xi_{ti}\psi_i(\mathbf{s}), \quad \mathbf{s} \in D; t = 1, \dots, T, \quad (9.88)
```

where \(\{\psi_i(\mathbf{s})\}\) are unknown orthonormal basis functions, \(\boldsymbol{\xi}_t = (\xi_{t1}, \dots, \xi_{tK})' \sim (\mathbf{0}, \boldsymbol{\Lambda})\) (\(t = 1, \dots, T\)) are uncorrelated random variables and \(\boldsymbol{\Lambda}\) is an unknown symmetric nonnegative-definite matrix.

Write \(\boldsymbol{\Lambda} = [\lambda_{ij}]_{K \times K}\). The spatial covariance function of \(Y(\mathbf{s}, t)\) is

```
C_Y(\mathbf{s}_1, \mathbf{s}_2) = \sum_{i=1}^K \sum_{j=1}^K \lambda_{ij} \psi_i(\mathbf{s}_1)\psi_j(\mathbf{s}_2). \quad (9.89)
```

Let \(\boldsymbol{\Lambda} = \mathbf{V} \boldsymbol{\Lambda}^* \mathbf{V}'\) be the eigen-decomposition of \(\boldsymbol{\Lambda}\), where _V_ consists of the _K_ orthonormal eigenvectors and \(\boldsymbol{\Lambda}^* = \text{diag}\{\lambda_1^*, \dots, \lambda_K^*\}\), where \(\lambda_1^* \ge \cdots \ge \lambda_K^*\) are the corresponding eigenvalues. Let \(\boldsymbol{\xi}_t^* = \mathbf{V}' \boldsymbol{\xi}_t\) and \([\psi_1^*(\mathbf{s}), \dots, \psi_K^*(\mathbf{s})] = [\psi_1(\mathbf{s}), \dots, \psi_K(\mathbf{s})]\mathbf{V}\).

It is easy to see that \(\{\psi_i^*(\mathbf{s})\}\) are also orthonormal and \(\xi_{ti}^* \sim (0, \lambda_i^*)\) for \(t = 1, \dots, T\) and \(i = 1, \dots, K\), are mutually uncorrelated. Consequently, we have

```
Y(s, t) = [\psi_1^*(\mathbf{s}), \dots, \psi_K^*(\mathbf{s})]\boldsymbol{\xi}_t^* = \sum_{i=1}^K \xi_{ti}^*\psi_i^*(\mathbf{s}), \quad \mathbf{s} \in D. \quad (9.90)
```

This expansion is known as the Karhunen-Loève expansion of \(Y(s, t)\) with \(K\) non-zero eigenvalues, where \(\psi_i^*(\mathbf{s})\) is the \(i\)th eigenfunction of \(C_y(\cdot, \cdot)\) with \(\lambda_i^*\) being the corresponding eigenvalue. See Karhunen (1947) and Loève (1978).

The observed data \(\mathbf{Z}_t = [Z(s_1, t), \dots, Z(s_m, t)]'\) can then be written as

```
\mathbf{Z}_t = \mathbf{Y}_t + \boldsymbol{\epsilon}_t = \boldsymbol{\Psi}\boldsymbol{\xi}_t + \boldsymbol{\epsilon}_t, \quad t = 1, \dots, T, \quad (9.91)
```

where \(\boldsymbol{\epsilon}_t\) denotes the vector of independent measurement errors with \(\boldsymbol{\epsilon}_t \sim (0, \sigma^2\mathbf{I})\), \(\mathbf{Y}_t\) consists of the latent S-T process \([Y(s_1, t), \dots, Y(s_m, t)]'\), \(\boldsymbol{\Psi} = [\psi_1, \dots, \psi_K]\) is an \(m \times K\) matrix with \((i, j)\)th element being \(\psi_j(s_i)\), and \(\boldsymbol{\xi}_t\) and \(\boldsymbol{\epsilon}_t\) are uncorrelated. The goal here is to find the first \(p \le K\) dominant patterns \(\psi_1, \dots, \psi_p\) with large \(\lambda_1^*, \dots, \lambda_p^*\) and to estimate the covariance function \(C_y(\cdot, \cdot)\) for spatial prediction. Equation (9.91) is in the form of a latent factor model, and PCA provides a way to achieve our goal. However, if the sample size \(T\) is not large compared with the dimension \(m\), the PCA estimates of the latent factor and loadings would vary substantially. To overcome the difficulty, Wang and Huang (2017) propose to use regularization and impose certain smoothness constraints in estimation. It turns out from the theory of smoothing splines (Green and Silverman, 1994) that the estimates \(\hat{\boldsymbol{\Psi}}(\mathbf{s})\) is a natural cubic spline when \(d = 1\) and a thin-plate spline when \(d = 2\) or \(3\) with nodes at \(\{s_1, \dots, s_m\}\). More specifically, let \(\mathbf{Z} = (\mathbf{Z}_1, \dots, \mathbf{Z}_T)'\) be the \(T \times m\) data matrix with the understanding that the mean of \(\mathbf{Z}_t\) is zero. The sample covariance matrix as \(\mathbf{S} = \mathbf{Y}'\mathbf{Y} / T\). Wang and Huang (2017) estimate \(\boldsymbol{\Psi}\) as follows

```
\hat{\boldsymbol{\Psi}}_{\tau_1,\tau_2} = \arg \min_{\boldsymbol{\Psi}:\boldsymbol{\Psi}'\boldsymbol{\Psi}=\mathbf{I}_K} \|\mathbf{Z} - \mathbf{Z}\boldsymbol{\Psi}\boldsymbol{\Psi}'\|_F^2 + \tau_1 \sum_{i=1}^K \boldsymbol{\psi}_i'\boldsymbol{\Omega}_i\boldsymbol{\psi}_i + \tau_2 \sum_{i=1}^K \sum_{j=1}^m |\psi_{ji}|, \quad (9.92)
```

subject to the constraints \(\boldsymbol{\psi}_1'\mathbf{S}\boldsymbol{\psi}_1 \ge \boldsymbol{\psi}_2'\mathbf{S}\boldsymbol{\psi}_2 \ge \dots \ge \boldsymbol{\psi}_K'\mathbf{S}\boldsymbol{\psi}_K\), where \(\tau_1\) and \(\tau_2\) are penalty parameters with values determined by cross-validation, and \(\|\mathbf{M}\|_F = (\sum_{i,j} m_{ij}^2)^{1/2}\) is the Frobenius norm of the matrix \(\mathbf{M} = [m_{ij}]\). Furthermore, to estimate the covariance function \(C_y(\cdot, \cdot)\), we also need to estimate \(\sigma^2\) and \(\boldsymbol{\Lambda}\). To this end, Wang and Huang apply a regularized least squares method of Tzeng and Huang (2015)

```
(\hat{\sigma}^2, \hat{\boldsymbol{\Lambda}}) = \arg \min_{\sigma^2, \boldsymbol{\Lambda} \mid \sigma^2 \ge 0, \boldsymbol{\Lambda} \ge 0} \left\{ \frac{1}{2} \|\mathbf{S} - \hat{\boldsymbol{\Psi}}\hat{\boldsymbol{\Lambda}}\hat{\boldsymbol{\Psi}}' - \sigma^2\mathbf{I}\|_F^2 + \gamma \|\hat{\boldsymbol{\Psi}}\hat{\boldsymbol{\Lambda}}\hat{\boldsymbol{\Psi}}'\|_* \right\}, \quad (9.93)
```

where \(\gamma \ge 0\) is a tuning parameter and \(\|\mathbf{A}\|_* = \text{tr}[(\mathbf{A}'\mathbf{A})^{1/2}]\) is the nuclear norm of \(\mathbf{A}\). It turns out closed-form solutions are available for both \(\hat{\boldsymbol{\Lambda}}\) and \(\hat{\sigma}^2\). See Wang and Huang (2017, proposition 1) for details. Finally, the analysis discussed in this section can be carried out via the R package SpatPCA.

### Example 9.4

Consider again the ozone measurements of Midwestern United States. In this particular instance, we have \(T = 89\) and \(m = 153\) so that the sample covariance matrix

### First eigenfunction

### Second eigenfunction

![](img/d4c84ae3c2178f4c506af3dd2ed13122_533_0.png)

![](img/d4c84ae3c2178f4c506af3dd2ed13122_533_1.png)

Figure 9.22 Eigenfunctions of the trend-adjusted ozone data of Midwestern United States. Part (a) is the first eigenfunction and part (b) is the second eigenfunction.

is singular and the conventional PCA is not useful. As in Example 9.2, we focus on the trend-adjusted data. Figure 9.22 shows the first two eigenfunctions of the trend-adjusted ozone data. From the plots, it is not surprising to see that the first eigenfunction has large weights along the west shoreline of Lake Michigan in northern Illinois and southern Wisconsin. On the other hand, the second eigenfunction shows certain geographical effect as it shows heavier weights on northern states (Wisconsin, Michigan, Northern Illinois) and lighter weights on southern states (Ohio and Kentucky). Figure 9.23 shows the time plots of the first two principal components. These plots indicate that the components appear to be temporally stationary. Finally, applying the estimated covariance function, we can perform spatial prediction of the ozone data. To demonstrate, we create a new grid points by using 200

![](img/d4c84ae3c2178f4c506af3dd2ed13122_534_0.png)

### Figure 9.24 The first eigenfunction of the spatial prediction of ozone measurements of the Midwestern United States based on regularized spatial PCA.

equally spaced points along the longitude used and 200 equally spaced points along the latitude used. In other words, we have a new grid of \(200 \times 200\) for the Midwestern United States. Figure 9.24 plots the first eigenfunction of the predicted ozone measurements (trend-adjusted). It is reassuring to see that the major concentration of ozone is located near the west shoreline of Lake Michigan near northern Illinois and southern Wisconsin.

### Spatial PCA: SpatPCA package of R.

```
>require(fields)
>require(SpatPCA)
>require(pracma)
>data(zone2)
>x <- ozone2$lon.lat
>Y <- ozone2$y
>date <- as.Date(zone2$date,format='%y%m%d')
>rmma <- !colSums(is.na(Y))
>YY <- matrix(Y[,rmma],nrow=nrow(Y))
>YY <- detrend(YY,"linear")
>xx <- x[rmma,]
>cv <- spatpca(x=xx,Y=YY)
>quilt.plot(xx,cv$eigenfn[,1])
>map("state",xlim=range(xx[,1]),ylim=range(xx[,2]),add=T)
>map.text("state",xlim=range(xx[,1]),ylim=range(xx[,2]),cex=1,add=T)
>title(main="First Eigen Function")
>quilt.plot(xx,cv$eigenfn[,2])
>map("state",xlim=range(xx[,1]),ylim=range(xx[,2]),add=T)
>map.text("state",xlim=range(xx[,1]),ylim=range(xx[,2]),cex=1,add=T)
>title(main="Second Eigen Function")
### Time plots
>par(mfcol=c(2,1))
>plot(date,YY%*%cv$eigenfn[,1],type="l",ylab="1st PCA")
>plot(date,YY%*%cv$eigenfn[,2],type="l",ylab="2nd PCA")
>par(mfcol=c(1,1))
```
