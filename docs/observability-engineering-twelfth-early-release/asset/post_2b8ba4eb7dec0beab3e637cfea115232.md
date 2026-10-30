
Using *proportional extrapolation*, you would calculate that in any given 24-hour period, you would expect to see 43,800 units / 30 days, or 1,460 units. In the last 6 hours, 25 of 50 units (50%) failed. If that proportional failure rate of 50% continues for the next 24 hours, you will burn 50% of 1,460 units, or 730 units—far more than your budget of 438 units. With this proportional calculation, you can reasonably project that your error budget will be exhausted in about half a day. An alert should trigger to notify an on-call engineer to investigate immediately.

## Context-aware burn alerts

When using *context-aware*, or *historical*, burn alerts, you keep a rolling total of the number of good and bad events that have happened over the entire window of the SLO, rather than just the baseline window. This section unpacks the various calculations you need to make for effective burn alerts. But you should tread carefully when practicing these techniques. The computational expense of calculating these values at each evaluation interval can quickly become financially expensive as well. At Honeycomb, we found this out the hard way when small SLO data sets suddenly started to rack up over $5,000 of AWS Lambda costs per day. :-)

To see how the considerations are different for context-aware burn alerts, let’s work an example. Say you have a service with an SLO target indicating 99% of units will succeed over a moving 30-day window. In a typical month, the service sees 43,800 units. *In the previous 26 days, you have already failed 285 units out of 37,960. In the past 24 hours, the service has seen 1,460 units, 130 of which failed.* To achieve the 99% target, only 1% of units are allowed to fail per month. Based on typical traffic volume (43,800 units), only 438 units are allowed to fail (your error budget). You want to know if, at this rate, you will burn your error budget in the next four days.

In this example, you want to project forward on a scale of days. Using the maximum practical extrapolation factor of 4, as noted previously, you set a baseline window that examines the last one day’s worth of data to extrapolate forward four days from now.

You must also consider the impact that your chosen scale has on your sliding window. If your SLO is a sliding 30-day window, your adjusted lookback window would be 26 days: 26 lookback days + 4 extrapolated days = your 30-day sliding window, as shown in Figure 13-6.

![](img/aaaa2d27e08e754554d501192f304d8f_219_0.png)

*Figure 13-6. An adjusted SLO 30-day sliding window. When projecting forward 4 days, the lookback period to consider must be shortened to 26 days before today. The projection is made by replicating results from the baseline window for the next 4 days and adding those to the adjusted sliding window.*

With those adjusted timeframes defined, you can now calculate how the future looks four days from now. For that calculation, you would do the following:

1. Examine every entry in the map of SLO events that has occurred in the past 26 days.
2. Store both the total number of events in the past 26 days and the total number of errors.
3. Reexamine map entries that occurred within the last 1 day to determine the baseline window failure rate.
4. Extrapolate the next 4 days of performance by presuming they will behave similarly to the 1-day baseline window.
5. Calculate the adjusted SLO 30-day sliding window as it would appear 4 days from now.
6. Trigger an alert if the error budget would be exhausted by then.

Using a Go-based code example, you could accomplish this task if you had a given time-series map containing the number of failures and successes in each window:

```go
func isBurnViolation(now time.Time, ba *BurnAlertConfig, slo *SLO, tm *timeseriesMap) bool {
	// For each entry in the map, if it's in our adjusted
	// window, add its totals.
	// If it's in our projection window, store the rate.
	// Then project forward and do the SLO calculation.
	// Then fire off alerts as appropriate.

	// Compute the window we will use to do the projection,
	// with the projection offset as the earliest bound.
	pOffset :=
		time.Duration(ba.ExhaustionMinutes/lookbackRatio) * time.Minute
	pWindow := now.Add(-pOffset)

	// Set the end of the total window to the beginning of the
	// SLO time period, plus ExhaustionMinutes.
	tWindow := now.AddDate(0, 0, -slo.TimePeriodDays).Add(
		time.Duration(ba.ExhaustionMinutes) * time.Minute)

	var runningTotal, runningFails int64
	var projectedTotal, projectedFails int64
	for i := len(tm.Timestamps) - 1; i >= 0; i-- {
		t := tm.Timestamps[i]

		// We can stop scanning backwards if we've run off the end.
		if t.Before(tWindow) {
			break
		}

		runningTotal += tm.Total[t]
		runningFails += tm.Fails[t]

		// If we're within the projection window, use this value to project forward,
		// counting it an extra lookbackRatio times.
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

![](img/aaaa2d27e08e754554d501192f304d8f_222_0.png)

*Figure 13-7. One day ago, 65% of the error budget remained. Now, only 35% of the error budget remains. At the current rate, the error budget will be exhausted in less than two days.*

### The Baseline Window

Having considered the lookahead window and the way it’s used, let’s now shift our attention back to the baseline window.

There’s an interesting question of how *much* of a baseline to use when extrapolating forward. If the performance of a production service suddenly turns downhill, you want to be aware of that fairly quickly. At the same time, you also don’t want to set such a small window that it creates noisy alerts. If you used a baseline of 15 minutes to extrapolate three days of performance, a small blip in errors could trigger a false alarm. Conversely, if you used a baseline of three days to extrapolate forward an hour, you’d be unlikely to notice a critical failure until it’s too late.

We previously described a practical choice as a factor of four for the timeline being considered. You may find that other more sophisticated algorithms can be used, but our experience has indicated that the factor of four is generally reliable. The baseline multiplier of 4 also gives us a handy heuristic: a 24-hour alarm is based on the last 6 hours of data, a 4-hour alarm is based on the last 1 hour of data, and so forth. It’s a good place to start when first approaching SLO burn alerts.

An odd implication when setting baseline windows proportionally is also worth mentioning. Setting multiple burn alerts on an SLO by using different lookahead window sizes is common. Therefore, it becomes possible for alarms to trigger in seemingly illogical order. For example, a system issue might trigger a burn alert that says you’ll exhaust your budget within two hours, but it won’t trigger a burn alert that says you’ll exhaust it within a day. That’s because the extrapolation for those two alerts comes from different baselines. In other words, if you keep burning at the rate from the last 30 minutes, you’ll exhaust in two hours—but if you keep burning at the rate from the last day, you’ll be fine for the next four days.

For these reasons, you should measure both, and you should act whenever either projection triggers an alert. Otherwise, you risk potentially waiting hours to surface a problem on the burn-down timescale of one day—when a burn alert with hour-level granularity means you could find out sooner. Launching corrective action at that point could also avert triggering the one-
