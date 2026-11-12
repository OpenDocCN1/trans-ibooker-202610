![](bbox=[0.16, 0.102, 0.845, 0.35])

Figure 7.31 Model checking of a scalar AR(22) model for the square-root transform of daily maximum PM<sub>2.5</sub> measurements of a monitoring station in the Kaohsiung City of Taiwan from 1 January 2006 to 31 December 2015. The data for 29 February were removed.

# TABLE 7.2 Summary Statistics of Out-of-Sample Forecasts of the Square-Root Transform of Daily Maximum PM<sub>2.5</sub> of a Station in Kaohsiung City

| Model | Bias | RMSE | MAE |
|---|---|---|---|
| AR(22)+0 | −0.0991 | 1.145 | 0.886 |
| AR(22)+1 | 0.0778 | 1.015 | 0.783 |
| AR(22)+2 | 0.0135 | 0.985 | 0.760 |
| AR(22)+3 | −0.0319 | 0.949 | 0.731 |
| AR(22)+4 | −0.0045 | 0.917 | 0.697 |
| AR(22)+5 | 0.0096 | 0.913 | 0.693 |
| AR(22)+6 | −0.0050 | 0.884 | 0.683 |

In the Table AR(22)+q denotes the first q hourly data are used in Nowcasting, and Bias, RMSE, and MAE are the average bias, root mean squared forecast error, and mean absolute forecast error, respectively.

```
X <- rbind(X, st1[ist:(ist+5)])
}
m1 <- ar(y)
p <- m1$order
p
ist <- p+1
nT <- 3650
Y <- y[ist:nT]
Z <- NULL
for (i in 1:p) {
Z <- cbind(Z, y[(ist-i):(nT-i)])
}
```