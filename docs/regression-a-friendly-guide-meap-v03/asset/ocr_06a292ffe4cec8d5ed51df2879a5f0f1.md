```
cohorts["Null deviance"] = (2*
    (cohorts["Successes"]*np.log( cohorts["Percentage"]/pi_null) +
    cohorts["Failures"]*np.log((1 - cohorts["Percentage"])/(1 - pi
cohorts
```

| Bias | Age | Number | Successes | Failures | Percentage | Null deviance |
| 0 | 1 | 0 | 796 | 21 | 775 | 0.026382 | 1161.79 |
| 1 | 1 | 1 | 505 | 32 | 473 | 0.063366 | 610.16 |
| 2 | 1 | 2 | 320 | 29 | 291 | 0.090625 | 337.81 |
| 3 | 1 | 3 | 258 | 37 | 221 | 0.143411 | 208.65 |
| 4 | 1 | 4 | 299 | 30 | 269 | 0.100334 | 300.78 |
| ... | ... | ... | ... | ... | ... | ... | ... |
| 96 | 1 | 96 | 13 | 12 | 1 | 0.923077 | 7.87 |
| 97 | 1 | 97 | 11 | 10 | 1 | 0.909091 | 6.02 |
| 98 | 1 | 98 | 6 | 5 | 1 | 0.833333 | 1.81 |
| 99 | 1 | 99 | 2 | 1 | 1 | 0.500000 | 0.04 |
| 100 | 1 | 100 | 6 | 5 | 1 | 0.833333 | 1.81 |

101 rows x 7 columns

```
np.sum( cohorts["Null deviance"])
```

9369.377128605302

Perfect! Now for the residual deviance:

```
cohorts["Probability"] = cohorts_fit.predict()
cohorts["Deviance"] = (2*(
    cohorts["Successes"]*np.log(
        cohorts["Percentage"]/cohorts["Probability"] ) +
    cohorts["Failures"]*np.log(
        (1 - cohorts["Percentage"])/(1 - cohorts["Probability"])
cohorts[["Age","Percentage","Null deviance","Probability","Devian
```

| Age | Percentage | Null deviance | Probability | Deviance |
| 0 | 0 | 0.026382 | 1161.794446 | 0.115130 | 86.263867 |
| 1 | 1 | 0.063366 | 610.161711 | 0.120514 | 18.413977 |
| 2 | 2 | 0.090625 | 337.812295 | 0.126115 | 4.001841 |
| 3 | 3 | 0.143411 | 208.652149 | 0.131937 | 0.289567 |
| 4 | 4 | 0.100334 | 300.788097 | 0.137986 | 3.881705 |
| ... | ... | ... | ... | ... | ... |
| 96 | 96 | 0.923077 | 7.875780 | 0.949544 | 0.164944 |
| 97 | 97 | 0.909091 | 6.023549 | 0.951969 | 0.354287 |
| 98 | 98 | 0.833333 | 1.815906 | 0.954284 | 1.231814 |
| 99 | 99 | 0.500000 | 0.047729 | 0.956492 | 3.585992 |
| 100 | 100 | 0.833333 | 1.815906 | 0.958598 | 1.384948 |