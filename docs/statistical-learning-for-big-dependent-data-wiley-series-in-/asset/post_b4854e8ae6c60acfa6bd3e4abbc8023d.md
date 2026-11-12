
![](img/d4c84ae3c2178f4c506af3dd2ed13122_523_0.png)

**Figure 9.16** Empirical S-T semivariogram of daily maximum temperature from the NOAA data set during August 1993, computed using the command `variogram` in **gstat**. Source: NOAA (National Oceanic and Atmospheric Administration) data set.

For S-T kriging, we need to fit a parametric model to the empirical semivariogram. To this end, we employ a separable covariance function of the form

$$\gamma(\boldsymbol{h}, \tau) = s \times (\bar{\gamma}^{(s)}(\|\boldsymbol{h}\|) + \bar{\gamma}^{(t)}(|\tau|) - \bar{\gamma}^{(s)}(\|\boldsymbol{h}\|)\bar{\gamma}^{(t)}(|\tau|)),$$

where the standardized semivariograms $\bar{\gamma}^{(s)}$ and $\bar{\gamma}^{(t)}$ have separate nugget effects and \(s = 1\) denoting the sill. As a second model, we also employ the covariance function

$$c_{st}(\|v_a\|) = b_1 \exp(-\phi \|v_a\|) + b_2 I(\|v_a\| = 0),$$

