```
runningTotal += tm.Total[t]
runningFails += tm.Fails[t]

// If we're within the projection window, use this value to project forward, counting it an extra lookbackRatio times.
value to project forward, counting it an extra lookbackRatio times.

if t.After(pWindow) {
    projectedTotal += lookbackRatio * tm.Total[t]
    projectedFails += lookbackRatio * tm.Fails[t]
}
}

projectedTotal = projectedTotal + runningTotal
projectedFails = projectedFails + runningFails

allowedFails := projectedTotal * int64(slo.BudgetPPM) / int64(1e6)
return projectedFails != 0 && projectedFails >= allowedFails
}
```

To calculate an answer to the example problem, you note that in the past 26 days, there have already been 37,960 units, and 285 failed. The baseline window of 1 day will be used to project forward 4 days. At the start of the baseline window, you are at day 25 of the data set (1 day ago).

Visualizing results as a graph, as shown in Figure 13-7, you see that when the one-day baseline window started, you still had 65% of your error budget remaining. Right now, you have only 35% of your error budget remaining. In one day, you’ve burned 30% of your error budget. If you continue to burn your error budget at that rate, it will be exhausted in far less than four days. You should trigger an alarm.