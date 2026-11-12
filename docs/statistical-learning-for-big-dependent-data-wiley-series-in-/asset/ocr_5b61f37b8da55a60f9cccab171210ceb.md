![](bbox=[0.15, 0.09, 0.85, 0.32])

Figure 9.7 Hovmöller plots for both the longitude (left) and latitude (right) coordinates for the daily maximum temperature (°F) of the NOAA data set from 1 May to 30 September 1993. Source: NOAA (National Oceanic and Atmospheric Administration) data set.

properties. It also introduces variogram and three types of kriging. Section 9.4 discusses lattice processes whereas Section 9.5 introduces spatial point processes. Section 9.6 studies S-T processes and extends the spatial methods to S-T methods. Section 9.8 provides various descriptive models for S-T data, including use of S-T basis functions and spatial principal component analysis. Finally, Section 9.9 briefly discusses dynamic S-T models. Some R commands for plotting spatial or S-T data are given in Appendix 9.A.

# 9.2 SPATIAL PROCESSES AND DATA ANALYSIS

A useful statistical framework for spatial data analysis is the hierarchical model (HM). See Cressie and Wikle (2011). Under the HM framework, the data are observed from the underlying spatial process with noises. The spatial process then depends on some covariates with some unknown parameters and follows a well-specified distribution function. Statistical inference can then be made by Bayesian analysis or the conventional maximum likelihood approach. See below for more details.

Let \(D \subset R^d\) be the region in \(R^d\) of interest and the spatial process is \(\{Y(\boldsymbol{s}) | \boldsymbol{s} \in D \}\). The region \(D\) is referred to as the domain of \(Y(\boldsymbol{s})\). Assume that noisy observations are observed at known locations \(\{\boldsymbol{s}_1, \dots, \boldsymbol{s}_m \}\) and the data model is

```
Z(\boldsymbol{s}_i) = Y(\boldsymbol{s}_i) + \epsilon(\boldsymbol{s}_i), \quad i = 1, \dots, m, \qquad (9.1)
```

where \(\epsilon(\boldsymbol{s})\) is a white noise process with \(\boldsymbol{s} \in D\) and is independent of \(Y(\boldsymbol{s})\). As usual, by white noise, we have \(E[\epsilon(.)] = 0\) and \(\text{Var}[\epsilon(.)] = \sigma_\epsilon^2 > 0\) and \(\text{Cov}[\epsilon(\boldsymbol{s}_1), \epsilon(\boldsymbol{s}_2)] = 0\) if \(\boldsymbol{s}_1 \neq \boldsymbol{s}_2\). The \(\epsilon(\boldsymbol{s})\) process can be considered as a measurement error. Let \(\boldsymbol{Z} = \{ z(\boldsymbol{s}_i) | i = 1, \dots, m \}\) be the data collection. In geostatistics, an important problem is to obtain an optimal spatial predictor of \(Y(\boldsymbol{s}_0)\) at a known location \(\boldsymbol{s}_0\) based on the squared error