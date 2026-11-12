```
#active bounds at final generalized Cauchy point 0
#norm of the final projected gradient 1.1482
#final function value 5602.96
```

```
#F = 5602.96
#final value 5602.958497
#converged
```

```
### Prediction
>x.final <- coef(SpatioTemporalfit1,pars="cov")$par
>spat_pred_grid <- expand.grid(lon=seq(-100, -80, length=20),
lat=seq(32,46,length=20))
>spat_pred_grid$id <- 1:nrow(spat_pred_grid)
>temp_pred_grid <- as.Date("1993-08-01")+seq(3,28,length=6)
>obs_pred_wide <- matrix(0,6,400)
>rownames(obs_pred_wide) <- as.character(temp_pred_grid)
>colnames(obs_pred_wide) <- spat_pred_grid$id
>covars_pred <- spat_pred_grid
>STdata_pred <- createSTdata(obs=obs_pred_wide,covars=covars_pred)
>E <- predict(STmodel,x.final,STdata=STdata_pred)
>names(E)
### Level plot for predictions and prediction errors of August 14, 1993
>t1 <- E$EX
>t2 <- E$VX.pred
>E1 <- matrix(t1[3,],20,20)
>E2 <- matrix(sqrt(t2[3,]),20,20)
>k1 <- seq(32,46,length=20)
>k2 <- seq(-100,-80,length=20)
>colnames(E2) <- colnames(E1) <- paste(round(k1,2))
>rownames(E2) <- rownames(E1) <- paste(round(k2,2))
>require(lattice)
### define color palette
>color_pal <- rev(colorRampPalette(brewer.pal(11,"Spectral"))(16))
>levelplot(E1,col.regions=color_pal,xlab="lon (deg) ",ylab="lat (deg) ",
main="Predictions (deg Fahrenheit) ")
>levelplot(E2,col.regions=color_pal,xlab="lon (deg) ",ylab="lat (deg) ",
main="Prediction errors (deg Fahrenheit) ")
```

Our discussion focuses on Gaussian distributions. There are many non-Gaussian S-T data exist in empirical applications. For instance, S-T count data, such as the number of cases of a particular disease in a given region within a specified time period, are common in epidemiological studies. The analysis we discussed for Gaussian data can be generalized to model non-Gaussian S-T data. Interested readers are referred to Cressie and Wikle (2011) and Diggle (2013).

# 9.8 DYNAMIC S-T MODELS

In this section, we consider dynamic S-T statistical models. Given the complexity involved, there are many such models available in the literature and it would not be possible to discuss them all here. Instead, we only provide some general models that can be tailored to specific needs for a given application.