
## How We Actually Observe Our Systems in the Real World

As a refresher, let’s go through the actual, unvarnished process that operators go through when they investigate a problem.

Investigation involves two steps: noticing that something has happened, and then determining what caused it to happen.

When we perform these tasks, we use our observability tools. But, importantly, we don’t use each tool in isolation. We use all of them together. And all along the way, the siloed nature of these tools puts an enormous cognitive load on the operator.

Often, the investigation starts when someone notices that an important metric has gone all squiggly. In this case, “squiggly” is an important term of art, as often the only information an operator has at this point is the shape of a tiny line on a dashboard and their own internal assessment as to whether the shape of that line looks “right” or not (Figure 1-2).

![](img/4e6e799bc9cb3e86f203595f40542979_14_0.png)

Having determined the point where the shape of the line started to look “off,” the operator would then squint and try to find other lines on the dashboard that went “squiggly” at the same time. However, because these metrics are completely independent of each other, the operator must do the comparison in their brain, without help from the computer.

Needless to say, staring at charts in hope of finding a useful correlation takes time and brainpower, not to mention it leads to eyestrain.

Personally, I’d use a ruler or a piece of paper and just look at what lined up. In “modern” dashboards, the ruler is now a line drawn as part of the UI. But this is just as crude a solution. The real work of identifying correlations must still occur in the operator’s head, again with no help from the computer.

Having gained an initial, rough guess about the problem, the operator usually begins to investigate transactions (logs) and resources (machines, processes, configuration files) they believe may be associated with the problem (Figure 1-3).

![](img/4e6e799bc9cb3e86f203595f40542979_15_0.png)

Figure 1-3. Finding the logs associated with an anomaly is also a terrible experience.

Here, again, the computer is no real help. The logs are stored in a completely separate system and cannot be automatically associated with any metrics dashboard. Configuration files and other service-specific information are often in no system, and operators must SSH or otherwise access running machines to look at them.

Therefore, the operator is once again left to do the job of finding correlations, this time between metrics and the relevant logs. Identifying these logs can be difficult; often the source code must be consulted to even get an idea of what logs might be present.

When a (possibly, hopefully) relevant log is found, the next step is usually to determine the chain of events that caused this log to be generated. This means finding the rest of the logs in the same transaction.

Once again, a lack of correlations puts a huge burden on the operator. Unstructured and semistructured logging systems do not have a mechanism for automatically indexing and filtering logs by transaction. Even though this is by far the most common logging workflow that operators perform, they are left to perform an ad hoc series of queries and filters to winnow down the available logs into a subset that, hopefully, represents an approximation of the transaction. To even have a chance of success, they must lean on application developers to add various request IDs and other breadcrumbs to find later and stitch together.

In a small system, this process of reconstructing transactions is tedious but possible. But once a system grows to include many horizontally scaled services, the amount of time it takes to reconstruct a single transaction begins to seriously limit the scope of any investigation. Figure 1-4 shows a complex transaction that touches many services. How would you collect all of the logs?

![](img/4e6e799bc9cb3e86f203595f40542979_16_0.png)

Figure 1-4. With traditional logging, finding the exact logs that make up a specific transaction can take a lot of effort. Once systems become large enough, it becomes almost impossible.

One great answer is distributed tracing, which actually has all of the IDs and indexing tools needed to reconstruct a transaction automatically. Unfortunately, tracing systems are often seen as a niche tool for latency analysis. As a result, they are sent comparatively little logging data. And because they are focused on latency analysis, tracing systems are often heavily sampled, rendering them irrelevant for these kinds of investigations.

## Not Three Pillars, but a Single Braid

Needless to say, this is a bummer. The above workflow really does represent a terrible state of affairs. But because we’ve been living with this technology regime for so long, we often don’t recognize how inefficient it actually is, compared to what it could be.

Today, to understand how their systems are changing, operators must first gather large amounts of data. Then they must use their minds to identify correlations in that data, based on visual stimuli such as dashboard displays and log scanning. This is an intense mental effort. It also would be unnecessary, if a computer program could scan and correlate this data automatically. Operators would save time—often in situations where time is critical—if they could focus on investigating how their system is changing, without first having to identify what is changing.

Before writing a computer program that could accurately perform this kind of change analysis, all of these data points would need to be connected. Logs would need to be linked together so that the transactions could be identified. Metrics would need to be linked to logs so that the statistics being generated could be connected to the transactions they are measuring. And each data point would need to be linked to the underlying system resources—software, infrastructure, and configuration details—so that all events could be connected to a topology of the entire system.

The end result—a single, traversable graph containing all of the data needed to describe the state of a distributed system—is the type of data structure that would give an analysis tool a complete view of the system. Rather than “three pillars” of disconnected data, we would have a single braid of interconnected data.

This is where OpenTelemetry comes in. As illustrated in Figure 1-5, OpenTelemetry is a new telemetry system that generates traces, logs, and metrics in an integrated fashion. All of these connected data points are then transmitted together in the same protocol, which can then be input into a computer program for identifying correlations across the entire data set.

![](img/4e6e799bc9cb3e86f203595f40542979_17_0.png)

Figure 1-5. OpenTelemetry takes all of this siloed information and connects it as a single, highly structured data stream.

What does this unified data look like? In the next chapter, we’ll set the three pillars aside and build a new mental model from scratch.

## The Value of Structured Data

Squinting at graphs is not the best way to hunt for correlations. A huge amount of work currently done inside the heads of operators can actually be automated. This frees operators to move quickly among identifying issues, making hypotheses, and validating root causes.

To build better tools, we need better data. Telemetry must have two qualities to support high-quality automated analysis:

- All data points must be connected in a graph with proper indexing.
- All data points that represent common operations must have well-defined keys and values.

In this chapter, we will walk through a modern telemetry data model, starting with a basic building block: the attribute.

### Attributes: Defining Keys and Values

The most fundamental data structure is an *attribute*, which is defined as a key and a value. Every data structure in OpenTelemetry contains a list of attributes. Every component of a distributed system (an HTTP request, a SQL client, a serverless function, a Kubernetes Pod) is defined in the OpenTelemetry specification as a specific set of attributes. These definitions are called the OpenTelemetry semantic conventions. Table 2-1 shows a partial list of the HTTP conventions.
