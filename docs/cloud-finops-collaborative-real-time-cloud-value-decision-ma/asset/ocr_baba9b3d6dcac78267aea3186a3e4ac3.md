# Combining metrics

To determine a performance metric for your commitments, you measure how much of a commitment is being used versus not being used over a specific time period. This is called your commitment utilization. At an individual commitment level you monitor by time, measuring the proportion of hours (or seconds) that a commitment applied a discount to your resource usage versus the number of hours (or seconds) you didn’t have any usage for the commitment to discount.

However, if you roll your commitment utilization up to show one overall performance metric, the individual underperforming commitments can be hidden. Instead, you measure individual commitments and alert when they individually underperform. Let’s look at an example of this in Figure 22-4.

![](bbox=[0.13, 0.36, 0.85, 0.55])

Figure 22-4. Commitment usage over time

You use the usage in Figure 22-4 to determine your commitment utilization, based on having seven commitments. You can see that the first four commitments are utilized 100%. The fifth commit will have 90% utilization and the sixth will have 80% utilization, with the last commitment at only 50%.

If you just average out the utilizations (100% + 100% + 100% + 100% + 90% + 80% + 50%) / 7, you get 88.5% utilized. An overall utilization rate of 88% on the surface appears pretty decent and wouldn’t raise concerns. Had you set a target utilization of 80%, MDCO would reflect no changes needed. But remember: there’s an individual commitment achieving only 50% utilization, and that deserves your attention and possible modification.

# Data Driven

For years, teams have been operating their services using metrics to monitor items like service performance, resource usage, and request rates as an early warning