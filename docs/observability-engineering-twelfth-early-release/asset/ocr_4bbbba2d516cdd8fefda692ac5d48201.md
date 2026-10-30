# Query Execution Time

Observability tools need to be fast, with queries ranging from subsecond to low seconds. A key tenet of observability is explorability, since you don’t always know what you’re looking for. You spend less time running the same queries over and over, and more time following a trail of breadcrumbs. During an investigative flow, disruptions when you have to sit and wait for a minute or longer for results can mean you lose your entire train of thought.

In comparison, BI tools are often optimized to run reports, or to craft complex queries that will be used again and again. It’s OK if these take longer to run, because this data isn’t being used to react in real time, but rather to feed into other tools or systems. You typically make decisions about steering the business over time measured in weeks, months, or years, not minutes or seconds. If you’re making strategic business decisions every few seconds, something has gone terribly wrong.

# Accuracy

For observability workloads, if you have to choose, it’s better to return results that are fast as opposed to perfect (as long as they are very close to correct). For iterative investigations, you would almost always rather get a result that scans 99.5% of the events in one second than a result that scans 100% in one minute. This a real and common trade-off that must be made in massively parallelized distributed systems across imperfect and flaky networks (see Chapter 16).

Also (as covered in Chapter 17), some form of dynamic sampling is often employed to achieve observability at scale. Both of these approaches trade a slight bit of accuracy for massive gains in performance. When it comes to BI tools and business data warehouses, both sampling and a “close to right” approach are typically verboten. When it comes to billing, for example, you will always want the accurate result no matter how long it takes.

# Recency