such as dashboard displays and log scanning. This is an intense mental effort. It also would be unnecessary, if a computer program could scan and correlate this data automatically. Operators would save time—often in situations where time is critical—if they could focus on investigating how their system is changing, without first having to identify what is changing.

Before writing a computer program that could accurately perform this kind of change analysis, all of these data points would need to be connected. Logs would need to be linked together so that the transactions could be identified. Metrics would need to be linked to logs so that the statistics being generated could be connected to the transactions they are measuring. And each data point would need to be linked to the underlying system resources—software, infrastructure, and configuration details—so that all events could be connected to a topology of the entire system.

The end result—a single, traversable graph containing all of the data needed to describe the state of a distributed system—is the type of data structure that would give an analysis tool a complete view of the system. Rather than “three pillars” of disconnected data, we would have a single braid of interconnected data.

This is where OpenTelemetry comes in. As illustrated in Figure 1-5, OpenTelemetry is a new telemetry system that generates traces, logs, and metrics in an integrated fashion. All of these connected data points are then transmitted together in the same protocol, which can then be input into a computer program for identifying correlations across the entire data set.

![](bbox=[0.2, 0.7, 0.85, 0.8])

Figure 1-5. OpenTelemetry takes all of this siloed information and connects it as a single, highly structured data stream.

What does this unified data look like? In the next chapter, we’ll set the three pillars aside and build a new mental model from scratch.