Personally, I’d use a ruler or a piece of paper and just look at what lined up. In “modern” dashboards, the ruler is now a line drawn as part of the UI. But this is just as crude a solution. The real work of identifying correlations must still occur in the operator’s head, again with no help from the computer.

Having gained an initial, rough guess about the problem, the operator usually begins to investigate transactions (logs) and resources (machines, processes, configuration files) they believe may be associated with the problem (Figure 1-3).

![](bbox=[0.15, 0.38, 0.85, 0.6])

Figure 1-3. Finding the logs associated with an anomaly is also a terrible experience.

Here, again, the computer is no real help. The logs are stored in a completely separate system and cannot be automatically associated with any metrics dashboard. Configuration files and other service-specific information are often in no system, and operators must SSH or otherwise access running machines to look at them.

Therefore, the operator is once again left to do the job of finding correlations, this time between metrics and the relevant logs. Identifying these logs can be difficult; often the source code must be consulted to even get an idea of what logs might be present.

When a (possibly, hopefully) relevant log is found, the next step is usually to determine the chain of events that caused this log to be generated. This means finding the rest of the logs in the same transaction.

Once again, a lack of correlations puts a huge burden on the operator. Unstructured and semistructured logging systems do not have a mechanism for automatically indexing and filtering logs by transaction. Even though this is by far the most common logging workflow that operators perform, they are left to perform an ad hoc series of queries and filters to winnow down the available logs into a subset that, hopefully, represents an approximation of the transaction. To