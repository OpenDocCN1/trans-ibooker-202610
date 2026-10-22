```
.... .... .... .... .... .... ....
315 2017 10 116 13.666667 Cleistogamous 0
316 2017 10 128 14.100000 Chasmogamous 0
317 2017 10 128 14.100000 Cleistogamous 18
318 2017 10 143 14.550000 Chasmogamous 0
319 2017 10 143 14.550000 Cleistogamous 9
```

320 rows × 6 columns

The rows shown display some of the data from the first and tenth plots in this experiment, in which the researchers studied a native population of downy yellow violets in a local public park. To understand the impact of environmental cues on flowering, they randomly selected and then monitored 10 plots of equal area, measuring ambient conditions and counting how many buds appeared on the violets in each plot. This particular violet species benefits from a mixed breeding system consisting of 2 distinct bud types: *chasmogamous buds* that mature into flowers that are usually fertilized by pollinators like bees, and *cleistogamous buds* that become self-pollinating flowers. These bud types appear at different times, with chasmogamous buds appearing in the spring and cleistogamous buds appearing in the summer and early autumn. The data above corroborate this, as 2 chasmogamous buds and 0 cleistogamous buds appeared in plot 1 on the first 5 days that measurements were taken.

For now, we’re going to investigate the influence of the *photoperiod*, the number of hours per day that a plot of violets received sunlight, on chasmogamous bud formation. Although it’s not clear from the first five rows of the dataset, we can tell from the last three that Bud counts records the total number of buds observed in a plot and is not binary. We therefore need to filter the data and transform Bud counts into a binary response variable—we’ll define the new variable Bud to equal 1 if Bud count is positive and to equal 0 otherwise. Once Bud is ready to go, we can plot its dependence on Photoperiod:

```
chasm = violets[ violets['Bud type']=="Chasmogamous" ]
chasm["Bud"] = (violets['Bud counts'] > 0).astype('int')
chasm = chasm[["Photoperiod", "Bud"]]
chasm
```

Photoperiod Bud
1 12.833333 1