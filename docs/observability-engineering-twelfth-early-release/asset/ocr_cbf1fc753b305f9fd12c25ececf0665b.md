In comparison, BI tools often collect and process large amounts of unstructured data into structured, queryable form. BI data warehouses would be an ungovernable mess without structures and predefined schemas. You need consistent schemas in order to perform any kind of useful analysis over time. And BI workloads tend to ask similar questions in repeatable ways to power things like dashboards. BI data can be optimized with indexes, compound indexes, summaries, etc.

Because BI data warehouses are designed to grow forever, it is important that they have predefined schemas and grow at a predictable rate. Observability data is designed for rapid feedback loops and flexibility: it is typically most important under times of duress, when predictability is far less important than immediacy.

# ## Time Windows

Observability and BI tools both have the concept of a session, or trace. But observability tends to be limited to a time span measured in seconds, or minutes at most. BI tools can handle long-term journeys, or traces that can take days or weeks to complete. That type of trace longevity is not a use case typically supported by observability tools. With observability tools, longer-running processes (like import/export jobs or queues) are typically handled with a polling or heartbeat process, and not with a single trace.

# ## Ephemerality

Summarizing many of these points, debugging data is inherently more ephemeral than business data. You might likely need to retrieve a specific transaction record or billing record from two years ago with total precision, for example. In contrast, you are unlikely to need to know if the latency between service A and service B was high for a particular user request two years ago.

You may, however, want to know if the latency between service A and service B has increased over the last year or two, or if the 95% percentile latency has gone up over that time. That type of question is common, and it