![](bbox=[0.283, 0.1, 0.715, 0.335])

**Figure 9.16** Empirical S-T semivariogram of daily maximum temperature from the NOAA data set during August 1993, computed using the command `variogram` in **gstat**. Source: NOAA (National Oceanic and Atmospheric Administration) data set.

For S-T kriging, we need to fit a parametric model to the empirical semivariogram. To this end, we employ a separable covariance function of the form

$$\gamma(\boldsymbol{h}, \tau) = s \times (\bar{\gamma}^{(s)}(\|\boldsymbol{h}\|) + \bar{\gamma}^{(t)}(|\tau|) - \bar{\gamma}^{(s)}(\|\boldsymbol{h}\|)\bar{\gamma}^{(t)}(|\tau|)),$$

where the standardized semivariograms $\bar{\gamma}^{(s)}$ and $\bar{\gamma}^{(t)}$ have separate nugget effects and \(s = 1\) denoting the sill. As a second model, we also employ the covariance function

$$c_{st}(\|v_a\|) = b_1 \exp(-\phi \|v_a\|) + b_2 I(\|v_a\| = 0),$$

where $\boldsymbol{v}_a = (\boldsymbol{h}', a\tau)'$ with the parameter \(a\) denoting the scaling factor used for generating the space-time anisotropy. It turns out that, for this particular instance, the two models provide similar fits. The mean squared error of the separable model is 1.72, whereas that of the anisotropic model is 1.69. Figure 9.17 shows the fitted semivariograms of the two models.

Since we treat the latitude as a covariate, we use universal kriging in the following demonstration. First, we create a space-time grid for prediction. For the spatial grid, we consider 20 spatial locations between 100 °W and 80 °W and 20 spatial locations between 32 °N and 46 °N. For the temporal grid, we consider six equally spaced days in August 1993. The spatial grid and temporal grid are then used to construct an STF object in **gstat** for our space-time prediction grid. Finally, we omit the data on 9 August 1993 to show the capability of S-T kriging to predict across time.

Figure 9.18 shows the predictions and associated standard errors of universal kriging with the fitted separable model. From the plots, the high-standard errors of 9 August are understandable as the data were omitted.