where $\boldsymbol{v}_a = (\boldsymbol{h}', a\tau)'$ with the parameter \(a\) denoting the scaling factor used for generating the space-time anisotropy. It turns out that, for this particular instance, the two models provide similar fits. The mean squared error of the separable model is 1.72, whereas that of the anisotropic model is 1.69. Figure 9.17 shows the fitted semivariograms of the two models.

Since we treat the latitude as a covariate, we use universal kriging in the following demonstration. First, we create a space-time grid for prediction. For the spatial grid, we consider 20 spatial locations between 100 °W and 80 °W and 20 spatial locations between 32 °N and 46 °N. For the temporal grid, we consider six equally spaced days in August 1993. The spatial grid and temporal grid are then used to construct an STF object in **gstat** for our space-time prediction grid. Finally, we omit the data on 9 August 1993 to show the capability of S-T kriging to predict across time.

Figure 9.18 shows the predictions and associated standard errors of universal kriging with the fitted separable model. From the plots, the high-standard errors of 9 August are understandable as the data were omitted.

![](img/d4c84ae3c2178f4c506af3dd2ed13122_524_0.png)

Figure 9.17 Fitted semivariograms of parametric models for the daily maximum temperature of NOAA data set during August 1993. The models used are (a) separable model and (b) metric model with anisotropy. Source: NOAA (National Oceanic and Atmospheric Administration) data set.

![](img/d4c84ae3c2178f4c506af3dd2ed13122_524_1.png)

Figure 9.18 S-T kriging predictions of daily maximum temperature using a separable covariance function for August 1993. Part (a) is prediction and part (b) is the associated standard errors. The data of 9 August are omitted.

## R Demonstration: S-T Kriging.

```
> require(sp)
> require(spacetime)
> require(ggplot2)
> require(dplyr)
> require(gstat)
> require(RColorBrewer)
> require(tidyr)
> require(STRbook)
##
> data("STObj3", package = "STRbook")
> STObj4 <- STObj3["1993-08-01::1993-08-31"]
> da <- as.data.frame(STObj4)
> names(da) ## show lon, lat, sp.ID, time, ..., z.
> dim(da) ## show 10168-by-12.
> length(unique(da$id)) ## show 328.
> v <- variogram(object = z ~ 1 + lat, data = STObj4,
     width = 80, cutoff = 1000, tlags = 0.01:6.01)
> plot(v)
> print(v)
> sepVGM <- vgmST(stModel = "separable", space = vgm(10, "Exp", 400, nugget = 0.1),
     time = vgm(10, "Exp", 1, nugget = 0.1), sill = 30)
> sepVGM <- fit.StVariogram(v, sepVGM)
> metricVGM <- vgmST(stModel = "metric",
     joint = vgm(100, "Exp", 400, nugget = 0.1), sill = 20, stAni = 100)
> metricVGM <- fit.StVariogram(v, metricVGM)
> metricMSE <- attr(metricVGM, "optim")$value
> sepMSE <- attr(sepVGM, "optim")$value
> metricMSE
## [1] 1.697554
> sepMSE
## [1] 1.723087
> plot(v, list(sepVGM, metricVGM), main = "Semi-variance")
> spat_pred_grid <- expand.grid(lon = seq(-100, -80, length = 20),
     lat = seq(32, 46, length = 20)) %>%
     SpatialPoints(proj4string = CRS(proj4string(STObj3)))
> gridded(spat_pred_grid) <- TRUE
> temp_pred_grid <- as.Date("1993-08-01") + seq(3, 28, length = 6)
> DE_pred <- STF(spat_pred_grid, time = temp_pred_grid)
> STObj5 <- as(STObj4[, -9], "STIDF")
> STObj5 <- subset(STObj5, !is.na(STObj5$z))
> pred_kriged <- krigeST(z ~ 1 + lat, data = STObj5, newdata = DE_pred,
     modelList = sepVGM, computeVar = TRUE)
> color_pal <- rev(colorRampPalette(brewer.pal(11, "Spectral"))(16))
> stplot(pred_kriged, main = "Predictions (deg Fahrenheit)",
     layout = c(3, 2), col.regions = color_pal)
> pred_kriged$sse <- sqrt(pred_kriged$var1.var)
> stplot(pred_kriged[, , "se"], main = "Prediction std. errors (deg Fahren.)",
     layout = c(3, 2), col.regions = color_pal)
```

## 9.7 DESCRIPTIVE S-T MODELS

A major difficulty in analyzing S-T data is that direct modeling the observed series \( Z_t = [Z(s_1, t), \dots, Z(s_m, t)]' \) would encounter a large number of parameters, especially when the number of locations \( m \) is large. To overcome the difficulty, dimension reduction and/or parameter constraint is often used. It is important, however, in either dimension reduction or parameter constraint to make use of the knowledge on the spatial and temporal structures. The neighborhood system is an example of spatial structure, and seasonal pattern is an example of temporal structure. In this section, we discuss some approaches available in the literature to provide good descriptions of the data.

### 9.7.1 Random Effects with S-T Basis Functions

A widely used approach to spatial-temporal modeling is to employ additive models with random effects. When covariates x(s, t) are available, we can write the model as

$$Y(s, t) = x(s, t)' \beta + \sum_{i=1}^{n_a} \phi_i(s, t) a_i + \nu(s, t), \qquad (9.82)$$

where \(\beta\) denotes the fixed effect of covariates, \(\{\phi_i(s, t) \mid i = 1, \dots, n_a\}\) are prespecified S-T basis functions evaluated at location \(s\) and time \(t\), \(n_a\) is a positive integer, \(\{\alpha_i\}\) are random effects, and \(\nu(s, t)\) is a S-T (residual) process needed to represent small-scale S-T random effects not captured by the basis functions.

Suppose that data are available at \(m\) locations, say \(\{s_1, \dots, s_m\}\). Then, at each time point \(t\), we can express the model in a matrix form as

$$Y_t = X_t \beta + \Phi \alpha + \nu_t, \qquad (9.83)$$

where \(Y_t = [Y(s_1, t), \dots, Y(s_m, t)]'\), \(\Phi\) is an \(m \times n_a\) matrix with \(i\)th row being \([\phi_1(s_i, t), \dots, \phi_{n_a}(s_i, t)]\), \(\alpha = (\alpha_1, \dots, \alpha_{n_a})'\), and \(\nu_t\) is defined in the same location ordering as \(Y_t\). A special feature of the model is that both \(\beta\) and \(\alpha\) are both space and time invariant. Typically, we assume that \(E(Y(s, t) \mid x_t) = x_t'\beta\) and the random effects \(\alpha\) follows the distribution \(N(0, \Sigma_a)\). Furthermore, under the assumption that (a) \(\nu(s, t)\) has no temporal dependence and (b) \(\nu_t \sim N(0, \Sigma_v)\). Then, we have \(Y_t \sim N(X_t \beta, \Sigma_{y,t})\), where \(\Sigma_{y,t} = \Phi_t \Sigma_a \Phi_t' + \Sigma_v\).

For the observed data with measurement errors \(Z(s, t) = Y(s, t) + \epsilon(s, t)\), we have \(Z_t \sim N(X_t \beta, \Sigma_{z,t})\), where \(\Sigma_{z,t} = \Sigma_{y,t} + \Sigma_\epsilon = \Phi_t \Sigma_a \Phi_t' + \Sigma_v + \Sigma_\epsilon\). If \(n_a \ll m\), which is referred to as a row-rank representation, the model in Eq. (9.82) is relatively easy to estimate as its likelihood function can be evaluated effectively with the matrix inversion formula

$$\Sigma_{z,t}^{-1} = V^{-1} - V^{-1} \Phi_t (\Phi_t' V^{-1} \Phi_t + \Sigma_a^{-1})^{-1} \Phi_t' V^{-1},$$

where \(V = \Sigma_v + \Sigma_\epsilon\). This is particularly so if both \(\Sigma_v\) and \(\Sigma_\epsilon\) follow some simple structures, e.g. diagonal matrices.

Suppose that the data are observed at \(t = 1, \dots, T\). Then, we can put all the data in a matrix form

$$Z = X \beta + \Phi \alpha + \nu + \epsilon,$$

where \(Z = [Z_1', \dots, Z_T']'\) is a \(mT\)-dimensional vector, and \(X\) and \(\Phi\) are stacked matrices of \(X_t\) and \(\Phi_t\) for \(t = 1, \dots, T\) with the same ordering as \(Z\).

For the \(m = n_a\) (full-rank) and \(n_a > m\) (over-complete) cases, the model can still enjoy some computational benefits if one introduces sparsity in the covariance matrix \(\Sigma_a\) of the random effects.

The choices of basis functions and the number of basis functions play a role in applying the model in Eq. (9.83). There are many basis functions available in the literature. See, for instance, the basis functions available in the R packages FRK of
